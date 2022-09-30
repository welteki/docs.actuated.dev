# Example: Kubernetes with k3s

You may need to access Kubernetes within your build. [K3s](https://k3s.io) is a for-production, lightweight distribution of Kubernetes that uses fewer resources than upstream. [k3sup](https://github.com/alexellis/k3sup) is a popular tool for installing k3s.

Certified for:

- [x] `x86_64`
- [x] `arm64` including Raspberry Pi 4

## Try out the action on your agent

Create a new file at: `.github/workspaces/build.yml` and commit it to the registry.

Note that it's important to make sure Kubernetes is responsive before performing any commands like running a Pod or installing a helm chart.

```yaml
name: build

on: push
jobs:
  start-k3s:
    runs-on: actuated
    steps:
      - name: get arkade
        run: curl -sLS https://get.arkade.dev | sudo sh
      - name: get k3sup
        run: arkade get k3sup
      - name: get kubectl
        run: arkade get kubectl
      - name: Install K3s with k3sup
        run: |
          mkdir -p $HOME/.kube/
          $HOME/.arkade/bin/k3sup install --local --local-path $HOME/.kube/config
      - name: Wait for nodes to be ready
        run: |
          for i in {1..10};
          do
            $HOME/.arkade/bin/kubectl wait --for=condition=ready node --all --timeout=300s
            retVal=$?
            if [ "$retVal" -eq "0" ];
            then
              echo "Node ready"
              break
            fi
            sleep 2
          done
      - name: Wait until CoreDNS is ready
        run: |
          $HOME/.arkade/bin/kubectl rollout status deploy/coredns -n kube-system --timeout=300s
      - name: Explore nodes
        run: $HOME/.arkade/bin/kubectl get nodes -o wide
      - name: Explore pods
        run: $HOME/.arkade/bin/kubectl get pod -A -o wide
```

See also: [actuated-samples/k3sup-tester](https://github.com/actuated-samples/k3sup-tester/blob/master/.github/workflows/build.yml)

To run this on ARM64, just change the actuated label to `actuated-aarch64`.
