# Binfmt

[![GitHub release](https://img.shields.io/github/release/karellen/binfmt.svg?style=flat-square)](https://github.com/karellen/binfmt/releases/latest)
[![CI Status](https://img.shields.io/github/actions/workflow/status/karellen/binfmt/ci.yml?label=ci&logo=github&style=flat-square)](https://github.com/karellen/binfmt/actions?query=workflow%3Aci)
[![Go Report Card](https://goreportcard.com/badge/github.com/karellen/binfmt?style=flat-square)](https://goreportcard.com/report/github.com/karellen/binfmt)
<!--- [![Docker Pulls](https://img.shields.io/docker/pulls/tonistiigi/binfmt.svg?style=flat-square&logo=docker)](https://hub.docker.com/r/tonistiigi/binfmt/) --->

Cross-platform emulator collection distributed with Docker images.

# THIS IS A KARELLEN, INC. FORK OF [tonistiigi/binfmt](https://github.com/tonistiigi/binfmt).

No affiliation with or endorsement by Docker, Inc. Use at OWN risk.

## Build local binaries

```bash
docker buildx bake
```

This builds the qemu-user emulator binaries for your local plaform to the `bin` directory.

## Build test image

```bash
REPO=myuser/binfmt docker buildx bake --load mainline
docker run --privileged --rm myuser/binfmt
```

Prints similar to:

```
{
  "supported": [
    "linux/amd64",
    "linux/arm64",
    "linux/riscv64",
    "linux/ppc64le",
    "linux/s390x",
    "linux/386",
    "linux/arm/v7",
    "linux/arm/v6"
  ],
  "emulators": [
    "qemu-aarch64",
    "qemu-arm",
    "qemu-i386",
    "qemu-ppc64le",
    "qemu-riscv64",
    "qemu-s390x"
  ]
}
```

## Installing emulators

```bash
docker run --privileged --rm ghcr.io/karellen/binfmt --install all
docker run --privileged --rm ghcr.io/karellen/binfmt --install arm64,riscv64,arm
```

## Installing emulators from Docker-Compose

```docker
version: "3"
services:
  emulator:
    image: ghcr.io/karellen/binfmt
    container_name: emulator
    privileged: true
    command: --install all
    network_mode: bridge
    restart: "no"
```
Only use container `restart-policy` as `no`, otherwise docker will keep restarting the container.

## Uninstalling emulators

```bash
docker run --privileged --rm ghcr.io/karellen/binfmt --uninstall qemu-aarch64
```

Emulator names can be found from the status output.

You can also uninstall all archs for a specific emulator:

```bash
docker run --privileged --rm ghcr.io/karellen/binfmt --uninstall qemu-*
```

## Display version

```bash
docker run --privileged --rm ghcr.io/karellen/binfmt --version
```
```
binfmt/9a44d27 qemu/v6.0.0 go/1.15.11
```

## Development commands

```bash
# validate linter
./hack/lint

# validate vendored files
./hack/validate-vendor

# update vendored files
./hack/update-vendor

# test, only run on nodes where you allow emulators to be installed in kernel
./hack/install-and-test
```

## Test current emulation support

```
docker run --rm arm64v8/alpine uname -a
docker run --rm arm32v7/alpine uname -a
docker run --rm ppc64le/alpine uname -a
docker run --rm s390x/alpine uname -a
docker run --rm ghcr.io/karellen/debian:riscv uname -a
```

## `buildkit` target

This repository also provides helper for BuildKit's automatic emulation support https://github.com/moby/buildkit/pull/1528.
These binaries are BuildKit specific and should not be installed in kernel with `binfmt_misc`.

## Licenses

MIT. See `LICENSE` for more details.
For QEMU see https://wiki.qemu.org/License
