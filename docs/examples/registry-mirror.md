# Example: Set up a registry mirror

Use-cases:

* Increase speed of pulls and builds by caching images on Actuated Servers
* Reduce failed builds due to rate-limiting

If you use Docker in your self-hosted builds, there is a chance that you'll run into the rather conservative rate-limits.

The Docker Hub allows for 100 image pulls within a 6 hour period, but this can be extended to 200 by logging in, or to 5000 by paying for a Pro license.

A registry mirror / pull-through cache running on an actuated agent is significantly faster than pulling from a remote server.

We will create a mirror that:

* Has no authentication, to keep the changes to your build to a minimum
* Cannot support PUSH / PUT / DELETE events
* Only has access to pull images from the Docker Hub
* Is not exposed to the Internet, but only to Actuated VMs
* When unavailable for any reason, the build continues without error
* Works on both Intel/AMD and ARM64 hosts

This tutorial shows you how to set up what was previously known as "Docker's Open Source Registry" and is now a CNCF project called [distribution](https://github.com/distribution/distribution).

## Create a Docker Hub Access token

Create a Docker Hub Access token with "Public repos only" scope, and save it as `~/hub.txt` on the Actuated Server.

![Settings for a public token](/images/read-only-public-token.png)

> Settings for an authorization token, with read-only permissions to public repositories

## Set up the registry on an actuated agent

```bash
curl -sLS https://get.arkade.dev | sudo sh

sudo arkade system install registry

sudo mkdir -p /etc/registry
sudo mkdir -p /var/lib/registry
```

Create a config file to make the registry only available on the Linux bridge for Actuated VMs:

```bash
export TOKEN=$(cat ~/hub.txt)
export USERNAME=""

cat >> /tmp/registry.yml <<EOF
version: 0.1
log:
  accesslog:
    disabled: true
  level: warn
  formatter: text

storage:
  filesystem:
    rootdirectory: /var/lib/registry

proxy:
  remoteurl: https://registry-1.docker.io
  username: $USERNAME

  # A Docker Hub Personal Access token created with "Public repos only" scope
  password: $TOKEN

http:
  addr: 192.168.128.1:5000
  relativeurls: false
  draintimeout: 60s
EOF

sudo mv /tmp/registry.yml /etc/registry/config.yml
```

Install and start the registry with a systemd unit file:

```bash
cat >> /tmp/registry.service <<EOF
[Unit]
Description=Registry
After=network.target

[Service]
Type=simple
Restart=always
RestartSec=5s
ExecStart=/usr/local/bin/registry serve /etc/registry/config.yml

[Install]
WantedBy=multi-user.target
EOF


sudo mv /tmp/registry.service /etc/systemd/system/registry.service
sudo systemctl daemon-reload
sudo systemctl enable registry --now
```

Check the status with:

```bash
sudo journalctl -u registry
```

## Use the registry within a workflow

Create a new registry in your organisation, along with a: `.github/workspaces/build.yml` file and commit it to the repository.

```yaml
name: CI

on:
  pull_request:
    branches:
      - '*'
  push:
    branches:
      - master
      - main

jobs:
    build:
        runs-on: [actuated]
        steps:

        - name: Setup mirror
          uses: self-actuated/hub-mirror@master

        - name: Checkout
            uses: actions/checkout@v2
    
        - name: Pull image using cache
            run: |
            docker pull alpine:latest
```

## Checking if it worked

You'll see the build run, and cached artifacts appearing in: `/var/lib/registry/`.

```bash
find /var/lib/registry/ -name "alpine"

/var/lib/registry/docker/registry/v2/repositories/library/alpine
```

You can also use the registry's API to query which images are available:

```bash
curl -i http://192.168.128.1:5000/v2/_catalog

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Docker-Distribution-Api-Version: registry/2.0
Date: Wed, 16 Nov 2022 09:41:18 GMT
Content-Length: 52

{"repositories":["library/alpine","moby/buildkit"]}
```

You can check the status of the mirror at any time with:

```bash
sudo journalctl -u registry --since today
```

## Further reading

* [Docker: Configuration for the registry](https://docs.docker.com/registry/configuration/)
* [GitHub: View the project on GitHub](https://github.com/distribution/distribution)
