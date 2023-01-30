# Roadmap

Actuated is in a pilot phase, running builds for participating customers. The core experience is functioning and we are dogfooding it for actuated itself, OpenFaaS Pro and inlets.

Our goal with the pilot is to prove that there's market fit for a solution like this, and if so, we'll invest more time in automation, user experience, agent autoscaling, dashboards and other polish.

The technology being used is run at huge scale in production by AWS (on Lambda and Fargate) and GitHub (self-hosted runners use the same runner software and control plane).

We believe actuated solves a real problem making CI fast, efficient, and multi-arch.

If you'd like to try it out for your team, [Register interest for the pilot](https://forms.gle/8XmpTTWXbZwWkfqT6).

Shipped

* [x] Firecracker MicroVM support for runners
* [x] Secure builds for both public and private repos
* [x] *Fat VM* image to match tooling installed by GitHub Actions
* [x] KinD support for runner's Kernel
* [x] K3s support for runner's Kernel
* [x] ARM64 support, including Raspberry Pi 4B
* [x] Efficient scheduling of jobs across fleet of agents
* [x] Samples for K3s/KinD/Matrix builds and OpenFaaS functions
* [x] Subscription plans delivered by Gumroad
* [x] API for reviewing connected agents and queue depth
* [x] Job event auditing for review via API
* [x] Documentation site with detailed GitHub Actions examples
* [x] Customer dashboard UI to show connected agents and build queue
* [x] Official website [actuated.dev](https://actuated.dev)
* [x] Remote / automated update of agents via control plane
* [x] Blog feature on actuated.dev with news, tutorials and updates from our team
* [x] Performance testing for Ionos & Scaleway for cost effective AMD bare-metal
* [x] Daily build statistics on your dashboard
* [x] Docker cache directly on the Actuated Hosts (servers) for much faster builds and avoiding rate-limiting
* [x] Subscriptions: migration to LemonSqueezy for lower fees, and more payment options
* [x] Dashboard - animation on all data pages for better feedback when refreshing data

Coming soon:

* [ ] Detailed insights across your organisation on usage
* [ ] Support for private, self-hosted GitHub Enterprise Server (GHES) installations

Under consideration:

* [ ] Actuated for GitLab
* [ ] Actuated for Jenkins
* [ ] GPU pass-through support for ML and AI workloads
* [ ] Automated agent installation and bootstrap

Is there something else you need? If you're already a customer, contact us via [the actuated Slack](https://self-actuated.slack.com) or [Register for interest](https://forms.gle/8XmpTTWXbZwWkfqT6).

