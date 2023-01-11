## Provision a Server for actuated

You'll need to provision or allocate a Server which is capable of virtualisation. This is because each of your builds will run in an isolated microVM, with its own networking, Kernel and filesystem.

We regularly update our recommendations, which include bare-metal and cloud VMs which support nested virtualisation. These do not need to be overly expensive, if performance is not a concern, DigitalOcean VMs are available from tens of dollars per month, but a bare-metal server with a CPU that will run at a burst capacity of 4.4GHz, is around 100 USD / mo. If you have your own hardware, this can also be a really cost effective way of adopting actuated.

1. Bare-metal on-premises (cheap, convenient, high performance)

    Running bare-metal on-premises is a cost-effective and convenient way to re-use existing hardware investment.

    The machine could be racked in your server room, under your desk, or in a co-lo somewhere.

    Make sure you segment or isolate the agent into its own subnet, VLAN, DMZ, or VPC so that it cannot access the rest of your network. If you are thinking of running an actuated runner at home, we can share some iptables rules that worked well for our own testing.

    For on-premises ARM64 builds, we recommend the Mac Mini M1 (2020) with 16GB RAM and 512GB storage with Asahi Linux. The Raspberry Pi 4 also works, and in one instance [was much faster than using emulation with a Hosted GitHub Runner](https://twitter.com/alexellisuk/status/1583092051398524928?s=20&t=2SelTpdc5idJLmayIu3Djw).

2. Bare-metal on the cloud (higher cost, convenient, high performance)

    Bare-metal doesn't have to mean managing hardware in your own physical datacenter. You can deploy machines by API, pay-as-you-go and get the highest performance available.
    
    Bear in mind that whilst the cost of bare-metal is higher than VMs, you will be able to pack more builds into them and get better throughput since actuated can schedule builds much more efficiently than GitHub's self-hosted runner.

    We have seen the best performance from hosts with high clock speeds like the recent generation of AMD processors, combined with local NVMe storage. Rotational drives and SATA SSDs are significantly slower. At the lower end of bare-metal providers, you'll pay 40-50 EUR / mo per host, moving up to 80-150 EUR / mo for NVMe and AMD processors, when you go up to enterprise-grade bare-metal with 10Gbit uplinks, you'll be more in the range of 500-1500 USD / mo.

    There are at least a dozen options for hosted bare-metal: [Equinix Metal](https://deploy.equinix.com/), [Ionos](https://ionos.co.uk), [Hetzner](https://hetzner.com), [AWS](https://aws.amazon.com/), [Cherry Servers](https://www.cherryservers.com/), [Alibaba Cloud](https://eu.alibabacloud.com/en), [OVHcloud](https://www.ovhcloud.com/en-gb/bare-metal/rise/), [fasthosts](https://www.fasthosts.co.uk/), [Scaleway](https://scaleway.com) and [Vultr](https://www.vultr.com/), [see a list here](https://github.com/alexellis/awesome-baremetal#bare-metal-cloud)
    
    For x86_64 builds we recommend using [Equinix Metal](https://deploy.equinix.com/) for the best price / performance ratio. They also have discounts for reserved instances on contract. The smallest instances available are the c3.small.x86 and c2.small.x86.

    We have done customer testing with [Ionos](https://ionos.co.uk) with AMD CPUs and local NVMe, these are very quick and are good value.

    [Having tested several of Scaleway's servers](https://twitter.com/alexellisuk/status/1605866713815437312?s=20&t=JGh5fGZJWklLTCTVkTVElg), we do not recommend their current generation of bare-metal due to I/O bottlenecks.

    For ARM64 builds the cheapest option is to use the [a1.metal](https://aws.amazon.com/ec2/instance-types/a1/) instance on AWS. For a step up on specs, take a look at the c3.large.arm64 from [Equinix Metal](https://metal.equinix.com/).

3. Cloud Virtual Machines (VMs) that support nested virtualization (lowest cost, convenient, mid-level performance)

    This option may not have the raw speed and throughput of a dedicated, bare-metal host, but keeps costs low and is convenient for getting started.

    We know of at least three providers which have options for nested virtualisation: [DigitalOcean](https://m.do.co/c/8d4e75e9886f) [Google Compute Platform (GCP)](https://cloud.google.com/compute) (new customers get 300 USD free credits from GCP) support nested virtualisation on their Virtual Machines (VMs), and [Azure](https://azure.com/).

    We have tested ARM64 VMs on Oracle OCI, Azure and GCP and found that they do not currently allow for virtualisation. So whilst these clouds may be an option for x86, for ARM64, you'll need access to bare-metal.

The recommended Operating System for an Actuated Agent is: Ubuntu Server 22.04 or Ubuntu Server 20.04.

Still not sure which option is right for your team? Get in touch with us on the [Actuated Slack](https://self-actuated.slack.com) and we'll help you decide.

## Next steps

Now that you've created a Server or VM with the recommended Operating System, you'll need to install the actuated agent and get in touch with us, to register it.

* [Install the Actuated Agent](/install-agent.md)
