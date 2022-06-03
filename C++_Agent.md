---
title: C++ Agent
description: 
published: true
date: 2022-06-03T01:12:06.061Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:30:40.556Z
---

## Reference MTConnect Agent

The MTConnect implementation that has been most closely tracking the MTConnect standard and is used in most production implementation is the C++ Agent that resides on the MTConnect project pages of github [C++ Agent](http://github.com/mtconnect/cppagent). 

As part of the effort, we also provide pre-built Windows x86 binaries that are statically linked and can be run on any Windows computer dating from around Windows XP SP2 and later. Earlier builds are possible, but these will be left up to the reader. The pre-built binaries can be found at: [C++ Agent
Releases](http://github.com/mtconnect/cppagent/releases).

The C++ Agent provides the fullest implementation of the standard available at the time of release. The version number (Major, Minor, Patch, Build) are tied to the MTConnect standard. The first three (Major, Minor, Build) are based directly on the MTConnect standard release nomenclature and the build number will increment as new versions of the C++ agent is released. When the agent is in pre-release for a upcoming version of the standard, it will be suffixed with a RCn where this stands for Release Candidate n as in RC1.

The agent has a rich level of configuration and is capable of serving up content from the local file system and allowing changes to be pushed. To get the full details on the configuration, again, visit the [C++ Agent github page](http://github.com/mtconnect/cppagent) and scroll down to the readme file. There are many features built in to the agent, some documented better than others. We will try to document some of the more interesting capabilities on these pages.

### Responsibilities

The MTConnect agent receives data from an adapter and transforms it as required by the MTConnect standard. The adapter communicates with the device's propriatary interface and delivers accoring to the MTConnect standard. There are certain responsibilities the Agent has and certain responsibilities the Adapter has in this architecture. 

#### The adapter is responsible for the following functionality:

1.  Connect to the controller's proprietary interface.
2.  Make the requisite calls to the controllers interfaces to retrieve the necessary data.
3.  Convert the controller state to events in the proper vocabulary. For example convert from Register 0x32 bit 6-9 to READY, ACTIVE, STOPPED.
4.  Format the data in the pipe delimited format described below.
5.  Respond to PING requests from the Agent in a timely manor to provide keep-alive heartbeats.
6.  Provide a complete snapshot of the data when an agent connects.

#### The agent is responsible for the following:

1.  Receive RESTful requests from application and respond with proper MTConnect XML.
2.  Provide push based streaming data when requested.
3.  Connect to the adapters and maintain the connections using heartbeats. Detect failures and cleanup connections.
4.  Receive the data from the adapter and parse the pipe delimited values.
5.  Minimally validate the data.
6.  Convert numeric values from the native units to the MTConnect units if required.
7.  Maintain the circular storage of events and assets.
8.  Handle many-to-many mappings from adapters to devices.
9.  Manage time series data in efficient manor.
10. Handle near-real-time updates to applications that have requested interval=0 notifications.
11. Efficiently format XML for applications.
12. Serve static XSD and other static assets as configured.
13. Handle timestamps from adapters or overwrite the timestamps using relative time adjustment or current clock time.

The agent is by far the more complex piece of software and takes a lot of work to get right. The adapters can be written in as little as 5 lines of Python or Ruby script and be perfectly adequate. The architecture was designed to operate in this way to make reduce the complexity of the adapter tools. Visit the C++ adapter SDK and the C\# adapter SDK to get a feel for what it takes to write an adapter. There are also videos on how to develop adapters here [Other_Resources](/Other_Resources "wikilink").

## Quick Start



## Usage and Configuration

[Agent Usage and Configuration](/Agent-Usage-and-Configuration "wikilink")

## Change History

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

