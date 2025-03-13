# Kernel Validation using GitHub Actions
Cross compilation x86_64 to riscv and validation of the kernel using GitHub Actions.

Workflow is big and chunky for simplicity. It can be broken down into smaller workflows.

## Running CI Locally
You can use `act` to run the CI locally.

### Podman as runtime
Note: Not easy to use `act` with Podman since most of the GitHub Actions are written for Docker.

### Docker as runtime
To run the CI locally, you can use the following command:
```bash
act -j build
```

## Workflow
Workflow is executed on GitHub "ubuntu-latest" runner, an x86_64 machine. We need to cross compile the kernel for riscv and run the tests on QEMU.

In order to cross-compile we can simply set the ARCH and CROSS_COMPILE environment variables.

The job runs on a ubuntu:20.04 container. We need to install the dependencies for the kernel compilation.
We also need to clone the repository to get the custom busybox defconfig (optional) and the init script.
The custom busybox config is necessary to correctly compile busybox for risc-v target. In particular, we need to disable SHA hw acceleration.

After installing all dependencies, we can clone and compile the Linux kernel. After that, we can compile kselftest for later use with `make -j$(nproc) kselftest-install`.

In order to provide the rootfs to the kernel, we create a simple initramfs containing risc-v fedora-container rootfs (or optionally busybox). We add also the init script and th just built selftests.
We then generate the cpio archive.

Finally, we run QEMU with the kernel and the initramfs as well as openSBI firmware. 