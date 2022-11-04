# Roadmap

Actuated is in a pilot phase, running builds for participating customers. The core product is functioning and we are dogfooding it for OpenFaaS and inlets.

Our goal with the pilot is to prove that there's market fit for a solution like this, and if so, we'll invest more time in automation, user experience, agent autoscaling, dashboards and other polish.

For now, if you're interested in participating and giving feedback, we believe actuated already solves pain at this stage.

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

Coming soon:

* [ ] UI Portal to show connected agents, build queue and GitHub job events
* [ ] Support for GitLab.com / self-hosted GitLab (contact us if you need this)
* [ ] Consider alternatives to having the control plane talk to agents over HTTPS, such as long polling and the agent connecting to the control plane.
* [ ] Automated agent installation and bootstrap
* [ ] Autoscaling for agents
