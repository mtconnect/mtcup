---
title: How to model a Device
description: 
published: true
date: 2022-06-09T11:44:22.717Z
tags: 
editor: markdown
dateCreated: 2022-06-09T11:44:22.717Z
---

## What is an MTConnect Device File?

MTConnect Device File is an XML file (XML as of v2.0) that provides the structure and semantics for any given device(s) complying with the MTConnect Information Model.

### C++ Agent and Device File

One of the [configuration parameters](/Agent-Usage-and-Configuration#configuration-parameters "wikilink") for a C++ Agent is `Devices` where you specify the XML MTConnect Device File for your piece of equipment.

### What is a Device in MTConnect?

Any data source can be a device in the MTConnect. Common devices are CNC machines, 3d printers, robot arms, controls, sensors or sensor controllers, smart tools, etc.

## Write your own Device File

Throughout the examples below, the content will be referred to the [MTConnect SysML Model](https://model.mtconnect.org/). So that the reader can explore and understand all the related properties which may or may not be mentioned in the examples below.

### Examples

#### Simplest Device

For the simplest possible device, we are modeling a linux CNC that has only `Availability`, `AssetChanged` and `AssetRemoved` dataitems. These are the minimum required set of dataitems for any given Device in MTConnect. See the definition in the MTConnect model here: [Device](https://model.mtconnect.org/#Structure__362a1da2-ea18-4b23-a0de-536424a92c67)

It is important to note that all the `id`s in a Device file **MUST** be unique. As a best practice, all the `name`s are also kept unique. `id`s and `name`s of the same element might be the same as can be seen below (example: for `ASSET_CHANGED`)

```XML
<Devices>
  <Device uuid="linux-01" name="linuxCNC">
    <DataItems>
      <DataItem type="AVAILABILITY" id="a1" name="avail" category="EVENT"/>
  		<DataItem type="ASSET_CHANGED" id="asset_chg1" name="asset_chg1" category="EVENT"/>
      <DataItem type="ASSET_REMOVED" id="asset_rem1" name="asset_rem1" category="EVENT"/>
    </DataItems>
  </Device>
</Devices>
```

As you can see in the example above, all devices in a device file are modeled under `Devices` and similarly all dataitems are modeled under `DataItems`. Other similar examples include `Components`, `Compositions` and `References` to name a few. These are called organizers and their main purpose is to organize similar element types within. 

A `Device` is a `Component` type and inherits all the properties and adds/modifies a distinct few. 

See the model here to learn about all properties that a `Component` may have here: [Component](https://model.mtconnect.org/#Structure__6625a512-a9a0-4105-9f83-8073f2658b8f) and [Component Diagram](https://model.mtconnect.org/#Diagrams__eb46364d-b0c6-437d-a158-532a2e6b3157).

Learn about the properties of a `DataItem` here: [DataItem](https://model.mtconnect.org/#Structure__38c4ab63-342d-4944-b93a-191620822f27).


#### More Examples

[Annoted XML Examples](/Annotated_XML_Examples "wikilink").

### Using XML Schema

If you are already familiar with XML and XML schemas, please find the latest MTConnectDevices schema [here](https://github.com/mtconnect/schema) to write your Device File. As of 9th June 2022, [MTConnectDevices_2.0.xsd](https://raw.githubusercontent.com/mtconnect/schema/master/MTConnectDevices_2.0.0.xsd) are the latest schema for MTConnect Devices.

### Validating Using XML Schema

You can also validate your XML Device file online against the above mentioned or whatever is the latest MTConnectDevices schema. There are various free tools online where you can just upload your Device file and the schema for validation.


