# Troubleshooting

## Getting support

All customers have access to a public Slack channel for support and collaboration.

Enterprise customers may also have an upgraded SLA for support tickets via email and access to a private Slack channel.

## The Actuated Dashboard

The first port of call should be the [Actuated Dashboard](https://dashboard.actuated.dev) where you can check the status of your agents and see the current queue of jobs.

For security reasons, an administrator for your GitHub Organisation will need to approve the Actuated Dashboard for access to your organisation before team members will be able to see any data. Send them the link for the dashboard, and have them specifically tick the checkbox for your organisation when logging in for the first time.

If you missed this step, have them head over to their [Applications Settings page](https://github.com/settings/apps/authorizations), click "Authorized OAuth Apps" and then "Actuated Dashboard". On this page, under "Organization access" they can click "Grant" for each of your organisations registered for actuated.

![OAuth Access Page](/images/oauth-dashboard-access.png)
> How to "Grant" access to the Dashboard.

## A job is stuck as "queued"

By default, we reject jobs on public repositories, however we can feature flag this on for you. Just ask us.

If you're using a private repo and the job is queued, let us know on Slack and we'll check the audit database to try and find out why.

You can also check `/var/log/actuated/` for log files, `tail -n 20 /var/log/actuated/*.txt` should show you any errors that may have occurred on any of the VM boot-ups or runner registrations.

From time to time, GitHub Actions does have an outage.

## You pull a lot of large images from the Docker Hub

As much as we like to make our images as small as possible, sometimes we just have to pull down either large artifacts or many smaller ones. It just can't be helped.

Since a MicroVM is a completely immutable environment, the pull needs to happen on each build, which is actually a *good thing*.

The pull speed can be dramatically improved by using a registry mirror on each agent:

* [Example: Set up a registry mirror](/examples/registry-mirror)

## You are running into rate limits when using container images from the Docker Hub

The Docker Hub implements stringent rate limits of 100 pulls per 6 hours, and 200 pulls per 6 hours if you log in. Pro accounts get an increased limit of 5000 pulls per 6 hours.

We've created simple instructions on how to set up a registry mirror to cache images on your Actuated Servers.

* [Example: Set up a registry mirror](/examples/registry-mirror)

## You need to rotate the authentication token used for your agent

There should not be many reasons to rotate this token, however, if something's happened and it's been leaked or an employee has left the company, contact us via email for the update procedure.

## You need to rotate your private/public keypair

Your private/public keypair is comparable to an SSH key, although it cannot be used to gain access to your agent via SSH.

If you need to rotate it for some reason, please contact us by email as soon as you can.

## Disk space is running out

This can be observed by running `df -h` or `df -h /`.

Over time, the various "fat" VM images that we ship to you may fill up the disk space on your machine.

You can delete all images, including the current image with the following command.

```bash
sudo ctr -n mvm image ls -q | xargs sudo ctr -n mvm image rm
```

We do not recommend running this on a cron schedule since the maintenance command will cause your agent to download the latest fat VM image ~ 1GB+/- again.

An alternative for a cron schedule would need to exclude the current image being used:

```bash
CURRENT="ghcr.io/openfaasltd/actuated-ubuntu:20.0.4-2022-09-30-1357"
sudo ctr -n mvm image ls -q |grep -v $CURRENT | xargs sudo ctr -n mvm image rm
```

## Your agent has been offline or unavailable for a significant period of time

If your agent has been offline for a significant period of time, then our control plane will have disconnected it from its pool of available agents.

Contact us via Slack to have it reinstated.

## The devmapper snapshotter is not available or running

The actuated agent uses the devmapper snapshotter for containerd, which emulates a thin-provisioned block device. Performance can be improved by attaching a dedicated disk or partition, but in our testing the devmapper works well enough for most workloads.

The dmsetup.sh script must be run upon every fresh boot-up of the host. It enables Firecracker to use snapshots to save disk space when launching new VMs.

If you see an error about the "devmapper" snapshot driver, then run the `dmsetup.sh` shell script then restart containerd:

```bash
./dmsetup.sh
sudo systemctl daemon-reload
sudo systemctl restart containerd
```

## Your builds are slower than expected

* Check free disk space (`df -h`)
* Check for unattended updates/upgrades (`ps -ef | grep unattended-upgrades`) and (`ps -ef | grep apt`)

If you're using spinning disks, then consider switching to SSDs. If you're already using SSDs, consider using PCIe/NVMe SSDs.

Finally, we do have another way to speed up microVMs by attaching another drive or partition to your host. Contact us for more information.
