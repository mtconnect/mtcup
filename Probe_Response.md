---
title: Probe Response
permalink: /Probe_Response/
---

The following is an example probe request for 4 Axis Simulator:

``` xml

1.  <?xml version="1.0" encoding="UTF-8"?>
2.  <MTConnectDevices xmlns:m="urn:mtconnect.org:MTConnectDevices:1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
ns="urn:mtconnect.org:MTConnectDevices:1.1" xsi:schemaLocation="urn:mtconnect.org:MTConnectDevices:1.1
http://www.mtconnect.org/schemas/MTConnectDevices_1.1.xsd">
3.   <Header creationTime="2010-03-13T08:02:38+00:00" sender="localhost" instanceId="1268463594" bufferSize="131072" version="1.1" />
4.   <Devices>
5.    <Device id="dev" name="VMC-4Axis" uuid="XXX111">
6.     <DataItems>
7.      <DataItem category="EVENT" id="avail" type="AVAILABILITY" />
8.     </DataItems>
9.     <Components>
10.     <Axes id="axes" name="axes">
11.      <Components>
12.       <Linear id="x" name="X">
13.        <DataItems>
14.         <DataItem category="SAMPLE" id="Xact" nativeUnits="MILLIMETER" subType="ACTUAL" type="POSITION" units="MILLIMETER" />
15.         <DataItem category="SAMPLE" id="Xload" nativeUnits="PERCENT" type="LOAD" units="PERCENT" />
16.         <DataItem category="CONDITION" id="Xtravel" type="POSITION" />
17.         <DataItem category="CONDITION" id="Xovertemp" type="TEMPERATURE" />
18.         <DataItem category="CONDITION" id="Xservo" type="ACTUATOR" />
19.        </DataItems>
20.       </Linear>
21.       <Linear id="y" name="Y">
22.        <DataItems>
23.         <DataItem category="SAMPLE" id="Yact" nativeUnits="MILLIMETER" subType="ACTUAL" type="POSITION" units="MILLIMETER" />
24.         <DataItem category="SAMPLE" id="Yload" nativeUnits="PERCENT" type="LOAD" units="PERCENT" />
25.         <DataItem category="CONDITION" id="Ytravel" type="POSITION" />
26.         <DataItem category="CONDITION" id="Yovertemp" type="TEMPERATURE" />
27.         <DataItem category="CONDITION" id="Yservo" type="ACTUATOR" />
28.        </DataItems>
29.       </Linear>
30.       <Linear id="z" name="Z">
31.        <DataItems>
32.         <DataItem category="SAMPLE" id="Zact" nativeUnits="MILLIMETER" subType="ACTUAL" type="POSITION" units="MILLIMETER" />
33.         <DataItem category="SAMPLE" id="Zload" nativeUnits="PERCENT" type="LOAD" units="PERCENT" />
34.         <DataItem category="CONDITION" id="Ztravel" type="POSITION" />
35.         <DataItem category="CONDITION" id="Zovertemp" type="TEMPERATURE" />
36.         <DataItem category="CONDITION" id="Zservo" type="ACTUATOR" />
37.        </DataItems>
38.       </Linear>
39.       <Rotary id="a" name="A">
40.        <DataItems>
41.         <DataItem category="SAMPLE" id="Aact" nativeUnits="DEGREE" subType="ACTUAL" type="ANGLE" units="DEGREE" />
42.         <DataItem category="SAMPLE" id="Aload" nativeUnits="PERCENT" type="LOAD" units="PERCENT" />
43.         <DataItem category="CONDITION" id="Atravel" type="POSITION" />
44.         <DataItem category="CONDITION" id="Aovertemp" type="TEMPERATURE" />
45.         <DataItem category="CONDITION" id="Aservo" type="ACTUATOR" />
46.        </DataItems>
47.       </Rotary>
48.       <Rotary id="c" name="C" nativeName="S1">
49.        <DataItems>
50.         <DataItem category="SAMPLE" id="S1speed" nativeUnits="REVOLUTION/MINUTE" type="SPINDLE_SPEED" units="REVOLUTION/MINUTE" />
51.         <DataItem category="EVENT" id="S1mode" type="ROTARY_MODE">
52.          <Constraints>
53.           <Value>SPINDLE</Value>
54.          </Constraints>
55.         </DataItem>
56.         <DataItem category="SAMPLE" id="S1load" nativeUnits="PERCENT" type="LOAD" units="PERCENT" />
57.         <DataItem category="CONDITION" id="spindle" type="SYSTEM" />
58.        </DataItems>
59.       </Rotary>
60.      </Components>
61.     </Axes>
62.     <Controller id="cont" name="controller">
63.      <DataItems>
64.       <DataItem category="CONDITION" id="logic" type="LOGIC_PROGRAM" />
65.       <DataItem category="EVENT" id="estop" type="EMERGENCY_STOP" />
66.       <DataItem category="CONDITION" id="servo" type="ACTUATOR" />
67.       <DataItem category="EVENT" id="message" type="MESSAGE" />
68.       <DataItem category="CONDITION" id="comms" type="COMMUNICATIONS" />
69.      </DataItems>
70.      <Components>
71.       <Path id="path" name="path">
72.        <DataItems>
73.         <DataItem category="SAMPLE" id="SspeedOvr" nativeUnits="PERCENT" subType="OVERRIDE" type="SPINDLE_SPEED" units="PERCENT" />
74.         <DataItem category="EVENT" id="block" type="BLOCK" />
75.         <DataItem category="EVENT" id="execution" type="EXECUTION" />
76.         <DataItem category="EVENT" id="program" type="PROGRAM" />
77.         <DataItem category="SAMPLE" id="path_feedrate" nativeUnits="MILLIMETER/SECOND" type="PATH_FEEDRATE" units="MILLIMETER/SECOND" />
78.         <DataItem category="EVENT" id="mode" type="CONTROLLER_MODE" />
79.         <DataItem category="EVENT" id="line" type="LINE" />
80.         <DataItem category="SAMPLE" id="path_pos" nativeUnits="MILLIMETER_3D" subType="ACTUAL" type="PATH_POSITION" units="MILLIMETER_3D" />
81.         <DataItem category="SAMPLE" id="probe" nativeUnits="MILLIMETER_3D" subType="PROBE" type="PATH_POSITION" units="MILLIMETER_3D" />
82.         <DataItem category="EVENT" id="part" type="PART_ID" />
83.         <DataItem category="CONDITION" id="motion" type="MOTION_PROGRAM" />
84.         <DataItem category="CONDITION" id="system" type="SYSTEM" />
85.        </DataItems>
86.       </Path>
87.      </Components>
88.     </Controller>
89.    </Components>
90.   </Device>
91.  </Devices>
92. </MTConnectDevices>
```