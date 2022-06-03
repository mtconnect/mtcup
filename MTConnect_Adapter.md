---
title: Adapter
description: 
published: true
date: 2022-06-03T11:17:28.782Z
tags: 
editor: markdown
dateCreated: 2022-06-03T11:14:50.014Z
---

# Adapter

## Overview

Adapter is an optional piece of hardware or software that transforms information provided by a device into a form that can be received by an MTConnect Agent. The MTConnect standard does not specify or restrict the behavior or implementation of the adapter.

For many systems, the adapter that translates device data into MTConnect is a software-only solution:

- For example, installing an MTConnect adapter for a Windows-based CNC controller might follow the same steps as installing drivers for desktop PC peripherals like printers or digital cameras. 

If a device does not have a controller, or if the controller is unable to run the adapter software, an additional hardware board is installed. The adapter software then runs on the external board, e.g. on a Raspberry Pi.

- In some cases a separate board is utilized for performance optimization and system resource management.

In most cases, there is a unique adapter for every unique device model. This is because most devices have their own proprietary data format that needs to be mapped or translated to the MTConnect data format.

## Adapter Connectivity

The ingress to the C++ MTConnect Agent requires adapters to communicate with it using a simple TCP sockets protocol. The semantics were chosen as the simplest possible format to get data from a device to another process with minimal overhead, but still easily verifiable. See [Adapter Connectivity.](/Agent-Adapter-Connectivity "wikilink")

## Adapter Examples

There are Adapter examples on MTConnect GitHub that one may follow and could use as a template to create their own adapters.
- [C++ Agent Adapter simulator](https://github.com/mtconnect/cppagent/blob/master/simulator/run_scenario.rb): Example in ruby.
- [Adapter Examples](https://github.com/mtconnect/adapter): Examples in several software languages.
- [PocketNC Adapter](https://github.com/mtconnect/PocketNC_adapter): Example in python2.7.
- [ISO Digital Twin Project](https://github.com/mtconnect/iso_digital_twin_adapter): Example in python3.
