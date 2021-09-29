---
title: Vertical Three Axis Mill
description: 
published: true
date: 2021-09-25T01:45:52.456Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:32:56.834Z
---

This is a simple machine tool with a vertical spindle and a table that
can move in two dimensions. The modeling always starts with the *Linear*
Z axis that is aligned with the primary spindle. The X axis is defined
as the longest axis perpendicular to the Z axis. The spindle is now
defined as a *Rotary* C axis that rotates around the Z axis.

![ThreeAxisMill.PNG](/images/ThreeAxisMill.PNG)

The [right hand rule](/Machine_Tool_Modeling "wikilink") applies when
naming the axes and defining positive motion and rotation. In this case
the *Rotary* axis only operates as a spindle, so it will have a constant
valued *[DataItem](/Terminology "wikilink")* called *RotaryMode*. This
machine is only capable of executing a single program and therefore only
capable of a single path. The following [XML](/Terminology "wikilink")
describes a simple configuration for this machine.

``` xml

<?xml version="1.0" encoding="UTF-8"?>
 <MTConnectDevices ns="urn:mtconnect.com:MTConnectDevices:1.1"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="urn:mtconnect.com:MTConnectDevices:1.1
    MTConnectDevices.xsd">
    <Header bufferSize="130000" instanceId="1" creationTime="2009-11-
      13T02:31:40" sender="local" version="1.1"/>
    <Devices>
        <Device id="d1" uuid="HM1" name="HMC_3Axis">
            <Description>3 Axis Mill</Description>
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
                                <DataItem type="POSITION" id="zp" category="SAMPLE" name="Zact"
             subType="ACTUAL" units="MILLIMETER" nativeUnits="MILLIMETER"
             coordinateSystem="MACHINE"/>
                            </DataItems>
                        </Linear>
                        <Rotary id="c" name="C">
                            <DataItems>
                                <DataItem type="ROTARY_VELOCITY" id="cspd" category="SAMPLE"
             name="Sspeed" subType="ACTUAL" units="REVOLUTION/MINUTE"
             nativeUnits="REVOLUTION/MINUTE"/>
                                <DataItem type=" ROTARY_VELOCITY " id="cso" category="SAMPLE"
             name="Sovr" subType="OVERRIDE" units="PERCENT"
             nativeUnits="PERCENT"/>
                                <DataItem type="ROTARY_MODE" id="rf" category="EVENT"
             name="rfunc">
                                    <Constraints>
                                        <Value>SPINDLE</Value>
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
                                <DataItem type="PROGRAM" id="pgm" category="EVENT"
             name="program"/>
                                <DataItem type="BLOCK" id="blk" category="EVENT" name="block"/>
                                <DataItem type="LINE" id="ln" category="EVENT" name="line"/>
                                <DataItem type="PATH_FEEDRATE" id="pf" category="SAMPLE"
             name="Fact" units="MILLIMETER/SECOND"
             nativeUnits="FOOT/MINUTE" subType="ACTUAL"/>
                                <DataItem type="PATH_FEEDRATE" id="pfo" category="SAMPLE"
             name="Fovr" units="PERCENT" nativeUnits="PERCENT"
             subType="OVERRIDE"/>
                                <DataItem type="PATH_POSITION" id="pp" category="SAMPLE"
             name="Ppos" units="MILLIMETER_3D" nativeUnits="FOOT_3D"
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