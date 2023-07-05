---
title: C++ Agent
description: 
published: true
date: 2023-07-05T13:40:34.023Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:30:40.556Z
---

# Reference MTConnect Agent

The MTConnect implementation that has been most closely tracking the MTConnect standard and is used in most production implementation is the C++ Agent that resides on the MTConnect project pages of github [C++ Agent](http://github.com/mtconnect/cppagent). 

As part of the effort, we also provide pre-built Windows x86 binaries that are statically linked and can be run on any Windows computer dating from around Windows XP SP2 and later. Earlier builds are possible, but these will be left up to the reader. The pre-built binaries can be found at: [C++ Agent
Releases](http://github.com/mtconnect/cppagent/releases).

The C++ Agent provides the fullest implementation of the standard available at the time of release. The version number (Major, Minor, Patch, Build) are tied to the MTConnect standard. The first three (Major, Minor, Build) are based directly on the MTConnect standard release nomenclature and the build number will increment as new versions of the C++ agent is released. When the agent is in pre-release for a upcoming version of the standard, it will be suffixed with a RCn where this stands for Release Candidate n as in RC1.

The agent has a rich level of configuration and is capable of serving up content from the local file system and allowing changes to be pushed. To get the full details on the configuration, again, visit the [C++ Agent github page](http://github.com/mtconnect/cppagent) and scroll down to the readme file. There are many features built in to the agent, some documented better than others. We will try to document some of the more interesting capabilities on these pages.

## Responsibilities

The MTConnect agent receives data from an adapter and transforms it as required by the MTConnect standard. The adapter communicates with the device's propriatary interface and delivers a simple pipe-delimited stream of data on change. The agent take the stream of data and associates it with the data model, performs any conversions, and then renders it in XML or JSON according to the standard. The agent exists to facilitate implemention by providing the protocol and transformat responsibilities away from the adapter. 

The agent main responsibility is data collection, transformation, and delivery to various sinks (destination protocols). The agent supports REST as the primary method using HTTP optionally with TLS (SSL) according to the MTConnect standard. Starting with version 2.0, various plugins are available for OPC UA, Kafka, InfluxDB, and MQTT.

## Quick Start

### Windows

* Download the pre-built Agent from [Releases](http://github.com/mtconnect/cppagent/releases)
* unzip the archive
  * The agent executable is in the `bin` directory with a sample `agent.cfg` file
  * The distribution comes with an example `Device.xml` file providing a description of the machine
 * Open a command prompt:
 ```cmd
       C:> cd cppagent-2.<version>-win64\bin
       C:> agent debug
```       
   > replace `<version>` with the release version number, such as `2.0.0.7`
 * Open a broswer and navigate to: http://localhost:5000/probe
 
### Linux


 ### Next...
 
 * Configure the agent for your machine and adapter
   * Modify the Device.xml with the components and data available from you adapter(s)
 * The new agent supports many incoming data feeds as follows:
   * Classic SHDR (pipe delimited. timestamped data)
   * MQTT
   * Another agent
   * Plugins available for OPC UA and other protocols.


## Usage and Configuration

[Agent Usage and Configuration](/Agent-Usage-and-Configuration "wikilink")

## Adapter Connectivity

[Adapter Connectivity](/Agent-Adapter-Connectivity "wikilink")


## Building the C++ Agent
  
The agent build is dependent on the following utilities:

- C++ Compiler compliant with C++ 17
- git is optional but suggested to download source and update when changes occur
- cmake for build generator and testing
- python 3 and pip to support conan for dependency and package management
- ruby and rake for mruby to support building the embedded scripting engine [not required if -o with_ruby=False]

  
### Raspberry Pi OS

[Installing C++ Agent on Raspberry Pi](/Installing_C++_Agent_on_Raspberry_Pi "wikilink")

### Ubuntu

[Installing C++ Agent on Ubuntu](/Installing_C++_Agent_on_Ubuntu "wikilink")

### Windows

[Installing C++ Agent on Windows](/Installing_C++_Agent_on_Windows "wikilink")
  
### Mac

[Installing C++ Agent on Mac](/Installing_C++_Agent_on_Mac "wikilink")
  
### Fedora Alpine

[Installing C++ Agent on Fedora Alpine](/Installing_C++_Agent_on_Fedora "wikilink")

## Change History

* Version 2.0.0: Rearchitecture of the agent with the following additions:
  * Full HTTP 1.1 support
  * TLS 1.2 and dual certificate support
  * Lighter weight runtime
  * QIF and Raw Material implicit support
  * File monitoring supports inplace updates of device data
  * Agent sources for aggregation
  * MQTT Source
  * Plug-in architecture to extend sources, sinks, and transformations
  * mruby embedded scripting for run-time defined transformations

* Version 1.8.0: No major changes

* Version 1.7.0: added kinematics, solid models, and new specifications types.

* Version 1.6.0: added coordinate systems, specifications, and tabular data.

* Version 1.5.0: added Data Set capabilities and has been updated to use C++ 14.

* Version 1.4.0: added time period filter constraint, compositions, initial values, and reset triggers.

* Version 1.3.0: added the filter constraints, references, cutting tool archetypes, and formatting styles.

* Version 1.2.0: added the capability to support assets.

* Version 1.1.0: add the ability to run the C++ Agent as a Windows service and support for a configuration file instead of command line arguments. The agent can accept input from a socket in a pipe (|) delimited stream according to the descriptions given in the adapter guide.
