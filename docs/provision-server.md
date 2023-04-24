## Provision a Server for actuated

You'll need to provision or allocate a Server which is capable of virtualisation. This is because each of your builds will run in an isolated microVM, with its own networking, Kernel and filesystem.

We regularly update our recommendations, which include bare-metal and cloud VMs which support nested virtualisation. These do not need to be overly expensive, if performance is not a concern, DigitalOcean VMs are available from tens of dollars per month, but a bare-metal server with a CPU that will run at a burst capacity of 4.4GHz, is around 100 USD / mo. If you have your own hardware, this can also be a really cost effective way of adopting actuated.

!!! info "500 USD free credit for bare-metal"
    [Equinix Metal](https://metal.equinix.com/) have partnered with us to offer 500 USD of credit for new customers to use on actuated. You can request the discount code after purchasing your actuated subscription.

### Intel/AMD

Intel and AMD CPUs can be used interchangeable and are known as `amd64` or `x86_64`.

1. Bare-metal on the cloud (higher cost, convenient, high performance)

    Bare-metal doesn't have to mean managing hardware in your own physical datacenter. You can deploy machines by API, pay-as-you-go and get the highest performance available.
    
    Bear in mind that whilst the cost of bare-metal is higher than VMs, you will be able to pack more builds into them and get better throughput since actuated can schedule builds much more efficiently than GitHub's self-hosted runner. For the same reasons, most customers are unlikely to need any form of autoscaling.

    We have seen the best performance from hosts with high clock speeds like the recent generation of AMD processors, combined with local NVMe storage. Rotational drives and SATA SSDs are significantly slower. At the lower end of bare-metal providers, you'll pay 40-50 EUR / mo per host, moving up to 80-150 EUR / mo for NVMe and AMD processors, when you go up to enterprise-grade bare-metal with 10Gbit uplinks, you'll be more in the range of 500-1500 USD / mo.

    There are at least a dozen options for hosted bare-metal: [Equinix Metal](https://deploy.equinix.com/), [Ionos](https://ionos.co.uk), [Hetzner](https://hetzner.com), [AWS](https://aws.amazon.com/), [Cherry Servers](https://www.cherryservers.com/), [Alibaba Cloud](https://eu.alibabacloud.com/en), [OVHcloud](https://www.ovhcloud.com/en-gb/bare-metal/rise/), [fasthosts](https://www.fasthosts.co.uk/), [latitude.sh](https://www.latitude.sh/), [Scaleway](https://scaleway.com) and [Vultr](https://www.vultr.com/), [see a list here](https://github.com/alexellis/awesome-baremetal#bare-metal-cloud). [Having tested several of Scaleway's servers](https://twitter.com/alexellisuk/status/1605866713815437312?s=20&t=JGh5fGZJWklLTCTVkTVElg), we do not recommend their current generation of bare-metal due to slow I/O and CPU speeds.

    [Equinix Metal](https://deploy.equinix.com/) have partnered with us to offer 500 USD of credit for new customers to use on actuated. You'll get the discount code after signing up with us. We've tested their c3.small.x86 and c2.small.x86 machines, and they are very fast, with enterprise-grade networking and support included, with many different regions available.

    Both [Ionos](https://ionos.co.uk) and [Hetzner](https://hetzner.com) have excellent value, with NVMe storage very fast AMD CPUs available.
    
    Hetzner have a minimum commitment of one month - we recommend their AX-Line with NVMe and ECC RAM - for instance the AX41-NVME, AX52, or AX102. 


2. Cloud Virtual Machines (VMs) with nested virtualization (lowest cost, convenient, mid-level performance)

    This option may not have the raw speed and throughput of a dedicated, bare-metal host, but keeps costs low and is convenient for getting started.

    We know of at least three providers which have options for nested virtualisation: [DigitalOcean](https://m.do.co/c/8d4e75e9886f), [Google Compute Platform (GCP)](https://cloud.google.com/compute) (new customers get 300 USD free credits from GCP) support nested virtualisation on their Virtual Machines (VMs), and [Azure](https://azure.com/).

3. Bare-metal on-premises (cheap, convenient, high performance)

    Running bare-metal on-premises is a cost-effective and convenient way to re-use existing hardware investment.

    The machine could be racked in your server room, under your desk, or in a co-lo somewhere.

    Make sure you segment or isolate the agent into its own subnet, VLAN, DMZ, or VPC so that it cannot access the rest of your network. If you are thinking of running an actuated runner at home, [we have suggested iptables rules that worked well for our own testing](/expose-agent#preventing-the-runner-from-accessing-your-local-network).

### Arm

64-bit Arm is also known as both `aarch64` and `arm64`.

Arm CPUs are highly efficient when it comes to power consumption and pack in many more cores than the typical x86_64 CPU. This makes them ideal for running many builds in parallel. In typical testing, we've seen Arm builds running under emulation taking 35-45 minutes being reduced to 1-3 minutes total.

1. Arm on-demand, in the cloud

    For ARM64, [Hetzner](https://hetzner.com) provides outstanding value in their [RX-Line](https://www.hetzner.com/dedicated-rootserver/matrix-rx) with 128GB / 256GB RAM coupled with NVMe and 80 cores for around 200 EUR / mo. These are [Ampere Altra Servers](https://amperecomputing.com/processors/ampere-altra/). There is a minimum commitment of one month, and an initial setup cost per server.

    Following on from that, you have the [a1.metal](https://aws.amazon.com/ec2/instance-types/a1/) instance on AWS with 16 cores and 30GB / RAM [for roughly 0.4 USD / hour](https://instances.vantage.sh/aws/ec2/a1.metal?region=us-east-1&os=linux&cost_duration=hourly&reserved_term=Standard.noUpfront), and roughly half that cost with a 1x year reservation. This is ideal if you already have an account with AWS and want to pay per minute. The instances are configured with an 8GB EBS volume by default, which will need to be increased to around 60-80GB. Optionally, the storage class can also be upgraded from gp2 (default) to gp3 for better baseline performance.
    
    For a step up on specs, and enterprise-grade networking, take a look at the c3.large.arm64 (Ampere Altra) from [Equinix Metal](https://metal.equinix.com/) comes with enterprise-grade networking and faster uplinks. These machines come in at around 2.5 USD / hour, but are packed out with many cores and other benefits. You can usually provision these servers in the Washington DC and Dallas metros.

2. Arm for on-premises

    For on-premises ARM64 builds, we recommend the Mac Mini M1 (2020) with 16GB RAM and 512GB storage with Asahi Linux.
    
    It's also possible to buy a 1 or 2U server from Ampere through one of their partners, with a capital expense up front.

    The Raspberry Pi 4 also works when used with an external NVMe, and in one instance [was much faster than using emulation with a Hosted GitHub Runner](https://twitter.com/alexellisuk/status/1583092051398524928?s=20&t=2SelTpdc5idJLmayIu3Djw).

3. Arm VMs with nested virtualisation

    The current generations of Arm CPUs available from cloud providers do not support KVM, or nested virtualisation, which means you need to pick from the previous two options.

    There are Arm VMs available on Azure, GCP, and Oracle OCI. We have tested each and since they are based on the same generation of Ampere Altra hardware, we can confirm that they do not have KVM available and will not work for running actuated builds.

### Server Operating System

The recommended Operating System for an Actuated Agent is: Ubuntu Server 22.04. Ubuntu Server 20.04 will also work if 22.04 is unavailable for some reason.

Still not sure which option is right for your team? Get in touch with us on the [Actuated Slack](https://self-actuated.slack.com) and we'll help you decide.

## Next steps

Now that you've created a Server or VM with the recommended Operating System, you'll need to install the actuated agent and get in touch with us, to register it.

* [Install the Actuated Agent](/install-agent)
