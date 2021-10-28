# Ambianic Edge  [![GitHub Repo stars](https://img.shields.io/github/stars/ambianic/ambianic-edge?style=social)](https://github.com/ambianic/ambianic-edge)

[![Join the Slack chat room](https://img.shields.io/badge/Slack-Join%20the%20chat%20room-blue)](https://join.slack.com/t/ambianicai/shared_invite/zt-eosk4tv5-~GR3Sm7ccGbv1R7IEpk7OQ)

[Ambianic Edge](https://github.com/ambianic/ambianic-edge) is an Open Source software project that turns your inexpensive home computer (Raspberry Pi or an old laptop) into a smart camera device.
We recommend running Ambianic Edge on [Ambianic Box](https://docs.ambianic.ai/users/ambianicbox/) hardware but you can also install and run it on most ARM on x86 computers.

# Quick Setup

We are meticulously focused on a simplified and streamlined DIY user experience from hardware assembly to mobile app. However we welcome advanced users to bring in their own components as needed.

## Hardware

The easiest way to start is by assembling a new Ambianic Box from inexpensive off the shelf components. Just follow [this step by step guide](https://docs.ambianic.ai/users/ambianicbox/).

## Software

Follow the step by step instructions for a quick software install [available here](https://docs.ambianic.ai/users/installsoftware/).

Once installed and running, you can connect to an Ambianic Edge via the [UI app](https://ui.ambianic.ai/) and see a timeline of detection events.

# Advanced Setup

## Hardware

You can use your own home computer as long as it has at least as much computing power as the reference hardware used for an [Ambianic Box](https://docs.ambianic.ai/users/ambianicbox/#components)

## Software

Follow these Advanced Install and CLI access instructions [available here](https://docs.ambianic.ai/users/quickstart/#advanced-install).

# Configuration

Some configuration features are available in the UI but more advanced configuration requires logging into the Ambianic Edge device via `ssh` and editing its `config.yaml` file. [Read more](https://docs.ambianic.ai/users/configure/) about advanced configuration.

# How to run in development mode

If you are interested to try the development version, follow [this guide](https://docs.ambianic.ai/developers/development-environment/).

[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/ambianic/ambianic-edge)


# Project Status and Stats

[![Docker pulls](https://img.shields.io/docker/pulls/ambianic/ambianic-edge?color=dark-green&label=Downloads)](https://hub.docker.com/r/ambianic/ambianic-edge)
![CI Build](https://github.com/ambianic/ambianic-edge/workflows/Ambianic%20Edge%20CI/badge.svg)
[![codecov](https://codecov.io/gh/ambianic/ambianic-edge/branch/master/graph/badge.svg?token=JJlxaW5flS)](https://codecov.io/gh/ambianic/ambianic-edge)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fambianic%2Fambianic-edge.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fambianic%2Fambianic-edge?ref=badge_shield)
[![CodeFactor](https://www.codefactor.io/repository/github/ambianic/ambianic-edge/badge)](https://www.codefactor.io/repository/github/ambianic/ambianic-edge)
![CodeQL](https://github.com/ambianic/ambianic-edge/workflows/CodeQL/badge.svg)
![Container Security Scan](https://github.com/ambianic/ambianic-edge/workflows/Container%20Security%20Scan/badge.svg)
[![Language grade: Python](https://img.shields.io/lgtm/grade/python/g/ambianic/ambianic-edge.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/ambianic/ambianic-edge/context:python)
[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/5198/badge)](https://bestpractices.coreinfrastructure.org/projects/5198)

# Acknowledgements

This project has been inspired by the prior work of many bright people. Special gratitude to:
* [Yong Tang](https://github.com/yongtang) for his guidance as Tensorflow SIG IO project lead
* [Robin Cole](https://github.com/robmarkcole) for his invaluable insights and code on home automation AI with Home Assistant
* [Blake Blackshear](https://github.com/blakeblackshear) for his work on Frigate and vision for the home automation AI space
