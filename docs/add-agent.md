# Add your first agent to actuated

actuated is split into three parts:

1. An agent that you run on your own machines or VMs, which can launch a VM with a single-use GitHub Actions runner
2. A VM image launched by the agent, with all the preinstalled software found on a hosted GitHub Actions runner
3. Our own control plane that talks to GitHub on your behalf, and schedules builds across your fleet of agents.

We look after 2 and 3 which means you just have to set up one or more agent to get started.

!!! info "Have you registered your organisation yet?"
    Before you can add an agent, you or your GitHub organisation admin will need to install the: [Actuated GitHub App](registration.md).

## Decide where to run your agent

There are three places you can run an agent:

1. Bare-metal on-premises (cheap, convenient, high performance)

    This could be a machine racked in your server room, under your desk, or in a co-lo somewhere.

    It can be a cheap and convenient way to re-use existing hardware.

    Make sure you segment or isolate the agent into its own subnet, VLAN, DMZ, or VPC so that it cannot access the rest of your network. If you are thinking of running an actuated runner at home, we can share some iptables rules that worked well for our own testing.

2. Bare-metal on the cloud (higher cost, convenient, high performance)

    You can provision bare-metal hosts in the cloud using any number of providers. A couple examples would be: [AWS i3.metal instances](https://aws.amazon.com/ec2/instance-types/i3/) or [Equinix Metal](https://metal.equinix.com/).

    This option is both convenient and offers the highest performance available, however bare-metal machines tends to be priced higher than you may be used to with VMs.

    Bear in mind that you may be able to run a single, larger bare-metal machine where you used to need half a dozen cloud VMs, since actuated can schedule builds much more efficiently than the built-in self-hosted runner from GitHub.

3. Cloud Virtual Machines (VMs) that support nested virtualization (lowest cost, convenient, mid-level performance)

    Both [DigitalOcean](https://m.do.co/c/8d4e75e9886f) and [Google Compute Platform (GCP)](https://cloud.google.com/compute) (new customers get 300 USD free credits from GCP) have options to support nested virtualisation on their Virtual Machines (VMs).

    This option may not have the raw speed and throughput of a dedicated, bare-metal host, but keeps costs low and is convenient for getting started.

## Review the End User License Agreement (EULA)

Make sure you've read the [Actuated EULA](https://github.com/self-actuated/actuated/blob/master/EULA.md) before registering your organisation with the actuated GitHub App, or starting the agent binary on one of your hosts.

## Set up your first agent

1. Download the agent binary

    Once you've decided where to set up your first agent, get in touch with us for a download link for the actuated agent binary.

    Transfer the agent binary over to the host along with the setup.sh script we provide.

    Install the agent binary to `/usr/local/bin/`

    Run the setup.sh script which will install all the required dependencies like containerd, CNI and Firecracker.

    Create a folder with `mkdir -p $HOME/.actuated/` then save your Gumroad license key to `$HOME/.actuated/LICENSE`

2. Generate an RSA keypair

    `agent -genkey`

    This should write: `key` and `key.pub` to the current working folder. Share `key.pub` with us via email or Slack. This key is not confidential, so don't worry about sharing it.

    Keep the `key` private, we will not ask you to share this file with us.

3. Install the agent's authentication token.

    Once you've sent us your public key, we will send you an authentication token for your agent. This will be used for any HTTP requests to the agent's endpoint (below).

    Save the token as: `$HOME/agent-token.txt`

4. Add HTTPS for the agent's endpoint

    For the pilot, the agent's HTTP endpoint will need to be exposed on the Internet using a reverse proxy or an inlets tunnel. The actuated control plane will only communicate with a HTTPS endpoint to ensure properly encryption is in place.

    We're considering other models for after the pilot, for instance GitHub's own API has the runner make an outbound connection and uses long-polling.

    See also: [expose the agent with HTTPS](expose-agent.md)

4. Start the agent

    The parameter for the start script includes a tag for the VM image, we are currently using version 35:

    `./start.sh 35`

## Next steps

You can now start your first build and see it run on your actuated agent.

[Start a build on your agent](test-build.md)
