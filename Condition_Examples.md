---
title: Condition Examples
description: 
published: true
date: 2021-09-24T00:30:52.141Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:30:49.990Z
---

The condition has additional restrictions which are different from the
*[Event](/Terminology "wikilink")* and
*[Sample](/Terminology "wikilink")*. The following will demonstrate the
differences and usage of the *Condition*.

``` xml

...
<Linear id="y" name="Y">
  <DataItems>
    <DataItem type="POSITION" subType="ACTUAL" id="yp" category="SAMPLE"
         name="Yact" units="MILLIMETER" nativeUnits="MILLIMETER"
         coordinateSystem="MACHINE"/>
    <DataItem type="POSITION" id="ylc" category="CONDITION"/>
    <DataItem type="LOAD" id="ylc" category="CONDITION"/>
    <DataItem type="TEMPERATURE" id="ytc" category="CONDITION"/>
  </DataItems>
</Linear>
...
<Controller id="cont" name="controller">
  <DataItems>
    <DataItem type="PROGRAM" id="pgm" category="EVENT" name="program"/>
    <DataItem type="BLOCK" id="blk" category="EVENT" name="block"/>
    <DataItem type="LINE" id="ln" category="EVENT" name="line"/>
    <DataItem type="PATH_FEEDRATE" id="pf" category="SAMPLE" name="Fact"
       units="MILLIMETER/SECOND" nativeUnits="FOOT/MINUTE" subType="ACTUAL"
       coordinateSystem="WORK"/>
    <DataItem type="PATH_FEEDRATE" id="pfo" category="SAMPLE" name="Fovr"
       units="PERCENT" nativeUnits="PERCENT" subType="OVERRIDE"/>
    <DataItem type="PATH_POSITION" id="pp" category="SAMPLE" name="Ppos"
       units="MILLIMETER" nativeUnits="MILLIMETER" coordinateSystem="WORK"/>
    <DataItem type="TOOL_ASSET_ID" id="tid" category="EVENT" name="Tid"/>
    <DataItem type="PART_ID" id="pid" category="EVENT" name="Pid"/>
    <DataItem type="EXECUTION" id="exec" category="EVENT" name="execution"/>
    <DataItem type="CONTROLLER_MODE" id="cm" category="EVENT" name="mode"/>

    <DataItem type="COMMUNICATIONS" id="cc1" category="CONDITION"/>
    <DataItem type="MOTION_PROGRAM" id="cc2" category="CONDITION"/>
    <DataItem type="LOGIC_PROGRAM" id="cc3" category="CONDITION"/>
  </DataItems>
</Controller >
```

In the previous example we have focused on two components, a Linear Y
axis and a controller. They both have *Condition* associated with them.
The axis has a temperature sensor and a load sensor that will alert when
the temperature or load goes out of range. The controller also has a few
*Condition* associated with the *Program* and *Communications*.

When everything is working properly, a
*[Current](/Terminology "wikilink")* request will deliver the following
[XML](/Terminology "wikilink"):

``` xml

<DeviceStream uuid="HM1" name="HMC_3Axis">
  <ComponentStream component="Linear" name="Y" componentId="y">
    <Samples>
      <Position dataItemId="yp" name="Yact" subType="ACTUAL" sequence="23"
         timestamp="2009-11-13T08:00:00">213.1232</Position>
    </Samples>
    <Condition>
      <Normal type="TEMPERATURE" dataItemId="ytmp" sequence="25"
         timestamp="..."/>
      <Normal type="LOAD" dataItemId ="ylc" sequence="26" timestamp="..."/>
      <Normal type="POSITION" dataItemId ="ypc" sequence="26"
          timestamp="..."/>
    </Condition>
  </ComponentStream>
</DeviceStream>
  <ComponentStream component="Controller" name="cont" componentId="cont">
    <Events>
       ...
    </Events>
    <Condition>
      <Normal type="MOTION_PROGRAM" dataItemId ="cc2" sequence="25"
         timestamp="..."/>
      <Normal type="COMMUNICATIONS" dataItemId ="cc1" sequence="26"
         timestamp="..."/>
      <Normal type="LOGIC_PROGRAM" dataItemId ="cc3" sequence="26"
         timestamp="..."/>
    </Condition>
  </ComponentStream>
</DeviceStream>
```

The example below shows all of the *Condition* items reporting that
everything is normal for the linear axis Y and that the controller has
two *Condition* that are normal, but there is a *Fault* of sub-type
*Communications* on the [device](/Terminology "wikilink").

``` xml

<DeviceStream uuid="HM1" name="HMC_3Axis">
  <ComponentStream component="Linear" name="Y" componentId="y">
    <Samples>
      <Position dataItemId="yp" name="Yact" subType="ACTUAL" sequence="23"
         timestamp="2009-11-13T08:00:00">213.1232</Position>
    </Samples>
    <Condition>
      <Normal type="TEMPERATURE" dataItemId="ytmp" sequence="25"
         timestamp="..."/>
      <Normal type="LOAD" dataItemId ="ylc" sequence="26" timestamp="..."/>
      <Normal type="POSITION" dataItemId ="ypc" sequence="26"
         timestamp="..."/>
    </Condition>
  </ComponentStream>
</DeviceStream>
  <ComponentStream component="Controller" name="cont" componentId="cont">
    <Events>
       ...
    </Events>
    <Condition>
      <Normal type="MOTION_PROGRAM" id="cc2" sequence="25" timestamp="..."/>
      <Fault type="COMMUNICATIONS" id="cc1" sequence="26" nativeCode="IO1231"
          timestamp="...">Communications error</Fault>
      <Normal type="LOGIC_PROGRAM" id="cc3" sequence="26" timestamp="..."/>
    </Condition>
  </ComponentStream>
</DeviceStream>
```

When a failure occurs the item MUST be reported as a *Fault*. This
indicates that intervention is required to fix the problem and reset the
state of the machine. In the following example, we show how multiple
*Faults* on the same *Condition* can exist.

``` xml

</DeviceStream>
  <ComponentStream component="Controller" name="cont" componentId="cont">
    <Events>
       ...
    </Events>
    <Condition>
      <Fault type="MOTION_PROGRAM" dataItemId="cc2" sequence="25"
          nativeCode="PR1123" timestamp="...">Syntax error on line
          107</Fault>
      <Fault type="MOTION_PROGRAM" dataItemId ="cc2" sequence="28"
          nativeCode="PR1123" timestamp="...">Syntax error on line
          112</Fault>
      <Fault type="MOTION_PROGRAM" dataItemId ="cc2" sequence="30"
          nativeCode="PR1123" timestamp="...">Syntax error on line
          122</Fault>
      <Normal type="COMMUNICATIONS" dataItemId ="cc1" sequence="26"
          timestamp="..."/>
      <Normal type="LOGIC_PROGRAM" dataItemId="cc3" sequence="26"
          timestamp="..."/>
    </Condition>
  </ComponentStream>
</DeviceStream>
```

In this case a bad motion program was loaded and multiple errors were
reported. When this occurs all errors MUST be provided and classified
accordingly. The only exception to having multiple values per
*Condition* is *Normal*. If the *Condition* is *Normal*, there MUST only
be one *Condition* with that type present. There MUST NOT be more than
one *Normal* and a *Normal* MUST NOT occur with a *Fault* or *Warning*
of the same type.

A *[Sample](/Terminology "wikilink")* request MUST treat *Condition*
items the same way it does *[Events](/Terminology "wikilink")* and
*[Samples](/Terminology "wikilink")* and only return those that are in
the current selection window.