---
title: HyperQuadrex
description: 
published: true
date: 2021-09-25T01:48:37.443Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:31:41.061Z
---

![Hyperquadrex.PNG](/images/Hyperquadrex.PNG)

``` xml numberLines

<?xml version="1.0" encoding="UTF-8"?>
<MTConnectDevices ns="urn:mtconnect.com:MTConnectDevices:1.1"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="urn:mtconnect.com:MTConnectDevices:
   1.1 ../MTConnectDevices.xsd">
    <Header bufferSize="130000" instanceId="1" creationTime="
     2009-11-13T02:31:40" sender="local" version="1.1"/>
    <Devices>
        <Device id="d1" uuid="HM1" name="HyperQuadrex">
            <Description>Mazak - HyperQuadrex</Description>
            <Components>
                <Axes id="a" name="base">
                    <Components>
                        <Linear id="x" name="X" nativeName="X1">
                            <DataItems>
                                <DataItem type="POSITION" subType="ACTUAL" id="xp" category="SAMPLE"
             name="Xact" units="MILLIMETER" nativeUnits="MILLIMETER"
             coordinateSystem="MACHINE">
                                    <Source>X1pos</Source>
                                </DataItem>
                                <DataItem type="LOAD" id="xl" category="SAMPLE" name="Xload"
             units="PERCENT">
                                    <Source>X1load</Source>
                                </DataItem>
                            </DataItems>
                        </Linear>
                        <Linear id="y" name="Y" nativeName="Y1">
                            <DataItems>
                                <DataItem type="POSITION" subType="ACTUAL" id="yp" category="SAMPLE"
             name="Yact" units="MILLIMETER" nativeUnits="MILLIMETER"
             coordinateSystem="MACHINE">
                                    <Source>Y1pos</Source>
                                </DataItem>
                                <DataItem type="LOAD" id="yl" category="SAMPLE" name="Yload"
             units="PERCENT">
                                    <Source>Y1load</Source>
                                </DataItem>
                            </DataItems>
                        </Linear>
                        <Linear id="z" name="Z" nativeName="Z1">
                            <DataItems>
                                <DataItem type="POSITION" id="zp" category="SAMPLE" name="Zact"
             subType="ACTUAL" units="MILLIMETER" nativeUnits="MILLIMETER"
             coordinateSystem="MACHINE">
                                    <Source>Z1pos</Source>
                                </DataItem>
                                <DataItem type="LOAD" id="zl" category="SAMPLE" name="Zload"
             units="PERCENT">
                                    <Source>Z1load</Source>
                                </DataItem>
                            </DataItems>
                        </Linear>
                        <Linear id="x2" name="X2" >
                            <DataItems>
                                <DataItem type="POSITION" subType="ACTUAL" id="x2p"
             category="SAMPLE" name="X2act" units="MILLIMETER"
             nativeUnits="MILLIMETER" coordinateSystem="MACHINE"/>
                                <DataItem type="LOAD" id="x2l" category="SAMPLE" name="X2load"
             units="PERCENT">
                                    <Source>X2load</Source>
                                </DataItem>
                            </DataItems>
                        </Linear>
                        <Linear id="y2" name="Y2">
                            <DataItems>
                                <DataItem type="POSITION" subType="ACTUAL" id="y2p"
             category="SAMPLE" name="Y2act" units="MILLIMETER"
             nativeUnits="MILLIMETER" coordinateSystem="MACHINE"/>
                                <DataItem type="LOAD" id="y2l" category="SAMPLE" name="Y2load"
             units="PERCENT"/>
                            </DataItems>
                        </Linear>
                        <Linear id="z2" name="Z2">
                            <DataItems>

 
         <DataItem type="POSITION" id="z2p" category="SAMPLE" name="Z2act"
             subType="ACTUAL" units="MILLIMETER" nativeUnits="MILLIMETER"
             coordinateSystem="MACHINE">
                                    <Source>Z2pos</Source>
                                </DataItem>
                                <DataItem type="LOAD" id="z2l" category="SAMPLE" name="Z2load"
             units="PERCENT"/>
                            </DataItems>
                        </Linear>
                        <Linear id="z3" name="Z3" nativeName="W">
                            <DataItems>
                                <DataItem type="POSITION" id="z3p" category="SAMPLE" name="Z3act"
             subType="ACTUAL" units="MILLIMETER" nativeUnits="MILLIMETER"
             coordinateSystem="MACHINE">
                                    <Source>Wpos</Source>
                                </DataItem>
                                <DataItem type="LOAD" id="z3l" category="SAMPLE" name="Z3load"
             units="PERCENT">
                                    <Source>Wload</Source>
                                </DataItem>
                            </DataItems>
                        </Linear>
                        <Rotary id="c" name="C " nativeName="C1">
                            <DataItems>
                                <DataItem type="LOAD" id="Cl" category="SAMPLE" name="Cload"
             units="PERCENT"/>
                                <DataItem type=" ROTARY_VELOCITY " id="cspd" category="SAMPLE"
             name="Sspeed" subType="ACTUAL" units="REVOLUTION/MINUTE"
             nativeUnits="REVOLUTION/MINUTE"/>
                                <DataItem type=" ROTARY_VELOCITY " id="cso" category="SAMPLE"
             name="Sovr" subType="OVERRIDE" units="PERCENT"
             nativeUnits="PERCENT"/>
                                <DataItem type="DIRECTION" id="cdir" category="EVENT" name="Sdir"/>
                                <DataItem type="ANGLE" id="cpos" category="SAMPLE" name="Cpos"
             subType="ACTUAL" units="DEGREE" nativeUnits="DEGREE"
             nativeScale="-1.0"/>
                                <DataItem type="ROTARY_MODE" id="rf" category="EVENT" name="rfunc">
                                    <Constraints>
                                        <Value>SPINDLE</Value>
                                        <Value>INDEX</Value>
                                    </Constraints>
                                </DataItem>
                            </DataItems>
                        </Rotary>
                        <Rotary id="c2" name="C2">
                            <DataItems>
                                <DataItem type="LOAD" id="C2l" category="SAMPLE" name="C2load"
             units="PERCENT"/>
                                <DataItem type=" ROTARY_VELOCITY " id="c2spd" category="SAMPLE"
             name="Sspeed" subType="ACTUAL" units="REVOLUTION/MINUTE"
             nativeUnits="REVOLUTION/MINUTE"/>
                                <DataItem type=" ROTARY_VELOCITY " id="c2so" category="SAMPLE"
             name="Sovr" subType="OVERRIDE" units="PERCENT"
             nativeUnits="PERCENT"/>
                                <DataItem type="DIRECTION" id="c2dir" category="EVENT"
             name="S2dir"/>

         <DataItem type="ROTARY_MODE" id="rf2" category="EVENT" name="rfunc">
                                    <Constraints>
                                        <Value>SPINDLE</Value>
                                    </Constraints>
                                </DataItem>
                            </DataItems>
                        </Rotary>
                        <Rotary id="b" name="B" nativeName="S1">
                            <DataItems>
                                <DataItem type="LOAD" id="bl" category="SAMPLE" name="Bload"
             units="PERCENT"/>
                                <DataItem type=" ROTARY_VELOCITY " id="bspd" category="SAMPLE"
             name="Sspeed" subType="ACTUAL" units="REVOLUTION/MINUTE"
             nativeUnits="REVOLUTION/MINUTE"/>
                                <DataItem type=" ROTARY_VELOCITY " id="bso" category="SAMPLE"
             name="Sovr" subType="OVERRIDE" units="PERCENT"
             nativeUnits="PERCENT"/>
                                <DataItem type="DIRECTION" id="bdir" category="EVENT" name="S3dir"/>
                                <DataItem type="ROTARY_MODE" id="brf" category="EVENT" name="rfunc">
                                    <Constraints>
                                        <Value>SPINDLE</Value>
                                    </Constraints>
                                </DataItem>
                            </DataItems>
                        </Rotary>
                        <Rotary id="b2" name="B2" nativeName="S2">
                            <DataItems>
                                <DataItem type="LOAD" id="b2l" category="SAMPLE" name="B2load"
             units="PERCENT"/>
                                <DataItem type=" ROTARY_VELOCITY " id="b2spd" category="SAMPLE"
             name="Sspeed" subType="ACTUAL" units="REVOLUTION/MINUTE"
             nativeUnits="REVOLUTION/MINUTE"/>
                                <DataItem type=" ROTARY_VELOCITY " id="b2so" category="SAMPLE"
             name="Sovr" subType="OVERRIDE" units="PERCENT"
             nativeUnits="PERCENT"/>
                                <DataItem type="DIRECTION" id="b2dir" category="EVENT"
             name="S3dir"/>
                                <DataItem type="ROTARY_MODE" id="b2rf" category="EVENT"
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
                        <Path id="path1" name="path1">
                            <DataItems>
                                <DataItem type="ACTIVE_AXES" category="EVENT" name="axes"
             id="act_axes1"/>
                                <DataItem type="PROGRAM" id="pgm1" category="EVENT" name="program"/>
                                <DataItem type="BLOCK" id="blk1" category="EVENT" name="block"/>
                                <DataItem type="LINE" id="ln1" category="EVENT" name="line"/>

         <DataItem type="PATH_FEEDRATE" id="pf1" category="SAMPLE"
             name="Fact"  units="MILLIMETER/SECOND" nativeUnits="FOOT/MINUTE"
             subType="ACTUAL" coordinateSystem="WORK"/>
                                <DataItem type="PATH_FEEDRATE" id="pfo1" category="SAMPLE"
             name="Fovr" units="PERCENT" nativeUnits="PERCENT"
             subType="OVERRIDE"/>
                                <DataItem type="PATH_POSITION" id="pp1" category="SAMPLE"
             name="Ppos" units="MILLIMETER_3D" nativeUnits="MILLIMETER_3D"
             coordinateSystem="WORK"/>
                                <DataItem type="TOOL_ASSET_ID" id="tid1" category="EVENT"
             name="Tid"/>
                                <DataItem type="PART_ID" id="pid1" category="EVENT" name="Pid"/>
                                <DataItem type="EXECUTION" id="exec1" category="EVENT"
             name="execution"/>
                                <DataItem type="CONTROLLER_MODE" id="cm1" category="EVENT"
             name="mode"/>
                            </DataItems>
                        </Path>
                        <Path id="path2" name="path2">
                            <DataItems>
                                <DataItem type="ACTIVE_AXES" category="EVENT" name="axes"
             id="act_axes2"/>
                                <DataItem type="PROGRAM" id="pgm2" category="EVENT" name="program"/>
                                <DataItem type="BLOCK" id="blk2" category="EVENT" name="block"/>
                                <DataItem type="LINE" id="ln2" category="EVENT" name="line"/>
                                <DataItem type="PATH_FEEDRATE" id="pf2" category="SAMPLE"
             name="Fact" units="MILLIMETER/SECOND" nativeUnits="FOOT/MINUTE"
             subType="ACTUAL" coordinateSystem="WORK"/>
                                <DataItem type="PATH_FEEDRATE" id="pfo2" category="SAMPLE"
             name="Fovr" units="PERCENT" nativeUnits="PERCENT"
             subType="OVERRIDE"/>
                                <DataItem type="PATH_POSITION" id="pp2" category="SAMPLE"
             name="Ppos" units=" MILLIMETER_3D" nativeUnits=" MILLIMETER_3D"
             coordinateSystem="WORK"/>
                                <DataItem type="TOOL_ASSET_ID" id="tid2" category="EVENT"
             name="Tid"/>
                                <DataItem type="PART_ID" id="pid2" category="EVENT" name="Pid"/>
                                <DataItem type="EXECUTION" id="exec2" category="EVENT"
             name="execution"/>
                                <DataItem type="CONTROLLER_MODE" id="cm2" category="EVENT"
             name="mode"/>
                            </DataItems>
                        </Path>
                    </Components>
                </Controller>
                <Door id="d" name="door">
                    <DataItems>
                        <DataItem id="ds" category="EVENT" name="door" type="DOOR_STATE"/>
                    </DataItems>
                </Door>
            </Components>
        </Device>
    </Devices>
</MTConnectDevices>
```