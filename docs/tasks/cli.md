# Actuated CLI

Monitor Actuated runners and jobs from the command line.

## Installation

Download and installation instruction are available via the [actuated-dashboard](https://dashboard.actuated.dev/cli)

You'll need to run `actuated-cli auth` first, so that you can get a Personal Access Token with the appropriate scopes from GitHub.

## View queued jobs

```bash
actuated-cli jobs \
    actuated-samples
```

## View runners for organization

```bash
actuated-cli runners \
    actuated-samples
```

## Check the logs of VMs

View the serial console and systemd output of the VMs launched on a specific server.

* Check for timeouts with GitHub's control-plane
* View output from the GitHub runner binary
* See boot-up messages
* Check for errors if the GitHub Runner binary is out of date

```bash
actuated-cli logs \
    --owner actuated-samples \
    --age 15m \
    runner1
```

The age is specified as a Go duration i.e. `60m` or `24h`.

## Check the logs of the actuated agent service

Show the logs of the actuated agent binary running on your server.

View VM launch times, etc.

```bash
actuated-cli agent-logs \
    --owner actuated-samples \
    --age 60m \
    runner1
```

## Schedule a repair to re-queue jobs

If a job has been retried for 30 minutes, without a runner to take it, it'll be taken off the queue. This command will re-queue all jobs that are in a "queued" state.

Run sparingly because it will launch one VM per job queued.

```bash
actuated-cli repair \
    actuated-samples
```

## JSON mode

Add `--json` to any command to get JSON output for scripting.

API rate limits apply, so do not run the CLI within a loop or `watch` command.

## Staff mode

The `--staff` flag can be added to the `runners`, `jobs` and the `repair` commands by OpenFaaS Ltd staff to support actuated customers.

## Help & support

Reach out to our team [on Slack](https://self-actuated.slack.com)