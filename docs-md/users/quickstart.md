
# Quick Start Guide [![GitHub User's stars](https://img.shields.io/github/stars/ambianic?style=social)](https://github.com/ambianic)

[![Join the Slack chat room](https://img.shields.io/badge/Slack-Join%20the%20chat%20room-blue)](https://join.slack.com/t/ambianicai/shared_invite/zt-eosk4tv5-~GR3Sm7ccGbv1R7IEpk7OQ)

## Recommended Install

The simplest way to get started is to follow these steps:

- [Assemble Ambianic Box hardware](ambianicbox.md)
- [Install and run Ambianic software](installsoftware.md)

If you prefer an alternative hosting platform for your Ambianic Edge device, keep reading about more advanced installation options.

## Advanced Install

If you are familiar with Docker you will be able to install Ambianic Edge in less than 5 minutes on almost any computer with x86 or ARM architecture. Follow the steps below:

### Ambanic Edge Docker image

Ambianic Edge is
[available](https://hub.docker.com/r/ambianic/ambianic-edge) as a docker image for ARM and x86 architectures so it can be deployed
on most modern machines and operating systems.

The reference test system is:
[Raspberry Pi 4 with 4GB RAM and 32GB SD card](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/). 
_Although docker images are [available](https://hub.docker.com/r/ambianic/ambianic-edge/tags) for most common ARM and x86 machines._

To deploy on a Raspberry Pi 4, you will need a recent
[Raspbian install](https://www.raspberrypi.org/documentation/setup/) with
[Docker](https://www.freecodecamp.org/news/the-easy-way-to-set-up-docker-on-a-raspberry-pi-7d24ced073ef/)
and [Docker Compose](https://docs.docker.com/compose/) on it. 
You can install and run the image in the default pi user space on Raspbian.

#### Installer for Raspberry OS and Debian-like linux

We have a commodity script that will take care of installing and setting up your system. Run this line to setup 

```sh
wget -qO - https://raw.githubusercontent.com/ambianic/ambianic-quickstart/master/installer.sh | sh
```

After the setup you can find the installation under `/opt/ambianic` where you can find the configuration files and the data directory (under `/opt/ambianic/data`).

Ambianic Pipeline configuration will be under `/etc/ambianic`.

#### The Ambianic Edge CLI

The installer will start the service for you. To manage the runtime you can use the `ambianic` command line. A few examples:

- Start, Stop or Restart with `ambianic [ start | stop | restart ]`
- View the instance status with `ambianic status`
- View logs with `ambianic logs`
- Open the UI (if you system has a GUI) with `ambianic ui`
- Upgrade the installation with `ambianic upgrade`

### Ambianic UI app

[Ambianic UI](https://ui.ambianic.ai/) is a
modern prorgressive web application (PWA) that provides Plug-and-Play pairing and remote access
to Ambianic Edge devices.

[Read more](ambianicui.md) about Ambianic UI.

## Development

If you want to contribute to the code read how to [setup a development environment](../developers/development-environment.md)

## Troubleshooting

If you experience problems with your initial setup and you can't find a good solution online, feel free to engage the team on [Twitter](https://twitter.com/ambianicai), [Slack](https://ambianicai.slack.com/join/shared_invite/zt-eosk4tv5-~GR3Sm7ccGbv1R7IEpk7OQ#/), or [Github discussion forums](https://github.com/ambianic/ambianic-ui/discussions).

## Supporting the project

If you find value in this project, consider supporting its future success. See the [sustainability](https://docs.ambianic.ai/#sustainability) section.
 
