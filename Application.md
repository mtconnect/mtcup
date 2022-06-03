---
title: Application
description: 
published: true
date: 2022-06-03T12:24:58.134Z
tags: 
editor: markdown
dateCreated: 2022-06-03T12:24:58.134Z
---

# Application

## Overview

MTConnect standard does not specify or restrict the Application layer in the architecture keeping it completely open ended for the end user implementation.

## HTTP Request to the Agent

The MTConnect Agent receives data from one or more devices and makes it available as XML, then transforms that into HTML. 

There are four types of HTTP GET Requests that can be made to an agent (See [REST Protocol Model](https://model.mtconnect.org/#Package__8082e379-d82e-4b0e-abad-83cdf92f7fe6)):

- `probe`: returns the structure of the available devices and their data items.
  - Format: `<ip_address:port>/probe` or simply `<ip_address:port>` since `probe` is also the default request.
  - Example: `http://127.0.0.1:5000/probe` where the agent is running locally on port 5000.

- `current`: returns the latest values for each data item, along with a timestamp.
  - Format: `<ip_address:port>/current`.

- `sample`: returns a sequence of values and their timestamps.
  - Format: `<ip_address:port>/sample`.

- `asset`: returns a set of assets associated with the devices.
  - Format: `<ip_address:port>/asset` or `<ip_address:port>/assets`.

## Query using XPath

Available data can be specifically requested using a simple query language called [XPath](https://en.wikipedia.org/wiki/XPath).

> These will give an error if the given path cannot be found in the document.

### Current Examples


#### Get availability of all devices

```
http://172.26.81.154:5604/current?path=//DataItem[@type="AVAILABILITY"]
```

#### Get availability by a specific id

```
http://172.26.81.154:5604/current?path=//DataItems/DataItem[@id="avail"]
```

#### Get all condition statuses

```
http://172.26.81.154:5604/current?path=//DataItems/DataItem[@category="CONDITION"]
```

#### Get all Controller dataitems

```
http://172.26.81.154:5604/current?path=//Controller/*
```

#### Get all Linear axis dataitems

```
http://172.26.81.154:5604/current?path=//Axes/Components/Linear/DataItems
```

#### Get all Linear x1 axis dataitems

```
http://172.26.81.154:5604/current?path=//Axes/Components/Linear[@id="x1"]
```

#### Get VARIABLES (v1.5)

```
http://172.26.81.154:5709/current?path=//DataItem[@type="VARIABLE"]
```

#### Get the EXECUTION dataitem

```
http://172.26.81.154:5604/current?path=//Controller/Components/Path/DataItems/DataItem[@type="EXECUTION"]
```

#### Get the EXECUTION and CONTROLLER_MODE dataitems

```
http://172.26.81.154:5604/current?path=//Controller/Components/Path/DataItems/DataItem[@type="EXECUTION" or @type="CONTROLLER_MODE"]
```

#### Get Rotary Velocity for all Spindles

```
http://172.26.83.69:5000/current?path=//Axes/Components/Rotary/DataItems/DataItem[@type="ROTARY_VELOCITY" and @subType="ACTUAL"]
```


#### Get All DataItems In the Controller section AND all Health conditions, AND AVAILABILITY 

```
http://172.26.81.154:5709/current?path=//DataItems/DataItem[@category="CONDITION"]|//Controller/*|//DataItem[@type="AVAILABILITY"]
```

### Sample Examples

- `from` and `count`:  The MTConnect Agent stores a certain number of observations, called sequences. You can specify which ones and how many to view with the `from` and `count` fields.

count:
```
http://172.26.83.74:5000/sample?path=//DataItem[@type="PART_COUNT"]&count=4095
```

from and count: 
```
http://172.26.83.74:5000/sample?path=//DataItem[@type="PART_COUNT"]&from=13&count=4095
```

### Asset Examples

#### Get Asset by AssetID

```
http://localhost:5000/asset/EDP1234
```

#### Get Only Cutting Tool Assets

```
http://localhost:5000/assets?path=//Assets/DataItems/DataItem[@type="CuttingTool"]
```

#### Get the Asset Changed Event

```
http://localhost:5000/current?path=//Devices/Device/DataItems/DataItem[@type="ASSET_CHANGED"]
```


```
http://172.26.83.83:5000/current?path=//DataItems/DataItem[@type="ASSET_CHANGED" or @type="ASSET_REMOVED"]
```

### Data Streaming Example

```
http://172.26.83.83:5000/Mazak/sample?from=172&interval=2000&path=//DataItems/DataItem[@type="ASSET_CHANGED" or @type="ASSET_REMOVED"]
```

> WARNING! `instanceId`s in device files can change anytime agent reboots.  Check and track the `instanceId` in the header.

## Best Practices for Querying

HTTP data streaming is a method for an agent to provide a continuous stream of observations in response to a single request using a publish and subscribe communication pattern. 

When an HTTP Request includes an interval parameter, an agent MUST provide data with a minimum delay in milliseconds between the end of one data transmission and the beginning of the next. A value of zero (0) for the interval parameter indicates that the agent should deliver data at the highest rate possible and is only relevant for sample requests.

Example:

```
http://172.26.83.83:5000/Mazak/sample?from=172&interval=2000&path=//DataItems/DataItem[@type="ASSET_CHANGED" or @type="ASSET_REMOVED"]
```

To learn more about data streaming see [REST Protocol Model](https://model.mtconnect.org/#Package__8082e379-d82e-4b0e-abad-83cdf92f7fe6).



## Examples

- [Dataminer](https://github.com/mtconnect/dataminer)
- [MTExplorer](https://github.com/mtconnect/mtexplorer)
- [TrakHound MTConnect](https://github.com/TrakHound/MTConnect.NET)