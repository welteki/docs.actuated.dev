# Example: Debug a job with SSH

If your it's included within your actuated plan, then you can get a shell into any self-hosted runner - including GitHub's own hosted runners.

Certified for:

- [x] `x86_64`
- [x] `arm64`

!!! info "Use a private repository if you're not using actuated yet"
    GitHub recommends using a private repository with self-hosted runners because changes can be left over from a previous run, even when using Actions Runtime Controller. Actuated uses an ephemeral VM with an immutable image, so can be used on both public and private repos. Learn why in the [FAQ](/faq).

## Try out the action on your agent

Create a secret for the repo or organisation for `SSH_GATEWAY` using the hostname that you were provided with by your support team, minus the `https://` prefix.

Create a `.github/workflows/workflow.yaml` file

```yaml
name: connect

on:
  pull_request:
    branches:
      - '*'
  push:
    branches:
      - master

permissions:
  id-token: write
  contents: read
  actions: read

jobs:
  connect:
    name: connect
    runs-on: actuated
    steps:
      - name: Setup SSH server for Actor
        uses: self-actuated/setup-sshd-for-actor@master
      - name: Connect to the actuated SSH gateway
        uses: self-actuated/connect-ssh-gateway@master
        with:
          gatewayaddr: ${{ secrets.SSH_GATEWAY }}
          secure: true
      - name: Setup a blocking tmux session
        uses: self-actuated/block-with-tmux@master
```

Next, trigger a build.

Open `https://$SSH_GATEWAY/list` in your browser and look for your session, you can log in using the SSH command outputted for you.

Alternatively, you can view your own SSH sessions from the [actuated dashboard](https://dashboard.actuated.dev).

Watch a demo:

<iframe width="560" height="315" src="https://www.youtube.com/embed/l9VuQZ4a5pc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
