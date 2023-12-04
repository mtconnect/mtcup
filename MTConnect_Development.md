---
title: What is Available?
description: Lists what is available for a developer to build their own MTConnect solution.
published: true
date: 2023-12-04T20:05:47.760Z
tags: 
editor: markdown
dateCreated: 2022-06-03T09:52:26.365Z
---

# Building your own MTConnect Solution?

## What is required?

The basic elements of an MTConnect architecture are:

- Device
- Adapter
- MTConnect Agent
- Application

See [White Paper on MTConnect Architecture](/Getting_Started_with_MTConnect_–_Architecture "wikilink") for more details on commonly seen implementations.


## What is available?

- MTConnect Agent.
  - Most MTConnect implementations use the open source C++ Agent available on MTConnect GitHub. See [C++ Agent](/C++_Agent "wikilink") to learn about the C++ Agent.
- Examples for Adapter. See
  - [Adapter Examples](https://github.com/mtconnect/adapter)
  - [PocketNC Adapter](https://github.com/mtconnect/PocketNC_adapter)
  - [ISO Digital Twin Project](https://github.com/mtconnect/iso_digital_twin_adapter)
  - [TrueAnalyticsSolutions/Mtconnect.Adapter](https://github.com/TrueAnalyticsSolutions/Mtconnect.Adapter): First model-aware Adapter SDK to use the [MTConnect Transpiler](/MTConnect-Transpiler "wikilinks").
  - See [Adapter](/MTConnect_Adapter "wikilink") for more details.
- Examples for Application. See
  - [Dataminer](https://github.com/mtconnect/dataminer)
  - [MTExplorer](https://github.com/mtconnect/mtexplorer)
  - [TrakHound MTConnect](https://github.com/TrakHound/MTConnect.NET)
  - [TrueAnalyticsSolutions/MtconnectCore](https://github.com/TrueAnalyticsSolutions/MtconnectCore): Deserializes MTConnect Response Documents into strongly-typed class model.
  - See [Application](/Application "wikilink") for more details.

## What needs to be developed?

- [Adapter.](/MTConnect_Adapter "wikilink")

- [Application.](/Application "wikilink")