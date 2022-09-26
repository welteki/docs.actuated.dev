# Frequently Asked Questions (FAQ)

## How does it work?

Actuated has three main parts:

1. an agent which knows how to run VMs, you install this on your hosts
2. a VM image and Kernel that we build which has everything required for Docker, KinD and K3s
3. a multi-tenant control plane that we host, which tells your agents to start VMs and register a runner on your GitHub organisation

The multi-tenant control plane is run and operated by OpenFaaS Ltd.

## What kind of machines do I need for the agent?

You'll need either: a bare-metal host (your own, AWS i3.metal or Equinix Metal), or a VM that supports nested virtualisation such as those provided by GCP and DigitalOcean.

## Who is actuated for?

actuated is primarily for software engineering teams who are currently using GitHub Actions. A GitHub organisation is required for installation, and runners are attached to individual repositories as required, to execute builds.

## How many builds does a single actuated VM run?

A VM can run at most one build, then it will be destroyed.

## What's in the VM image and how is it built?

The VM image will be publicly available in a container registry for you to explore.

## Is ARM64 supported?

Yes, actuated is built to run on both Intel/AMD and ARM64 hosts, check your subscription plan to see if ARM64 is included.

## Is Actuated free and open-source?

Actuated is commercial software developed by OpenFaaS Ltd. A subscription will be required to use the software. 

The website and documentation are available on GitHub and we plan to release some open source tools in the future for actuated customers. 

## Is there a risk that we could get "locked-in" to actuated?

No, you can move back to hosted runners, or self-managed self-hosted runners at any time, but you may run into the problems that actuated sets out to solve.

## Why is the brand called "actuated" and "selfactuated"?

The name of the software is actuated, in some places "actuated" is not available, and we liked "selfactuated" more than "actuatedhq" or "actuatedio" because it refers to the hybrid experience of self-hosted runners.

## What do I need to change in my workflows?

Very little, just add / set `runs-on: actuated`

## Where can I find example GitHub Actions for Docker, KinD, K3s or OpenFaaS?

* [AMD64/Intel examples](https://github.com/actuated-samples)
* [AARCH64 / Graviton / Raspberry Pi 4 examples](https://github.com/actuated-samples-arm64)
