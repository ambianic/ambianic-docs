
# Ambianic UI app [![GitHub Repo stars](https://img.shields.io/github/stars/ambianic/ambianic-ui?style=social)](https://github.com/ambianic/ambianic-ui)


[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/ambianic/ambianic-ui)
[![Join the Slack chat room](https://img.shields.io/badge/Slack-Join%20the%20chat%20room-blue)](https://join.slack.com/t/ambianicai/shared_invite/zt-eosk4tv5-~GR3Sm7ccGbv1R7IEpk7OQ)

[![npm downloads](https://img.shields.io/npm/dt/ambianic-ui)](https://www.npmjs.com/package/ambianic-ui)
![CI for Ambianic UI](https://github.com/ambianic/ambianic-ui/workflows/CI%20for%20Ambianic%20UI/badge.svg)
[![npm version](https://badge.fury.io/js/ambianic-ui.svg)](https://badge.fury.io/js/ambianic-ui)
[![Netlify Status](https://api.netlify.com/api/v1/badges/70a4b25a-2765-4d6b-bbd8-a7e5dd6e98cb/deploy-status)](https://app.netlify.com/sites/happy-franklin-69b6d4/deploys)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fambianic%2Fambianic-ui.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fambianic%2Fambianic-ui?ref=badge_shield) 
[![CodeFactor](https://www.codefactor.io/repository/github/ambianic/ambianic-ui/badge)](https://www.codefactor.io/repository/github/ambianic/ambianic-ui)
![Lighthouse PWA CI](https://github.com/ambianic/ambianic-ui/workflows/Lighthouse%20CI/badge.svg)
[![codecov](https://codecov.io/gh/ambianic/ambianic-ui/branch/master/graph/badge.svg?token=nVlrMWiZ4q)](https://codecov.io/gh/ambianic/ambianic-ui)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)


[Ambianic UI](https://ui.ambianic.ai/) is a
modern prorgressive web application (PWA) that provides Plug-and-Play pairing and remote access
to an Ambianic Edge device.

<img src="https://github.com/ambianic/ambianic-ui/raw/master/public/img/ambianic-pwa-badge.png" width="600">

Below is an example screenshot timeline of events detected by a connected Ambianic Edge device.

![Timeline](../assets/images/timeline-screen.png)

&nbsp;

The app is explicitly designed with user privacy and data ownership in mind:

* Stores data exclusively on user's own client device (desktop or mobile).
* Does not store ANY user information in the cloud.
* User may explicitly chose to store a backup of their data on a server of their choice.
* User data remains 100% owned and controlled by the user at all times.
* No fine print. No obscure opt-in / opt-out pop-ups of any kind.

# Project Status

A hosted version of the app is now available to install here: [https://ui.ambianic.ai/](https://ui.ambianic.ai/)

Example screenshots on mobile and desktop:
 
<img src="https://github.com/ambianic/ambianic-ui/raw/master/docs/img/ambianic-ui-mobile-screenshot.png" width="300" alt="ambianic UI mobile page example">

&nbsp;

<img src="https://github.com/ambianic/ambianic-ui/raw/master/docs/img/ambianic-ui-dekstop-screenshot.png" width="600" alt="ambianic UI mobile page example">

# Cloud deployment

This project is deployed on Netlify by default and is available at https://ui.ambianic.ai . 
If you prefer to launch your own deployment from this repo, click the button below:

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/ambianic/ambianic-ui)

# Development Environment

For instructions how to build, test and debug Ambianic UI via a virtual Continuous Dev Environment or a local dev environment, see [this guide](https://docs.ambianic.ai/developers/development-environment/).


# Connecting to an Ambianic Edge device

## Local Discovery

You can easily discover and connect to an Ambianic Box that runs on your local WiFi network. It works similar to Airdrop.

## Remote Connection

You can also connect to an Ambinic Edge device running on a remote network as long as you have access to the device' unique Peer ID.

## Steps

Go to Settings and follow the steps for adding a new device.

After a few moments, pairing will conclude and you will see the unique identifier of your Ambianic Edge device. 

Give your device a unique friendly name such as Entry Area or Front Door.

You can now head to the Timeline view and you will be able to see an image of what is in front of the Ambianic Box. The box is pre-configured to detect people and cars.

The pairing information is persisted on your Ambianic UI client device and you can now access Ambianic Edge from anywhere remotely! The connection is direct and encrypted end-to-end.

When you are ready to explore more advanced capabilities, continue to the next section.

# Configuration

Ambianic provides flexible configuration options via a configuration YAML file. You can customize: pipelines, input sources, AI models, notification channels and more.

Read on: [Configuring Ambianic Edge](configure.md).

# Community Support 

If you have questions, ideas or cool projects you'd like to share with the Ambianic team and community, please use the [Ambianic Twitter channel](https://twitter.com/ambianicai).

# Acknowledgements

*  This project's initial version was inspired by
[David Garoro](https://github.com/davidgaroro)'s [PWA example](https://github.com/davidgaroro/vuetify-todo-pwa).
*  The project relies heavily on the awesome [Vue.js](https://vuejs.org/) framework with [Vuetify](https://vuetifyjs.com/en/) material design components.

