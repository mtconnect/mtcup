---
title: Sensors
description: 
published: true
date: 2021-09-25T01:49:24.086Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:32:37.593Z
---

Sensors are modeled with the *[DataItem](/Terminology "wikilink")* types
associated directly with the *[Component](/Terminology "wikilink")* that
is being measured. In the example below, the spindle has measurement for
temperature (thermistor) and vibration (accelerometer). Additionally,
the sensor unit may have its own diagnostic measurements – in this case,
a temperature measurement (thermistor) to measure the health of the
sensor unit.

![SpindleSensingSystem.PNG](/images/SpindleSensingSystem.PNG)

The basic machine is modeled below – 3 linear axes and a spindle. The
spindle has two additional *[DataItems](/Terminology "wikilink")*
representing the sensors for temperature and acceleration.

``` xml

<?xml version="1.0" encoding="UTF-8"?>
<MTConnectDevices ns="urn:mtconnect.org:MTConnectDevices:1.2"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="urn:mtconnect.org:MTConnectDevices:
   1.2 ../MTConnectDevices_1.2.xsd">
  <Header bufferSize="130000" instanceId="1" creationTime="
     2009-11-13T02:31:40" sender="local" version="1.2"/>
  <Devices>
    <Device id="d1" uuid="HM1" name="HMC_3Axis">
      <Description>3 Axis Mill</Description>
      <DataItems>
          <DataItem type="AVAILABILITY" category="EVENT" id="avail" />
      </DataItems>
      <Components>
        <Axes id="a" name="base">
          <Components>
            <Linear id="y" name="Y">
              <DataItems>
                <DataItem type="POSITION" subType="ACTUAL" id="yp"
                   category="SAMPLE" name="Yact" units="MILLIMETER"
                   nativeUnits="MILLIMETER" coordinateSystem="MACHINE"/>
              </DataItems>
            </Linear>
            <Linear id="x" name="X">
              <DataItems>
                <DataItem type="POSITION" subType="ACTUAL" id="xp"
                   category="SAMPLE" name="Xact" units="MILLIMETER"
                   nativeUnits="MILLIMETER" coordinateSystem="MACHINE"/>
              </DataItems>
            </Linear>
            <Linear id="z" name="Z">
              <DataItems>
                <DataItem type="POSITION" id="zp" category="SAMPLE"
                   name="Zact" subType="ACTUAL" units="MILLIMETER"
                   nativeUnits="MILLIMETER" coordinateSystem="MACHINE"/>
              </DataItems>
            </Linear>
            <Rotary id="c" name="C">
              <DataItems>
                <DataItem type="ROTARY_VELOCITY" id="cspd" category="SAMPLE"
                   name="Sspeed" subType="ACTUAL" units="REVOLUTION/MINUTE"
                   nativeUnits="REVOLUTION/MINUTE"/>
                <DataItem type="ROTARY_VELOCITY" id="cso" category="SAMPLE"
                   name="Sovr" subType="OVERRIDE" units="PERCENT"
                   nativeUnits="PERCENT"/>
                <DataItem type="ROTARY_MODE" id="rf" category="EVENT"
                   name="rfunc">
                  <Constraints>
                    <Value>SPINDLE</Value>
                  </Constraints>
                </DataItem>
                <DataItem type="TEMPERATURE" category="SAMPLE" name="Ctemp"
                   id="ct" units="CELSIUS" statistic="AVERAGE">
                                 <Source componentId="s1">channel:1</Source>
                <DataItem type="ACCLERATION" category="SAMPLE" name="Sacc"
                   id="sa" units="MILLIMETERS/SECOND^2" statistic="MAXIMUM">
                                <Source componentId="s2">channel:2</Source>
                </DataItem>
              </DataItems>
            </Rotary>
          </Components>
        </Axes>
```

Additionally, the sensor unit is modeled with its configuration
information and a *[DataItem](/Terminology "wikilink")*of category
*[Sample](/Terminology "wikilink")* (*Voltage*) and a
*[DataItem](/Terminology "wikilink")* of type *Condition* (*Voltage*).

``` xml

    <Components>
    <Sensor id="sensor" name="sensor">
            <Configuration>
                <SensorConfiguration>
                    <FirmwareVersion>2.02</FirmwareVersion>
                    <CalibrationDate>2010-05-16</CalibrationDate>
                    <NextCalibrationDate>2010-05-16</NextCalibrationDate>
                    <CalibrationInitials>WS</CalibrationInitials>
                    <Channels>
                        <Channel number="1" name="A/D:1">
                            <Description>A/D With Thermister</Description>
                        <Channel number="2" name="A/D:2">
                            <Description>A/D With Accelerometer</Description>
                        </Channel>
                    </Channels>
                </SensorConfiguration>
            </Configuration>
            <DataItems>
                <DataItem category="CONDITION" id="senvc" type="VOLTAGE" />
                <DataItem category="SAMPLE" id="senv" type="VOLTAGE" units="VOLT"
         subType="DIRECT" />
            </DataItems>
    </Sensor>
            </Components>
        </Device>
    </Devices>
</MTConnectDevices>
```