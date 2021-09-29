---
title: Two Axis Lathe
description: 
published: true
date: 2021-09-25T01:48:01.653Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:32:46.529Z
---

This machine is a simple two axis horizontal lathe with a Z and an X
axis where the Linear Z axis is aligned with the primary spindle Rotary
C. The material is now held in the C axis and the tool is fixed.

![TwoAxisLathe.PNG](/images/TwoAxisLathe.PNG)

``` xml

<?xml version="1.0" encoding="UTF-8"?>
 <MTConnectDevices ns="urn:mtconnect.com:MTConnectDevices:1.1"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="urn:mtconnect.com:MTConnectDevices:1.1
     MTConnectDevices.xsd">
   <Header bufferSize="130000" instanceId="1"
      creationTime="2009-11-13T02:31:40" sender="local" version="1.1"/>
   <Devices>
     <Device id="d1" uuid="HM1" name="HMC_3Axis">
            <Description>3 Axis Mill</Description>
            <Components>
                <Axes id="a" name="base">
                    <Components>
                        <Linear id="x" name="X">
                            <DataItems>
                                <DataItem type="POSITION" subType="ACTUAL" id="xp" category="SAMPLE"
             name="Xact" units="MILLIMETER" nativeUnits="MILLIMETER"
             coordinateSystem="MACHINE"/>
                            </DataItems>
                        </Linear>
                        <Linear id="z" name="Z">
                            <DataItems>

           <DataItem type="POSITION" id="zp" category="SAMPLE" name="Zact"
             subType="ACTUAL" units="MILLIMETER" nativeUnits="MILLIMETER"
             coordinateSystem="MACHINE"/>
            </DataItems>
                        </Linear>
                        <Rotary id="c" name="C">
                            <DataItems>
                                <DataItem type=" ROTARY_VELOCITY " id="cspd" category="SAMPLE"
             name="Sspeed" subType="ACTUAL" units="REVOLUTION/MINUTE"
             nativeUnits="REVOLUTION/MINUTE"/>
                                <DataItem type=" ROTARY_VELOCITY " id="cso" category="SAMPLE"
             name="Sovr" subType="OVERRIDE" units="PERCENT"
             nativeUnits="PERCENT"/>
                                <DataItem type="ROTARY_MODE" id="rf" category="EVENT" name="rfunc">
                                    <Constraints>
                                        <Value>SPINDLE</Value>
                                        <Value>INDEX</Value>
                                    </Constraints>
                                </DataItem>
                            </DataItems>
                        </Rotary>
                    </Components>
                </Axes>
                <Controller id="cont" name="controller">
                    <Components>
                        <Path id="path" name="path">
                            <DataItems>
                                <DataItem type="PROGRAM" id="pgm" category="EVENT" name="program"/>
                                <DataItem type="BLOCK" id="blk" category="EVENT" name="block"/>
                                <DataItem type="LINE" id="ln" category="EVENT" name="line"/>
                                <DataItem type="PATH_FEEDRATE" id="pf" category="SAMPLE" name="Fact"
             units="MILLIMETER/SECOND" nativeUnits="FOOT/MINUTE"
             subType="ACTUAL"/>
                                <DataItem type="PATH_FEEDRATE" id="pfo" category="SAMPLE"
             name="Fovr" units="PERCENT" nativeUnits="PERCENT"
             subType="OVERRIDE"/>
                                <DataItem type="PATH_POSITION" id="pp" category="SAMPLE" name="Ppos"
             units="MILLIMETER_3D" nativeUnits="FOOT_3D"
             coordinateSystem="WORK"/>
                                <DataItem type="EXECUTION" id="exec" category="EVENT"
             name="execution"/>
                                <DataItem type="CONTROLLER_MODE" id="cm" category="EVENT"
             name="mode"/>
                            </DataItems>
                        </Path>
                    </Components>
                </Controller>
            </Components>
        </Device>
    </Devices>
</MTConnectDevices>
```