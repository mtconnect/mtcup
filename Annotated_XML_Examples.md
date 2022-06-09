---
title: Annotated XML Examples
description: 
published: true
date: 2022-06-09T11:43:32.405Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:30:28.692Z
---


## More Complex Device File Example

The *[Sample](/Terminology "wikilink")* was generated with the following
request:

http://10.1.23.5/LinuxCNC/probe

The following is an example of a 3 axis mill simulation. The mill has
three linear axes and one rotary axis (spindle):

``` xml

1.  <?xml version="1.0" encoding="UTF-8"?>
2.  <MTConnectDevices xmlns:m="urn:mtconnect.com:MTConnectDevices:0.9" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ns="urn:mtconnect.com:MTConnectDevices:0.9" xsi:schemaLocation="urn:mtconnect.com:MTConnectDevices:0.9 /schemas/MTConnectDevices.xsd">
3.    <Header sender="10.1.23.5" bufferSize="100000" creationTime=
           "2008-07-07T23:07:50-07:00" version="0.9"
           instanceId="1214527986"/>
4.     <Devices>
5.      <Device iso841Class="6" uuid="linux-01" name="LinuxCNC"
             sampleRate="100.0" id="d1">
```

Here we provide the top level container
*[Devices](/Terminology "wikilink")* and the information on the
*[Device](/Terminology "wikilink")*.

``` xml

6.     <Description manufacturer="NIST" serialNumber="01"/>
7.       <DataItems>
8.             <DataItem type="AVAILABILITY" name="avail" category="EVENT"
               id="a"/>
9.         </DataItems>
10.     <Components>
11.     <Axes name="Axes" id="3">
```

On line 11 we introduce the collection of *Axes*. The *Axes*
[component](/Terminology "wikilink") is a special
[component](/Terminology "wikilink") that acts as an abstract
[component](/Terminology "wikilink") as well as a collection. The *Axes*
[component](/Terminology "wikilink") contains various
*[DataItems](/Terminology "wikilink")* that have a global context; they
are not associated with any one axis but they go across all axes.

``` xml

  <Components>
13.       <Rotary name="C" id="c1">
14.        <DataItems>
15.         <DataItem type="ROTARY_VELOCITY" name="Cspeed" category="SAMPLE"
                  id="c2" nativeUnits="REVOLUTION/MINUTE" subType="ACTUAL"
                  units="REVOLUTION/MINUTE">
16.            <Source>Sspeed</Source>
17.         </DataItem>
18.           <DataItem type=”ROTARY_MODE” name=”Cmode” category=”EVENT”
                    id=”c3”>
19.              <Constraints>
20.                 <Value>SPINDLE</Value>
21.              </Constraints>
22.           </DataItem>
23.         </DataItems>
24.      </Rotary>
```

The spindle [component](/Terminology "wikilink") (Rotary Axis C)
declared on line 13 is the C axis and has spindle specific
*[DataItems](/Terminology "wikilink")*.

``` xml

25.      <Linear name="X" id="x1">
26.        <DataItems>
27.          <DataItem type="POSITION" name="Xact" category="SAMPLE" id="x2"
               nativeUnits="MILLIMETER" subType="ACTUAL" units="MILLIMETER"/>
28.          <DataItem type="POSITION" name="Xcom" category="SAMPLE" id="x3"
               nativeUnits="MILLIMETER" subType="COMMANDED"
               units="MILLIMETER"/>
29.        </DataItems>
30.      </Linear>
31.      <Linear name="Y" id="y1">
32.        <DataItems>
33.          <DataItem type="POSITION" name="Yact" category="SAMPLE" id="y2"
                nativeUnits="MILLIMETER" subType="ACTUAL"
                units="MILLIMETER"/>
34.          <DataItem type="POSITION" name="Ycom" category="SAMPLE" id="y3"
                nativeUnits="MILLIMETER" subType="COMMANDED"
                units="MILLIMETER"/>
35.        </DataItems>
36.      </Linear>
37.      <Linear name="Z" id="z1">
38.       <DataItems>
39.         <DataItem type="POSITION" name="Zact" category="SAMPLE" id="z2"
               nativeUnits="MILLIMETER" subType="ACTUAL" units="MILLIMETER"/>
40.

           <DataItem type="POSITION" name="Zcom"
             category="SAMPLE" id="z3"
               nativeUnits="MILLIMETER" subType="COMMANDED"
               units="MILLIMETER"/>
41.       </DataItems>
42.      </Linear>
43.   </Components>
44.   </Axes>
```

Lines 25, 31, and 37 define the three linear axes X, Y, and Z
respectively. In this example [device](/Terminology "wikilink"), the
*[Agent](/Terminology "wikilink")* is only collecting the actual and
commanded positions.

The *Controller* is capable of providing the program name, block, and
the current line being executed:

``` xml

45.     <Controller name="Controller" id="cn8">
46.      <Components>
47.       <Path id=”pth1” name=”path”>
48.         <DataItems>
49.           <DataItem type="LINE" name="line" category="EVENT" id="p1"/>
50.           <DataItem type="CONTROLLER_MODE" name="mode" category="EVENT"
                  id="p2"/>
51.           <DataItem type="PROGRAM" name="program" category="EVENT"
                  id="p3"/>
52.           <DataItem type="EXECUTION" name="execution" category="EVENT"
                  id="p4"/>
53.           <DataItem type="PATH_FEEDRATE" name="feedrate"
54.               category="SAMPLE" id="p5" units=”MILLIMETER/SECOND”
                  nativeUnits=”MILLIMETER/SECOND” />
55.           <DataItem type="PATH_POSITION" name="position"
56.               category="SAMPLE" id="p6" units=”MILLIMETER_3D”
57.               nativeUnits=”INCH_3D”/>
58.         </DataItems>
59.       </Path>
60.      </Components>
61.     </Controller>
62.    </Components>
      </Device>
    </Devices>
63. </MTConnectDevices>
```