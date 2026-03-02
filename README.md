# mattermost-arm64

Automated ARM64 Docker image builder for Mattermost Team Edition.

Built for running Mattermost natively on Apple Silicon (M1/M2/M3/M4) Macs.

## Why

The official Mattermost Docker image only supports AMD64.
Running it on ARM64 requires QEMU emulation, which is slow and unreliable.

This repository uses GitHub Actions to check for the latest version daily
and automatically builds a Docker image using the official ARM64 binary.

## Usage

```bash
docker pull ghcr.io/jdjin0511/mattermost-arm64:latest
```

Specific version:

```bash
docker pull ghcr.io/jdjin0511/mattermost-arm64:11.4.2
```

### With mattermost/docker

Since the [official Docker deployment repo](https://github.com/mattermost/docker) uses `mattermost/${MATTERMOST_IMAGE}:${MATTERMOST_IMAGE_TAG}` as the image reference, create a compose override:

```yaml
# docker-compose.arm64.yml
services:
  mattermost:
    image: ghcr.io/jdjin0511/mattermost-arm64:latest
```

Then run:

```bash
docker compose -f docker-compose.yml -f docker-compose.without-nginx.yml -f docker-compose.arm64.yml up -d
```

## How it works

- **Trigger**: Daily at UTC 00:00 + manual dispatch
- **Runner**: `ubuntu-24.04-arm` (native ARM64)
- **Source**: Official ARM64 binary from [releases.mattermost.com](https://releases.mattermost.com)
- **Base**: Official Dockerfile structure (multi-stage, distroless runtime)
- **Registry**: GitHub Container Registry (GHCR)

## Local build

```bash
docker build --build-arg MM_VERSION=11.4.2 -t mattermost-arm64 .
```
