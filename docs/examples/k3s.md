# Example: Kubernetes with k3s

You may need to access Kubernetes within your build. [K3s](https://k3s.io) is a for-production, lightweight distribution of Kubernetes that uses fewer resources than upstream. [k3sup](https://github.com/alexellis/k3sup) is a popular tool for installing k3s.

Certified for:

- [x] `x86_64`
- [x] `arm64` including Raspberry Pi 4

!!! warning "Don't use a public repository"
    Due to limitations in the design of GitHub's runner, we recommend using a private repository. Learn more in the [FAQ](/faq.md).

## Try out the action on your agent

Create a new file at: `.github/workspaces/build.yml` and commit it to the repository.

Note that it's important to make sure Kubernetes is responsive before performing any commands like running a Pod or installing a helm chart.

```yaml
name: k3sup-tester

on: push
jobs:
  k3sup-tester:
    runs-on: actuated
    steps:
      - name: Update developer tools
        run: |
          curl -sLS https://get.arkade.dev | sudo sh
          arkade get \
            kubectl \
            k3sup@0.12.7
          chmod +x $HOME/.arkade/bin/*
          sudo mv $HOME/.arkade/bin/* /usr/local/bin/
      - name: Install K3s with k3sup
        run: |
          mkdir -p $HOME/.kube/
          k3sup install --local --local-path $HOME/.kube/config
      - name: Wait until nodes ready
        run: |
          k3sup ready --quiet --kubeconfig $HOME/.kube/config --context default
      - name: Wait until CoreDNS is ready
        run: |
          kubectl rollout status deploy/coredns -n kube-system --timeout=300s
      - name: Explore nodes
        run: kubectl get nodes -o wide
      - name: Explore pods
        run: kubectl get pod -A -o wide
```

See also: [actuated-samples/k3sup-tester](https://github.com/actuated-samples/k3sup-tester/blob/master/.github/workflows/build.yml)

To run this on ARM64, just change the actuated label to `actuated-aarch64`.
