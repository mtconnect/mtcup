---
title: Near Real Time
description: 
published: true
date: 2021-09-24T00:32:10.860Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:32:08.410Z
---

MTConnect supports the ability to receive changes in near real time. The
reference implementation of the MTConnect Agent written in C++ has this
capability and has been validated to propagate changes from the adapter
first reporting the even to the client application in between 3-10ms
depending on the platform. For a reasonably recent computer with a CPU
\> 500Mhz the agent can easily handle a 5ms latency.

To request data with as little latency as possible, use the interval
option and set the value to 0. This means to send data every 0 ms. The
MTConnect standard specifies that if no data is available, then the
agent will only need to send out a heartbeat every n seconds where n
defaults to 10 seconds or can be set with the heartbeat option on the
URL. So, the URL will look like this:

` `<http://example.com:5000/sample?interval=0&heartbeat=1000>

This will send data immediately when it arrives and send a heartbeat
every second to the client. A heartbeat will allow the client to detect
a disconnect or failure within 2x heartbeat ms; in this case you can
detect a failure within 2 seconds. In this case we are defining failure
as the agent fails and the client application can't detect the failure,
such as the network was disconnected or the machine was powered off.

This will also require that the application consumes [MTConnect
Streaming](/MTConnect_Streaming "wikilink") data.