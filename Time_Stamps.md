---
title: Time Stamps
description: 
published: true
date: 2021-09-24T00:32:45.681Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:32:43.749Z
---

Time stamps in MTConnect are based on Universal Time Coordinated (UTC).
Also known as Greenwich Mean Time (GMT). Or ZULU time by the military.
This allows factories in different parts of the world to be monitored
and used in the same reports without error caused by Time Zone
difference.

Getting all your equipment clocks to be correct can sometimes be
difficult. If the controller is based on WINDOWS, you can use time
servers to synchronize them.

MTConnect Agents have the ability to ignore the time stamps from the
equipment (Adapters) and use the servers local clock to replace any
errors thus keeping all the equipment in synch. By putting all your
Agents on the same server, you can easily control your time stamp
synching and allow easier maintenance of settings. This is desirable in
large or mixed device type implementations. But it is strictly an
implementation preference.