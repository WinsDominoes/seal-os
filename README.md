# Seal OS &nbsp; [![bluebuild build badge](https://github.com/winsdominoes/seal-os/actions/workflows/build.yml/badge.svg)](https://github.com/winsdominoes/seal-os/actions/workflows/build.yml)

A nice little ublue based distro for my liking. 

## Installationwww

> **Warning**  
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable), try at your own discretion.

To rebase an existing atomic Fedora installation to the latest build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/winsdominoes/seal-os:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/winsdominoes/seal-os:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `recipe.yml`, so you won't get accidentally updated to the next major version.

## ISO
### Docker
```bash
# ISO command:
mkdir ./iso-output
sudo docker run --rm --privileged --volume ./iso-output:/build-container-installer/build --pull=always \
ghcr.io/jasonn3/build-container-installer:latest \
# ISO config:
IMAGE_REPO=ghcr.io/winsdominoes \
IMAGE_NAME=seal-os \
IMAGE_TAG=latest \
VARIANT=bluefin-dx # should match the variant your image is based on
```
### Podman
```bash
# ISO command:
mkdir ./iso-output
sudo podman run --rm --privileged --volume ./iso-output:/build-container-installer/build --security-opt label=disable --pull=newer \
ghcr.io/jasonn3/build-container-installer:latest \
# iso config:
IMAGE_REPO=ghcr.io/winsdominoes \
IMAGE_NAME=seal-os \
IMAGE_TAG=latest \
VARIANT=bluefin-dx
```

### Fedora Atomic
[https://blue-build.org/learn/universal-blue/#fresh-install-from-an-iso](https://blue-build.org/learn/universal-blue/#fresh-install-from-an-iso)

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/winsdominoes/seal-os
```

## Documentation

[BlueBuild docs](https://blue-build.org/how-to/setup/)
