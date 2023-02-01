# Example: Multi-arch with buildx

This example is taken from the Open Source [inlets-operator](https://github.com/inlets/inlets-operator)

It builds a container image using a Dockerfile in the root of the repository and publishes it for a number of architectures - Arm, Intel, etc to GitHub's Container Registry (GHCR). The action itself is able to authenticate to GHCR using a built-in, short-lived token. This is dependent on the "permissions" section and "packages: write" being set.

View [publish.yaml](https://github.com/inlets/inlets-operator/blob/master/.github/workflows/publish.yaml), adapted for actuated:

```diff
name: publish

on:
  push:
    tags:
      - '*'

jobs:
  publish:
    permissions:
      actions: read
      checks: write
      contents: read
      issues: read
      packages: write
      pull-requests: read
      repository-projects: read
      statuses: read

-   runs-on: ubuntu-latest
+   runs-on: actuated
    steps:
      - uses: actions/checkout@master
        with:
          fetch-depth: 1

+     - name: Setup mirror
+       uses: self-actuated/hub-mirror@master
      - name: Get TAG
        id: get_tag
        run: echo TAG=${GITHUB_REF#refs/tags/} >> $GITHUB_ENV
      - name: Get Repo Owner
        id: get_repo_owner
        run: echo "REPO_OWNER=$(echo ${{ github.repository_owner }} | tr '[:upper:]' '[:lower:]')" > $GITHUB_ENV

      - name: Set up QEMU
        uses: docker/setup-qemu-action@v2
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      - name: Login to container Registry
        uses: docker/login-action@v2
        with:
          username: ${{ github.repository_owner }}
          password: ${{ secrets.GITHUB_TOKEN }}
          registry: ghcr.io

      - name: Release build
        id: release_build
        uses: docker/build-push-action@v3
        with:
          outputs: "type=registry,push=true"
          platforms: linux/amd64,linux/arm/v6,linux/arm64
          build-args: |
            Version=${{  env.TAG }}
            GitCommit=${{ github.sha }}
          tags: |
            ghcr.io/${{ env.REPO_OWNER }}/inlets-operator:${{ github.sha }}
            ghcr.io/${{ env.REPO_OWNER }}/inlets-operator:${{ env.TAG }}
            ghcr.io/${{ env.REPO_OWNER }}/inlets-operator:latest
```

You'll see that we added a `Setup mirror` step, this explained in the [Registry Mirror example](/examples/registry-mirror)

The `docker/build-push-action@v3` step is responsible for passing in a number of platform combinations such as: `linux/amd64` for cloud, `linux/arm64` for Arm servers and `linux/arm/v6` for Raspberry Pi.

Within [the Dockerfile](https://github.com/inlets/inlets-operator/blob/master/Dockerfile), we needed to make a couple of changes.

You can pick to run the step in either the BUILDPLATFORM or TARGETPLATFORM. The BUILDPLATFORM is the native architecture and platform of the machine performing the build, this is usually amd64. The TARGETPLATFORM is important for the final step of the build, and will be injected based upon one each of the platforms you have specified in the step.

```diff
- FROM golang:1.18 as builder
+ FROM --platform=${BUILDPLATFORM:-linux/amd64} golang:1.18 as builder
```

For Go specifically, we also updated the `go build` command to tell Go to use cross-compilation based upon the TARGETOS and TARGETARCH environment variables, which are populated by Docker.

```diff
GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -o inlets-operator
```

Learn more in the Docker Documentation: [Multi-platform images](https://docs.docker.com/build/building/multi-platform/)

## Need a hand with GitHub Actions?

Check your plan to see if access to Slack is included, if so, you can contact us on Slack for help and guidance.
