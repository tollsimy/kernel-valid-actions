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