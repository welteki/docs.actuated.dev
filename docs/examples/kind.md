# Example: Kubernetes with KinD

You may need to access Kubernetes within your build. [KinD](https://kind.sigs.k8s.io) is a popular option, and easy to run in an action.

Certified for:

- [x] `x86_64`
- [x] `arm64` including Raspberry Pi 4

!!! warning "Don't use a public repository"
    Due to limitations in the design of GitHub's runner, we recommend using a private repository. Learn more in the [FAQ](/faq.md).

## Try out the action on your agent

Create a new file at: `.github/workspaces/build.yml` and commit it to the repository.

Note that it's important to make sure Kubernetes is responsive before performing any commands like running a Pod or installing a helm chart.

```yaml
name: build

on: push
jobs:
  start-kind:
    runs-on: actuated
    steps:
      - uses: actions/checkout@master
        with:
          fetch-depth: 1
      - name: get arkade
        run: curl -sLS https://get.arkade.dev | sudo sh
      - name: get kind
        run: arkade get kind
      - name: get kubectl
        run: arkade get kubectl
      - name: Install Kubernetes kind
        run: |
          mkdir -p $HOME/.kube/
          $HOME/.arkade/bin/kind create cluster --wait 300s
      - name: Wait until CoreDNS is ready
        run: |
          $HOME/.arkade/bin/kubectl rollout status deploy/coredns -n kube-system --timeout=300s
      - name: Explore nodes
        run: $HOME/.arkade/bin/kubectl get nodes -o wide
      - name: Explore pods
        run: $HOME/.arkade/bin/kubectl get pod -A -o wide
      - name: Show kubelet logs
        run: docker exec kind-control-plane journalctl -u kubelet
```

See also: [actuated-samples/kind-tester](https://github.com/actuated-samples/kind-tester)

To run this on ARM64, just change the actuated label to `actuated-aarch64`.
