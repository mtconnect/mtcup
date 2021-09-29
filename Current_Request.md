---
title: Current Request
description: 
published: true
date: 2021-09-24T00:30:55.184Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:30:53.017Z
---

Here is an example of the current request using the *at* parameter with
a very simple machine configuration:

``` xml numberLines
<?xml version="1.0" encoding="UTF-8"?>
<MTConnectDevices ns="urn:mtconnect.org:MTConnectDevices:1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:mtconnect.org:MTConnectDevices:1.1 http://www.mtconnect.org/schemas/MTConnectDevices_1.1.xsd">
  <Header creationTime="2010-04-01T21:22:43" sender="host" version="1.1" bufferSize="1" instanceId="1"/>
  <Devices>
    <Device name="minimal" uuid="1" id="d">
      <DataItems>
        <DataItem type="AVAILABILITY" category="EVENT" id="avail" />
      </DataItems>
      <Components>
        <Controller name="controller" id="c1">
          <DataItems>
            <DataItem id="estop" type="EMERGENCY_STOP" category="EVENT"/>
            <DataItem id="system" type="SYSTEM" category="CONDITION" />
          </DataItems>
          <Components>
            <Path id="p1" name="path" >
              <DataItems>
                <DataItem id="execution" type="EXECUTION" category="EVENT"/>
              </DataItems>
            </Path>
          </Components>
         </Controller>
       </Components>
     </Device>
  </Devices>
</MTConnectDevices>
```