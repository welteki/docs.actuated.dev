# Example: Custom VM sizes

Our team will have configured your servers so that they always launch a pre-defined VM size, this keeps the user experience simple and predictable.

However, you can also request a specific VM size with up to 32vCPU and as much RAM as is available in the server. vCPU can be over-committed safely, however over-committing on RAM is not advised because if all of the RAM is required, one of the running VMs may exit or be terminated.

Certified for:

- [x] `x86_64`
- [x] `arm64` including Raspberry Pi 4

## Request a custom VM size

To use the pre-configured size:

* `actuated`
* `actuated-arm64`

For a custom size just append `-cpu-` and `-gb` to the above labels, for example:

*x86_64* example:

* `actuated-1cpu-2gb`
* `actuated-4cpu-16gb`

*64-bit Arm example*:

* `actuated-arm64-4cpu-16gb`
* `actuated-arm64-32cpu-64gb`

You can change vCPU and RAM independently, there are no set combinations, so you can customise both to whatever you like.

The upper limit for vCPU is 32.

Create a new file at: `.github/workflows/build.yml` and commit it to the repository.

```yaml
name: specs

on: push
jobs:
  specs:
    runs-on: actuated-1cpu-2gb
    steps:
      - name: Print specs
        run: |
            nproc
            free -h
```

This will allocate 1x vCPU and 2GB of RAM to the VM. To run this same configuration for arm64, change `runs-on` to `actuated-arm64-1cpu-2gb`.

