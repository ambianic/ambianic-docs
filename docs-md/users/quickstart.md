
# 5 Minute Quick Start

Ambianic's goal is to provide helpful and actionable suggestions
in the context of home and business automation.

Ambianic has two major components: Ambianic UI and Ambianic Edge.

Ambianic UI is pre-deployed and readily available at [https://ui.ambianic.ai](https://ui.ambianic.ai). If you are familiar with Docker you will be able to install Ambianic Edge in less than 5 minutes. Let's go step by step.

## Ambanic Edge

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

### Fast setup

We are working on a script to ease the installation of Ambianic.ai. It will install `docker`, `docker-compose` and the Ambianic Edge.

Assuming a clean install on Raspberry Pi (or debian-like system), open a terminal and run the command

```sh
wget -qO - https://gist.githubusercontent.com/muka/725e07f26b8ac5f3e4f03bce5f2e8b45/raw/b5a1b3e812da7f75885e1d27b239a29730f665b3/setup.sh | sh
```

If everything runs well, run `sudo docker-compose up` to start the Ambianic Edge otherwise jump to the #install-setup channel on Slack and we'll find a solution.

### Starting Ambianic Edge

Here is a default [docker-compose.yaml](https://gist.github.com/ivelin/3891a7b5d61a12d6a1b9f652b6d53dce) file that you can start with.
It takes care of monitoring the health of Ambianic Edge, restarting when necessary and also graceful automatic updates to the latest release within 5 minutes.

_Note: The very first time you start Ambianic Edge it may take a minute or two. You will then see an error message that configuration file could not be found. Worry not. We will address that in due course. Let's get the base system running first. Then we will come back to configuration settings._

## Ambanic UI

[Ambianic UI](https://ui.ambianic.ai/) is a
modern prorgressive web application (PWA) that provides Plug-and-Play pairing and remote access
to an Ambianic Edge device.

Ambianic UI shows a timeline view with
important events around your home organized chronologically. Below is an example
timeline screenshot.

![Timeline](../assets/images/timeline-screen.png)

## Pairing Ambanic UI with Ambianic Edge

You can easily pair up your Ambianic UI with Ambianic Edge. It works similar to Airdrop.

Make sure to open Ambianic UI on the same local network where Ambianic Edge runs. Ambianic UI will initially display a welcome screen.

![Home Screen](../assets/images/home-screen.png)

If you click on Settings, it will show  a pairing page like the one below:

![Pairing](../assets/images/pairing-screen.png)

After a few moments, pairing will conclude and you will see the unique identifier of your Ambianic Edge device.

![Connected](../assets/images/connected-screen.png)

Congratulations! Your Ambianic instance is now up and running!

## Configuration

You are now ready to configure Ambianic: pipelines, input sources and AI models.

Read on: [Configuring Ambianic Edge](configure.md).

Once you configure Ambianic Edge and restart the docker image, you should be able to see timeline view like the one above.

The pairing information is persisted on your Ambianic UI client device and you can now access Ambianic Edge from anywhere remotely! The connection is direct and encrypted end-to-end.

## Troubleshooting

If you experience problems with your initial setup and you can't find a good solution online, feel free to engage the team on [Twitter](https://twitter.com/ambianicai) or [open a github issue](https://github.com/ambianic).

## Supporting the project

If you find value in this project, consider supporting its future success. See the [sustainability](https://docs.ambianic.ai/#sustainability) section.
