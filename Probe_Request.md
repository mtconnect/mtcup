---
title: Probe Request
description: 
published: true
date: 2021-09-24T00:32:17.265Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:32:15.047Z
---

The following is an example probe request for 4 Axis Simulator:

``` xml numberLines
<?xml version="1.0" encoding="UTF-8"?>
<MTConnectDevices xmlns:m="urn:mtconnect.org:MTConnectDevices:1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ns="urn:mtconnect.org:MTConnectDevices:1.1" xsi:schemaLocation="urn:mtconnect.org:MTConnectDevices:1.1 http://www.mtconnect.org/schemas/MTConnectDevices_1.1.xsd">
 <Header creationTime="2010-03-13T08:02:38+00:00" sender="localhost" instanceId="1268463594" bufferSize="131072" version="1.1" />
 <Devices>
  <Device id="dev" name="VMC-4Axis" uuid="XXX111">
   <DataItems>
    <DataItem category="EVENT" id="avail" type="AVAILABILITY" />
   </DataItems>
   <Components>
    <Axes id="axes" name="axes">
     <Components>
      <Linear id="x" name="X">
       <DataItems>
        <DataItem category="SAMPLE" id="Xact" nativeUnits="MILLIMETER" subType="ACTUAL" type="POSITION" units="MILLIMETER" />
        <DataItem category="SAMPLE" id="Xload" nativeUnits="PERCENT" type="LOAD" units="PERCENT" />
        <DataItem category="CONDITION" id="Xtravel" type="POSITION" />
        <DataItem category="CONDITION" id="Xovertemp" type="TEMPERATURE" />
        <DataItem category="CONDITION" id="Xservo" type="ACTUATOR" />
       </DataItems>
      </Linear>
      <Linear id="y" name="Y">
       <DataItems>
        <DataItem category="SAMPLE" id="Yact" nativeUnits="MILLIMETER" subType="ACTUAL" type="POSITION" units="MILLIMETER" />
        <DataItem category="SAMPLE" id="Yload" nativeUnits="PERCENT" type="LOAD" units="PERCENT" />
        <DataItem category="CONDITION" id="Ytravel" type="POSITION" />
        <DataItem category="CONDITION" id="Yovertemp" type="TEMPERATURE" />
        <DataItem category="CONDITION" id="Yservo" type="ACTUATOR" />
       </DataItems>
      </Linear>
      <Linear id="z" name="Z">
       <DataItems>
        <DataItem category="SAMPLE" id="Zact" nativeUnits="MILLIMETER" subType="ACTUAL" type="POSITION" units="MILLIMETER" />
        <DataItem category="SAMPLE" id="Zload" nativeUnits="PERCENT" type="LOAD" units="PERCENT" />
        <DataItem category="CONDITION" id="Ztravel" type="POSITION" />
        <DataItem category="CONDITION" id="Zovertemp" type="TEMPERATURE" />
        <DataItem category="CONDITION" id="Zservo" type="ACTUATOR" />
       </DataItems>
      </Linear>
      <Rotary id="a" name="A">
       <DataItems>
        <DataItem category="SAMPLE" id="Aact" nativeUnits="DEGREE" subType="ACTUAL" type="ANGLE" units="DEGREE" />
        <DataItem category="SAMPLE" id="Aload" nativeUnits="PERCENT" type="LOAD" units="PERCENT" />
        <DataItem category="CONDITION" id="Atravel" type="POSITION" />
        <DataItem category="CONDITION" id="Aovertemp" type="TEMPERATURE" />
        <DataItem category="CONDITION" id="Aservo" type="ACTUATOR" />
       </DataItems>
      </Rotary>
      <Rotary id="c" name="C" nativeName="S1">
       <DataItems>
        <DataItem category="SAMPLE" id="S1speed" nativeUnits="REVOLUTION/MINUTE" type="SPINDLE_SPEED" units="REVOLUTION/MINUTE" />
        <DataItem category="EVENT" id="S1mode" type="ROTARY_MODE">
         <Constraints>
          <Value>SPINDLE</Value>
         </Constraints>
        </DataItem>
        <DataItem category="SAMPLE" id="S1load" nativeUnits="PERCENT" type="LOAD" units="PERCENT" />
        <DataItem category="CONDITION" id="spindle" type="SYSTEM" />
       </DataItems>
      </Rotary>
     </Components>
    </Axes>
    <Controller id="cont" name="controller">
     <DataItems>
      <DataItem category="CONDITION" id="logic" type="LOGIC_PROGRAM" />
      <DataItem category="EVENT" id="estop" type="EMERGENCY_STOP" />
      <DataItem category="CONDITION" id="servo" type="ACTUATOR" />
      <DataItem category="EVENT" id="message" type="MESSAGE" />
      <DataItem category="CONDITION" id="comms" type="COMMUNICATIONS" />
     </DataItems>
     <Components>
      <Path id="path" name="path">
       <DataItems>
        <DataItem category="SAMPLE" id="SspeedOvr" nativeUnits="PERCENT" subType="OVERRIDE" type="SPINDLE_SPEED" units="PERCENT" />
        <DataItem category="EVENT" id="block" type="BLOCK" />
        <DataItem category="EVENT" id="execution" type="EXECUTION" />
        <DataItem category="EVENT" id="program" type="PROGRAM" />
        <DataItem category="SAMPLE" id="path_feedrate" nativeUnits="MILLIMETER/SECOND" type="PATH_FEEDRATE" units="MILLIMETER/SECOND" />
        <DataItem category="EVENT" id="mode" type="CONTROLLER_MODE" />
        <DataItem category="EVENT" id="line" type="LINE" />
        <DataItem category="SAMPLE" id="path_pos" nativeUnits="MILLIMETER_3D" subType="ACTUAL" type="PATH_POSITION" units="MILLIMETER_3D" />
        <DataItem category="SAMPLE" id="probe" nativeUnits="MILLIMETER_3D" subType="PROBE" type="PATH_POSITION" units="MILLIMETER_3D" />
        <DataItem category="EVENT" id="part" type="PART_ID" />
        <DataItem category="CONDITION" id="motion" type="MOTION_PROGRAM" />
        <DataItem category="CONDITION" id="system" type="SYSTEM" />
       </DataItems>
      </Path>
     </Components>
    </Controller>
   </Components>
  </Device>
 </Devices>
</MTConnectDevices>
```