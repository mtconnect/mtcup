---
title: Installing C++ Agent on Raspberry Pi
permalink: /Installing_C++_Agent_on_Raspberry_Pi/
---

## Dockerfile

You can quickly install the MTConnect Agent using the Docker image at
<https://hub.docker.com/r/raymondhub/mtconnect>. A Docker image is a
container with a preconfigured set of files and configuration; the
Dockerfile was created using the Agent install instructions in this Wiki
page.

## Overview

Installation procedure for running MTConnect Agent on Raspberry Pi 3
Model B with Raspbian 9.x (Debian GNU/Linux 9.x "stretch"). This is a
general guide and covers the basic configuration of the MTConnect
simulator.

## Bill of Materials

  - Raspberry Pi 3 Model B
  - micro USB cable (to power Raspberry Pi)
  - computer or USB wall wart (to power Raspberry Pi)
  - 16GB or micro SD card
  - ethernet cable
  - internet connection
  - keyboard
  - HDMI cable
  - monitor
  - screwdriver (optional for Pi case assembly)

## Setup Raspberry Pi

If you already have a micro SD card with Raspbian installed, skip this
section.

If starting with a blank micro SD card, follow the [Quick Start
Guide](https://projects.raspberrypi.org/en/projects/noobs-install/) and
subsequently the NOOBS Guide/Setup on RaspberryPi.org.

## Agent and Adapter

Everything needed for the agent and a simulated adapter are downloaded,
setup, and configured from the "Terminal" command line interface (CLI).

From this point forward, follow the instructions as written in
[Installing C++ Agent on
Ubuntu](/Installing_C++_Agent_on_Ubuntu "wikilink").