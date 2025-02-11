# github.com/tiredofit/docker-wyoming

[![GitHub release](https://img.shields.io/github/v/tag/tiredofit/docker-wyoming?style=flat-square)](https://github.com/tiredofit/docker-wyoming/releases)
[![Build Status](https://img.shields.io/github/workflow/status/tiredofit/docker-wyoming/build?style=flat-square)](https://github.com/tiredofit/docker-wyoming/actions?query=workflow%3Abuild)
[![Docker Stars](https://img.shields.io/docker/stars/tiredofit/wyoming.svg?style=flat-square&logo=docker)](https://hub.docker.com/r/tiredofit/wyoming/)
[![Docker Pulls](https://img.shields.io/docker/pulls/tiredofit/wyoming.svg?style=flat-square&logo=docker)](https://hub.docker.com/r/tiredofit/wyoming/)
[![Become a sponsor](https://img.shields.io/badge/sponsor-tiredofit-181717.svg?logo=github&style=flat-square)](https://github.com/sponsors/tiredofit)
[![Paypal Donate](https://img.shields.io/badge/donate-paypal-00457c.svg?logo=paypal&style=flat-square)](https://www.paypal.me/tiredofit)

## About

This will build a Docker Image for [Wyoming](https:///), A series of utilities to support voice processing, either speech to text (STT) or text to speech (TTS).

## Maintainer

- [Dave Conroy](https://github.com/tiredofit/)

## Table of Contents

- [About](#about)
- [Maintainer](#maintainer)
- [Table of Contents](#table-of-contents)
- [Installation](#installation)
  - [Build from Source](#build-from-source)
  - [Prebuilt Images](#prebuilt-images)
    - [Multi Architecture](#multi-architecture)
- [Configuration](#configuration)
  - [Quick Start](#quick-start)
  - [Persistent Storage](#persistent-storage)
  - [Environment Variables](#environment-variables)
    - [Base Images used](#base-images-used)
    - [Container Options](#container-options)
    - [OpenWakeWord Options](#openwakeword-options)
    - [Piper Options](#piper-options)
    - [Whisper Options](#whisper-options)
  - [Networking](#networking)
- [Maintenance](#maintenance)
  - [Shell Access](#shell-access)
- [Support](#support)
  - [Usage](#usage)
  - [Bugfixes](#bugfixes)
  - [Feature Requests](#feature-requests)
  - [Updates](#updates)
- [License](#license)
- [References](#references)


## Installation
### Build from Source
Clone this repository and build the image with `docker build -t (imagename) .`

### Prebuilt Images
Builds of the image are available on [Docker Hub](https://hub.docker.com/r/tiredofit/wyoming).

```
docker pull tiredofit/wyoming:(imagetag)
```

Builds of the image are also available on the [Github Container Registry](https://github.com/tiredofit/wyoming/pkgs/container/wyoming)

```
docker pull ghcr.io/tiredofit/docker-wyoming:(imagetag)
```

The following image tags are available along with their tagged release based on what's written in the [Changelog](CHANGELOG.md):

| Container OS | Tag       |
| ------------ | --------- |
| Debian       | `:latest` |

#### Multi Architecture
Images are built primarily for `amd64` architecture, and may also include builds for `arm/v7`, `arm64` and others. These variants are all unsupported. Consider [sponsoring](https://github.com/sponsors/tiredofit) my work so that I can work with various hardware. To see if this image supports multiple architecures, type `docker manifest (image):(tag)`

## Configuration

### Quick Start

* The quickest way to get started is using [docker-compose](https://docs.docker.com/compose/). See the examples folder for a working [compose.yml](examples/compose.yml) that can be modified for development or production use.

* Set various [environment variables](#environment-variables) to understand the capabilities of this image.
* Map [persistent storage](#data-volumes) for access to configuration and data files for backup.

### Persistent Storage

The following directories are used for configuration and can be mapped for persistent storage.

| Directory | Description |
| --------- | ----------- |
| `/data`   | Data        |
| `/logs`   | Logs        |


* * *
### Environment Variables

#### Base Images used

This image relies on an [Debian Linux](https://hub.docker.com/r/tiredofit/debian) base image that relies on an [init system](https://github.com/just-containers/s6-overlay) for added capabilities. Outgoing SMTP capabilities are handlded via `msmtp`. Individual container performance monitoring is performed by [zabbix-agent](https://zabbix.org). Additional tools include: `bash`,`curl`,`less`,`logrotate`,`nano`.

Be sure to view the following repositories to understand all the customizable options:

| Image                                                  | Description                            |
| ------------------------------------------------------ | -------------------------------------- |
| [OS Base](https://github.com/tiredofit/docker-debian/) | Customized Image based on Debian Linux |


#### Container Options
| Variable    | Description                            | Default  | `_FILE` |
| ----------- | -------------------------------------- | -------- | ------- |
| `MODE`      | `OPENWAKEWORD` `PIPER` `WHISPER` `ALL` | `ALL`    |         |
| `DATA_PATH` | Data Path                              | `/data`  |         |
| `LOG_PATH`  | Log Path                               | `/logs/` |         |

#### OpenWakeWord Options

| Variable                     | Description | Default                             | `_FILE` |
| ---------------------------- | ----------- | ----------------------------------- | ------- |
| `OPENWAKEWORD_DATA_PATH`     |             | `${DATA_PATH}/openwakeword/`        |         |
| `OPENWAKEWORD_LISTEN_IP`     |             | `0.0.0.0`                           |         |
| `OPENWAKEWORD_LISTEN_PORT`   |             | `10400`                             |         |
| `OPENWAKEWORD_LISTEN_TYPE`   |             | `TCP`                               |         |
| `OPENWAKEWORD_LOG_FILE`      |             | `openwakeword.log`                  |         |
| `OPENWAKEWORD_LOG_FORMAT`    |             | `YYYY-MM-DDTHH:mm:ss`               |         |
| `OPENWAKEWORD_LOG_LEVEL`     |             | `info`                              |         |
| `OPENWAKEWORD_LOG_PATH`      |             | `${LOG_PATH}/openwakeword/`         |         |
| `OPENWAKEWORD_LOG_TYPE`      |             | `both`                              |         |
| `OPENWAKEWORD_MODEL`         |             | `hey_jarvis`                        |         |
| `OPENWAKEWORD_MODEL_PATH`    |             | `${OPENWAKEWORD_DATA_PATH}/models/` |         |
| `OPENWAKEWORD_OUTPUT_PATH`   |             | `${OPENWAKEWORD_DATA_PATH}/output/` |         |
| `OPENWAKEWORD_THRESHOLD`     |             | `0.5`                               |         |
| `OPENWAKEWORD_TRIGGER_LEVEL` |             | `1`                                 |         |

#### Piper Options

| Variable              | Description | Default               | `_FILE` |
| --------------------- | ----------- | --------------------- | ------- |
| `PIPER_DATA_PATH`     |             | `${DATA_PATH}/piper/` |         |
| `PIPER_LISTEN_IP`     |             | `0.0.0.0`             |         |
| `PIPER_LISTEN_PORT`   |             | `10200`               |         |
| `PIPER_LISTEN_TYPE`   |             | `TCP`                 |         |
| `PIPER_LOG_FILE`      |             | `piper.log`           |         |
| `PIPER_LOG_FORMAT`    |             | `YYYY-MM-DDTHH:mm:ss` |         |
| `PIPER_LOG_LEVEL`     |             | `INFO`                |         |
| `PIPER_LOG_PATH`      |             | `${LOG_PATH}/piper/`  |         |
| `PIPER_LOG_TYPE`      |             | `both`                |         |
| `PIPER_VOICE`         |             | `en_US-lessac-medium` |         |
| `PIPER_SPEAKER`       |             | `0`                   |         |
| `PIPER_LENGTH_SCALE`  |             | `1.0`                 |         |
| `PIPER_NOISE_SCALE`   |             | `0.667`               |         |
| `PIPER_NOISE_WEIGHT`  |             | `0.333`               |         |
| `PIPER_PROCESS_LIMIT` |             | `1`                   |         |
| `PIPER_UPDATE_VOICES` |             | `TRUE`                |         |

#### Whisper Options

| Variable                 | Description | Default                                                            | `_FILE` |
| ------------------------ | ----------- | ------------------------------------------------------------------ | ------- |
| `WHISPER_DATA_PATH`      |             | `${DATA_PATH}/whisper/`                                            |         |
| `WHISPER_LISTEN_IP`      |             | `0.0.0.0`                                                          |         |
| `WHISPER_LISTEN_PORT`    |             | `10300`                                                            |         |
| `WHISPER_LISTEN_TYPE`    |             | `TCP`                                                              |         |
| `WHISPER_LOG_FILE`       |             | `whisper.log`                                                      |         |
| `WHISPER_LOG_FORMAT`     |             | `YYYY-MM-DDTHH:mm:ss`                                              |         |
| `WHISPER_LOG_LEVEL`      |             | `INFO`                                                             |         |
| `WHISPER_LOG_PATH`       |             | `${LOG_PATH}/whisper/`                                             |         |
| `WHISPER_LOG_TYPE`       |             | `both`                                                             |         |
| `WHISPER_LANGUAGE`       |             | `en`                                                               |         |
| `WHISPER_INITIAL_PROMPT` |             | ` `                                                                |         |
| `WHISPER_DOWNLOAD_PATH`  |             | `${WHISPER_DATA_PATH}/download/`                                   |         |
| `WHISPER_BEAM_SIZE`      |             | `1`                                                                |         |
| `WHISPER_MODEL`          |             | `tiny-int8`                                                        |         |
| `WHISPER_DEVICE`         |             | `CPU`                                                              |         |


### Networking

| Port    | Protocol | Description  |
| ------- | -------- | ------------ |
| `10200` | `tcp`    | Piper        |
| `10300` | `tcp`    | Whisper      |
| `10400` | `tcp`    | OpenWakeWord |

## Maintenance
### Shell Access

For debugging and maintenance purposes you may want access the containers shell.

```bash
docker exec -it (whatever your container name is) bash
```
## Support

These images were built to serve a specific need in a production environment and gradually have had more functionality added based on requests from the community.
### Usage
- The [Discussions board](../../discussions) is a great place for working with the community on tips and tricks of using this image.
- Consider [sponsoring me](https://github.com/sponsors/tiredofit) for personalized support.
### Bugfixes
- Please, submit a [Bug Report](issues/new) if something isn't working as expected. I'll do my best to issue a fix in short order.

### Feature Requests
- Feel free to submit a feature request, however there is no guarantee that it will be added, or at what timeline.
- Consider [sponsoring me](https://github.com/sponsors/tiredofit) regarding development of features.

### Updates
- Best effort to track upstream changes, More priority if I am actively using the image in a production environment.
- Consider [sponsoring me](https://github.com/sponsors/tiredofit) for up to date releases.

## License
MIT. See [LICENSE](LICENSE) for more details.

## References

* <[https://](https:///)>
