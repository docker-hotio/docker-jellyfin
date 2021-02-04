[<img src="https://hotio.dev/img/jellyfin.png" alt="logo" height="130" width="130">](https://github.com/jellyfin/jellyfin)

[![GitHub Source](https://img.shields.io/badge/github-source-ffb64c?style=flat-square&logo=github&logoColor=white&labelColor=757575)](https://github.com/hotio/jellyfin)
[![GitHub Registry](https://img.shields.io/badge/github-registry-ffb64c?style=flat-square&logo=github&logoColor=white&labelColor=757575)](https://github.com/orgs/hotio/packages/container/package/jellyfin)
[![Docker Pulls](https://img.shields.io/docker/pulls/hotio/jellyfin?color=ffb64c&style=flat-square&label=pulls&logo=docker&logoColor=white&labelColor=757575)](https://hub.docker.com/r/hotio/jellyfin)
[![Discord](https://img.shields.io/discord/610068305893523457?style=flat-square&color=ffb64c&label=discord&logo=discord&logoColor=white&labelColor=757575)](https://hotio.dev/discord)
[![Upstream](https://img.shields.io/badge/upstream-project-ffb64c?style=flat-square&labelColor=757575)](https://github.com/jellyfin/jellyfin)
[![Website](https://img.shields.io/badge/website-hotio.dev-ffb64c?style=flat-square&labelColor=757575)](https://hotio.dev/containers/jellyfin)

## Starting the container

CLI:

```shell
docker run --rm \
    --name jellyfin \
    -p 8096:8096 \
    -e PUID=1000 \
    -e PGID=1000 \
    -e UMASK=002 \
    -e TZ="Etc/UTC" \
    -e ARGS="" \
    -v /<host_folder_config>:/config \
    hotio/jellyfin
```

Compose:

```yaml
version: "3.7"

services:
  jellyfin:
    container_name: jellyfin
    image: hotio/jellyfin
    ports:
      - "8096:8096"
    environment:
      - PUID=1000
      - PGID=1000
      - UMASK=002
      - TZ=Etc/UTC
      - ARGS
    volumes:
      - /<host_folder_config>:/config
```

In most cases you'll need to add additional volumes, depending on your own personal preference, to get access to your files.

## Tags

| Tag                | Upstream                      | Version | Build |
| -------------------|-------------------------------|---------|-------|
| `release` (latest) | Releases                      | ![version](https://img.shields.io/badge/dynamic/json?color=f5f5f5&style=flat-square&label=&query=%24.version&url=https%3A%2F%2Fraw.githubusercontent.com%2Fhotio%2Fjellyfin%2Frelease%2FVERSION.json) | ![build](https://img.shields.io/github/workflow/status/hotio/jellyfin/build/release?style=flat-square&label=) |
| `nightly`          | Every commit to master branch | ![version](https://img.shields.io/badge/dynamic/json?color=f5f5f5&style=flat-square&label=&query=%24.version&url=https%3A%2F%2Fraw.githubusercontent.com%2Fhotio%2Fjellyfin%2Fnightly%2FVERSION.json) | ![build](https://img.shields.io/github/workflow/status/hotio/jellyfin/build/nightly?style=flat-square&label=) |

You can also find tags that reference a commit or version number.

## Configuration location

Your jellyfin configuration inside the container is stored in `/config/app`, to migrate from another container, you'd probably have to move your files from `/config` to `/config/app`. The following jellyfin path locations are used by default.

```shell
JELLYFIN_CONFIG_DIR="/config/app"
JELLYFIN_DATA_DIR="/config/app/data"
JELLYFIN_LOG_DIR="/config/app/log"
JELLYFIN_CACHE_DIR="/config/app/cache"
```

You can override these locations by setting them to a different value with a docker environment variable.

## Hardware support

To make your hardware devices available inside the container use the following argument `--device=/dev/dri:/dev/dri` for Intel QuickSync and `--device=/dev/dvb:/dev/dvb` for a tuner. NVIDIA users should go visit the [NVIDIA github](https://github.com/NVIDIA/nvidia-docker) page for instructions. For Raspberry Pi OpenMAX you'll need to use `--device=/dev/vchiq:/dev/vchiq -v /opt/vc/lib:/opt/vc/lib`, V4L2 will need `--device=/dev/video10:/dev/video10 --device=/dev/video11:/dev/video11 --device=/dev/video12:/dev/video12` and MMAL needs `--device=/dev/vcsm:/dev/vcsm` or `--device=/dev/vc-mem:/dev/vc-mem`.
