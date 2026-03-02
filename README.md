# mattermost-arm64

Mattermost Team Edition ARM64 Docker 이미지 자동 빌드.

Apple Silicon (M1/M2/M3/M4) Mac에서 네이티브로 Mattermost를 실행하기 위한 프로젝트.

## 왜 필요한가

Mattermost 공식 Docker 이미지는 AMD64만 제공한다.
ARM64 환경에서는 QEMU 에뮬레이션으로 실행해야 하는데, 이는 느리고 불안정하다.

이 저장소는 GitHub Actions로 매일 최신 버전을 확인하고,
공식 ARM64 바이너리를 사용해 Docker 이미지를 자동으로 빌드한다.

## 사용법

```bash
docker pull ghcr.io/jdjin0511/mattermost-arm64:latest
```

특정 버전:

```bash
docker pull ghcr.io/jdjin0511/mattermost-arm64:11.4.2
```

### mattermost/docker와 함께 사용

[공식 Docker 배포 저장소](https://github.com/mattermost/docker)의 `.env`에서 이미지를 변경:

```env
MATTERMOST_IMAGE=ghcr.io/jdjin0511/mattermost-arm64
MATTERMOST_IMAGE_TAG=latest
```

> `docker-compose.yml`에서 이미지 참조 형식이 `mattermost/${MATTERMOST_IMAGE}:${MATTERMOST_IMAGE_TAG}`로 되어 있다면,
> 전체 이미지 경로를 직접 지정하도록 compose override를 사용한다.

## 빌드 방식

- **트리거**: 매일 UTC 00:00 + 수동 실행 가능
- **러너**: `ubuntu-24.04-arm` (네이티브 ARM64)
- **소스**: [releases.mattermost.com](https://releases.mattermost.com)의 공식 ARM64 바이너리
- **베이스**: 공식 Dockerfile 구조 기반 (multi-stage, distroless runtime)
- **레지스트리**: GitHub Container Registry (GHCR)

## 수동 빌드

```bash
docker build --build-arg MM_VERSION=11.4.2 -t mattermost-arm64 .
```
