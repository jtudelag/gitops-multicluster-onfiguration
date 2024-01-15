# s3cmd-container

The repo `rhel-8-for-x86_64-baseos-rpms` must be enabled in the RHEL host in order to build this container image
because the s3cmd package has `python3-magic` as a dependency.

# Build the container

To build the container image, run the following command:

```bash
podman build -f Containerfile -t localhost/s3cmd:latest
```
