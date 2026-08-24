# Prepare the macOS base image

The macOS agent creates its base VM locally from a restore image downloaded
directly from Apple. Actuated does not distribute a macOS base image.

Preparing the base image installs macOS, configures the guest agent, and adds
the GitHub Actions runner. The agent uses this base as a template and creates a
disposable clone for each job VM.

Complete the host requirements and [install the agent binary](install-macos-agent.md#2-install-the-agent)
before preparing the image. Run all agent commands as the regular macOS user
that will run the service, not as root.

## Choose an image preparation method

The automatic method is recommended. It completes the macOS Setup Assistant
without user interaction. The manual method opens the VM console and leaves
the macOS Setup Assistant to the operator.

Both methods create the same base bundle at `~/.actuated/base.bundle`.

=== "Automatic"

    Create the VM from Apple's latest restore image supported by the agent:

    ```bash
    ~/.actuated/bin/agent base create \
      --ipsw latest \
      --bundle ~/.actuated/base.bundle
    ```

    The agent downloads the IPSW from Apple, which can take a few minutes.

    Start the VM and let the agent complete the macOS Setup Assistant. The
    default guest account is `runner` with password `runner`.

    ```bash
    ~/.actuated/bin/agent base oobe \
      --bundle ~/.actuated/base.bundle
    ```

    When setup finishes, leave the `oobe` command running. Open a second
    Terminal, install the GitHub Actions runner into the base image, then shut
    down the VM:

    ```bash
    ~/.actuated/bin/agent base provision \
      --bundle ~/.actuated/base.bundle

    ~/.actuated/bin/agent vm agent-shutdown \
      --bundle ~/.actuated/base.bundle
    ```

=== "Manual"

    Create the VM and open its console:

    ```bash
    ~/.actuated/bin/agent base create \
      --ipsw latest \
      --bundle ~/.actuated/base.bundle
    ```

    The agent downloads the IPSW from Apple, which can take a few minutes.

    Complete the macOS Setup Assistant as follows:

    1. Select the language and region.
    2. Set up the Mac as new and skip accessibility setup.
    3. Create the local account using `runner` for the full name, account name,
       and password.
    4. Skip Apple Account sign-in. The guest must not use an Apple Account.
    5. Complete the privacy and analytics screens and wait for the desktop.

    Open Terminal inside the VM and run:

    ```bash
    sudo "/Volumes/My Shared Files/guest-agent" init
    ```

    Enter the `runner` password when prompted. The VM shuts down and the
    `base create` command exits after the guest agent is installed.

    Start the VM again, install the GitHub Actions runner into the base image,
    then shut down the VM:

    ```bash
    ~/.actuated/bin/agent vm run \
      --bundle ~/.actuated/base.bundle &

    ~/.actuated/bin/agent base provision \
      --bundle ~/.actuated/base.bundle

    ~/.actuated/bin/agent vm agent-shutdown \
      --bundle ~/.actuated/base.bundle
    ```

If `latest` cannot be installed on the host, download a compatible IPSW from
Apple and pass its local path to `--ipsw` instead.

The prepared base image now contains the GitHub Actions runner. Per-job
credentials are supplied to disposable clones and are never stored in the
base.

## Verify the base image

Verification is optional. If you verify the image, always test a disposable
clone, not the base itself:

```bash
~/.actuated/bin/agent vm clone \
  --from ~/.actuated/base.bundle \
  --to ~/.actuated/smoke.bundle

~/.actuated/bin/agent vm run \
  --bundle ~/.actuated/smoke.bundle &

~/.actuated/bin/agent vm agent-health \
  --bundle ~/.actuated/smoke.bundle

~/.actuated/bin/agent vm rm \
  --bundle ~/.actuated/smoke.bundle \
  --force
```

The health command should return JSON describing the guest. Keep
`base.bundle` stopped; the agent clones it for each job.

Next, return to [Install and join the macOS agent](install-macos-agent.md#4-enroll-the-host).
