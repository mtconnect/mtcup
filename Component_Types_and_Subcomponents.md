---
title: Component Types and Subcomponents
permalink: /Component_Types_and_Subcomponents/
---

*[Component](/Terminology "wikilink")* is an abstract type that allows
for extensibility. As the specification progresses, more
[component](/Terminology "wikilink") types will be added to support new
[devices](/Terminology "wikilink") and parts of new
[devices](/Terminology "wikilink"). Some examples of
[component](/Terminology "wikilink") types are *Axes*, *Controller*, and
*Systems*. Any of these [component](/Terminology "wikilink") types can
have [data items](/Terminology "wikilink") and *subcomponents*. Appendix
B contains reference models for common equipment to guide developers in
implementing MTConnect on their [devices](/Terminology "wikilink").

## Axes

*Axes* is the root of all [device](/Terminology "wikilink")
[components](/Terminology "wikilink") that have linear or rotational
motion. Currently there are only *Linear* and *Rotary* axes supported
and when axes are defined the *Axes*
[component](/Terminology "wikilink") MUST contain at least one *Linear*
or *Rotary* axis. The Linear axes MUST be named X, Y, Z with numbers
appended for additional axes in the same plane, for example X2, Y2, and
Z2 are the secondary axes to X, Y, and Z. *Rotary* axes MUST be named A,
B, and C and rotate around the X, Y, and Z axes respectively. As with
the *Linear* axes, a number MUST be appended for additional axes in the
same plane.

The *Axes* represent the physical data for the axis
[components](/Terminology "wikilink"). When data is defined specifically
for the physical axes, positions MUST be given in *MACHINE* coordinates.
The *WORK* coordinates are represented in the *Path*
[component](/Terminology "wikilink") of the *Controller*.

DEPRECATION WARNING: In Version 1.1 of the MTConnect® standard, the
Spindle [component](/Terminology "wikilink") was no longer supported.
The Spindle will now be represented by a rotary axis that has a
*RotaryMode* of *SPINDLE*. The S(n) axis nomenclature SHOULD be removed
and replaced with A, B, or C to clearly identify which primary plane the
rotary axis is rotating around. All associated
*[DataItems](/Terminology "wikilink")* SHOULD now be named accordingly.

Note: The convention for multiple linear and rotary axes having the same
designation is to index the axes letter with a number. For this
standard, the secondary axis number starts at 2 (i.e. X, X2, X3, … or C,
C2, C3, C4, …). This is in compliance with the ISO-841-2001.

Figure one below shows an axes example with three linear axes and one
rotary axis.

[none|500px|thumb|Figure 1: Axes
Example](/File:AxesExample.PNG "wikilink")

**LINEAR**

  -
    A linear axis represents the movement of a physical
    [device](/Terminology "wikilink"), or a portion of a
    [device](/Terminology "wikilink"), in a straight line. Movement may
    be in either a positive or negative direction.

**ROTARY**

  -
    An axis whose function is to provide rotary motion may function as a
    continuous rotation (i.e. spindle mode), continuous-path contour
    cutting in a rotary motion (i.e. contouring), or repositioning (i.e.
    indexing) different faces of the part. As such, a rotary axis MUST
    operate in one of the three following modes: *SPINDLE*, *INDEX*, or
    *CONTOUR*.

## Controller

The *Controller* [component](/Terminology "wikilink") represents an
intelligent [device](/Terminology "wikilink"). Examples include a *CNC*
(Computer Numerical Control) or *PAC* (Programmable Automation Control)
which may be referred to as a *Motion Control* or *General Purpose
Motion Control*. The *Controller* provides information regarding the
execution of a control program and the execution state of the
[device](/Terminology "wikilink"). There are no required *subcomponents*
of the *Controller*.

*Note*: MTConnect *Version* 1.1.0 and later implementations SHOULD use a
*Path* sub-component to represent an individual tool path and execution
state (see *Path*). When the machine is capable of executing more than
one simultaneous program, the implementation MUST use the *Path*
[component](/Terminology "wikilink").

### Path

For more complex [devices](/Terminology "wikilink") and controllers,
each path will be represented by a *Path* *subcomponent*. A *Path*
represents the motion of a control point as it moves through space as
controlled by a set of control instructions (i.e. vector move). The
*Path* will encapsulate the position, feedrate, and rotation of the
control point as presented by the controller. The control point is the
positioning of a tool at a point in space.

If the controller is capable of running more than one task
simultaneously, a *Path* [component](/Terminology "wikilink") MUST be
given for each task under the *Controller*
[component](/Terminology "wikilink").

## Door

This [component](/Terminology "wikilink") represents a door closure that
can be opened or closed. It MUST have a
*[DataItem](/Terminology "wikilink")* called *DoorState* to indicate if
it is opened, closed or unlatched. A [device](/Terminology "wikilink")
may contain multiple door [components](/Terminology "wikilink").

## Actuator

An *Actuator* is a [device](/Terminology "wikilink") for moving or
controlling a mechanism or system. It takes energy, usually transported
by air, electric current, or liquid and converts it into some kind of
motion. An *Actuator* may be a *[Component](/Terminology "wikilink")* of
a *[Device](/Terminology "wikilink")* or it may be a *subcomponent* of a
parent *[Component](/Terminology "wikilink")*.

## Sensor

*Sensor* is an abstract type [component](/Terminology "wikilink") that
provides measurement data related to a
*[Device](/Terminology "wikilink")* or
*[Component](/Terminology "wikilink")*. Depending on the type of data
provided by the sensor, it may be modeled in the
[XML](/Terminology "wikilink") schema in different ways. However, it
will always be modeled to associate the data contained in *Sensor* with
the *[Component](/Terminology "wikilink")*
[XML](/Terminology "wikilink") Element to which the data is most closely
associated.

A sensor is typically comprised of two major
[components](/Terminology "wikilink") – the *sensing element* (provides
a signal or measured value) and the *sensor interface* (signal
processing, conversion, and communications). In MTConnect, the *sensor
interface* is modeled as a *[Component](/Terminology "wikilink")* called
*Sensor*. The *sensing element* or measured value is modeled as a
*[DataItem](/Terminology "wikilink")*. Example: A pressure transducer
could be modeled as a *Sensor* (*[Component](/Terminology "wikilink")*)
with a *name* = *Pressure Transducer B* and its measured value could be
modeled as a *[DataItem](/Terminology "wikilink")* of type PRESSURE.

*[Sensor](/Terminology "wikilink")* MUST NOT be modeled in the plural.
*[Sensor](/Terminology "wikilink")* will always refer to the *sensor
interface*. Each *sensor interface* may have multiple *sensing
elements*; each representing the data for a variety of measured values.

### Sensor Data

The most basic implementation of a *sensing element* is the providing of
a measured value associated with a
*[Component](/Terminology "wikilink")* which is the *Sensor* data. An
example would be the measured value of the *Temperature* of the spindle
(Rotary Axis C). This would be represented as a
*[DataItem](/Terminology "wikilink")* called *Temperature* that is
associated with the Rotary Axis C as follows:

``` xml

      <Components>
        <Axes
          <Components>
            <Rotary id="c" name="C">
              <DataItems>
                <DataItem type="TEMPERATURE" id="ctemp" category="SAMPLE"
                   name="Stemp" units="DEGREE"/>
              </DataItems>
            </Rotary>
          </Components>
        </Axes>
      </Components>
```

A sensor may measure values associated with any
*[Component](/Terminology "wikilink")*, *Sub-Component*, or
*[Device](/Terminology "wikilink")*. Some examples of how sensor data
may be modeled are represented in Figure 2 below:

[none|500px|thumb|Figure 2: Sensor Data
Associations](/File:SensorDataAssociations.PNG "wikilink")

### Sensor Interface

*Sensing element(s)* are most typically connected to a *sensor
interface*. The *sensor interface* provides additional information
concerning the *sensing element(s)*.

Typical functions of the *sensor interface* include:

  - convert low level signals from the *sensing elements* into data that
    can be used by other [devices](/Terminology "wikilink"). (Example:
    Convert a non-linear millivolt signal from a temperature sensor into
    a scaled temperature value that can be transmitted to another
    [device](/Terminology "wikilink").)

<!-- end list -->

  - process *sensing element* data into calculated values. (Example:
    temperature sensor data is converted into calculated values of
    average temperature, maximum temperature, minimum temperature, etc.)

<!-- end list -->

  - provide calibration and configuration information associated with
    each *sensing element*.

<!-- end list -->

  - monitor the health and integrity of the *sensing elements* and the
    *sensor interface*. (Example: The *sensor interface* may provide
    diagnostics on each *sensing element* (e.g. open wire detection) and
    itself (e.g. measure internal temperature of the *sensor
    interface*).

The *sensor interface* is modeled in the [XML](/Terminology "wikilink")
schema as a *[Component](/Terminology "wikilink")* called
*[Sensor](/Terminology "wikilink")*. *Sensor* SHOULD be modeled in the
[XML](/Terminology "wikilink") schema so that the *Sensor* is
represented as part of the *[Component](/Terminology "wikilink")* to
which it is most closely associated.

*Sensor* may be associated with any
*[Component](/Terminology "wikilink")*, Sub-Component, or
*[Device](/Terminology "wikilink")*. Some examples of where a sensor may
be modeled are represented in Figure 3 below:

[none|500px|thumb|Figure 3: Sensor
Associations](/File:SensorAssociations.PNG "wikilink")

When a *Sensor* is modeled as a *[Component](/Terminology "wikilink")*,
it MAY have its own [uuid](/Terminology "wikilink") so it can be tracked
throughout its lifetime.

The following examples demonstrate how *Sensor* may be modeled in the
[XML](/Terminology "wikilink") schema differently based on how the
sensor functions within the overall *[Device](/Terminology "wikilink")*:

**Example 1**: If *Sensor* provides vibration measurement data for the
spindle, it should be modeled as a *Sensor* for Rotary Axis C.

``` xml

      <Components>
        <Axes>
          <Components>
            <Rotary id="c" name="C">
              <Sensor id="spdlm" name="Spindlemonitor">
         <DataItems>
                    <DataItem type="DISPLACEMENT" id="cvib" category="SAMPLE"
                   name="Svib" units="MILLIMETER"/>
                 </DataItems>
              </Sensor>
            </Rotary>
          </Components>
        </Axes>
      </Components>
```

**Example 2**: If *Sensor* provides measurement data for multiple
*[Components](/Terminology "wikilink")* within a
*[Device](/Terminology "wikilink")* and is not associated with any
particular *[Component](/Terminology "wikilink")*, it MAY be modeled in
the [XML](/Terminology "wikilink") schema as an independent
*[Component](/Terminology "wikilink")* of the
*[Device](/Terminology "wikilink")*.

``` xml

      <Device id="d1" uuid="HM1" name="HMC_3Axis">
        <Description>3 Axis Mill</Description>
        <Components>
          <Sensor id="sensor" name="sensor"/>
             <DataItems>
               <DataItem type="TEMPERATURE" id="sentemp" category="SAMPLE"
                     name="Sensortemp" units="DEGREE"/>
            </DataItems>
      </Components>
    </Device>
```

While *Sensor* MAY be modeled in different ways in the
[XML](/Terminology "wikilink") schema, the measured value of the
*sensing element* MUST always be modeled as a
*[DataItem](/Terminology "wikilink")* associated with the
*[Component](/Terminology "wikilink")* to which the measured value is
most closely associated.

The following represents a sensor with two *sensing elements*, one
measures spindle vibration and the other measures the temperature for
the X axis. The sensor also has a *sensing element* measuring the
internal temperature of the *sensor interface*:

``` xml

      <Device id="d1" uuid="HM1" name="HMC_3Axis">
        <Description>3 Axis Mill</Description>
        <Components>
          <Sensor id="sens1" name="Sensorunit">
         <DataItems>
               <DataItem type="TEMPERATURE" id="sentemp" category="SAMPLE"
                     name="Sensortemp" units="DEGREE"/>
             </DataItems>
          </Sensor>
          <Axes>
            <Components>
              <Rotary id="c" name="C">
           <DataItems>
                  <DataItem type="DISPLACEMENT" id="cvib" category="SAMPLE"
                   name="Svib" units="MILLIMETER"/>
               </DataItems>
              </Rotary>
              <Linear id="x" name="X">
                <DataItems>
                  <DataItem type="TEMPERATURE" id="xt"
                   category="SAMPLE" name="Xtemp" units="DEGREE"/>
                </DataItems>
              </Linear>
            </Components>
          </Axes>
        </Components>
      </Device>
```

### Sensor as a Device

A *sensor* may function as an independent
[device](/Terminology "wikilink"). In this case, it is not associated
with a parent *[Device](/Terminology "wikilink")* or
*[Component](/Terminology "wikilink")*.

Examples of a sensor functioning as a
*[Device](/Terminology "wikilink")* would be a sensor used to monitor
the ambient temperature of a building or an air quality monitoring
system. Another example would be a vibration monitoring system that is
moved from one machine to another. In these cases, the sensor functions
as an intelligent device performing a specific function.

A sensor functioning as a *[Device](/Terminology "wikilink")* would be
modeled in the [XML](/Terminology "wikilink") schema as follows:

``` xml

    <Device id="s1" uuid="HM1" name="AMBIENT_MONITOR">
      <Description>Ambient Temperature Monitor</Description>
        <DataItems>
        <DataItem type="TEMPERATURE" id="ambtemp" category="SAMPLE"
           name="Ambienttemp" units="DEGREE"/>
      </DataItems>
    </Device>
```

A sensor that is modeled as a [device](/Terminology "wikilink") MUST
have an [uuid](/Terminology "wikilink") so that it can be uniquely
tracked.

## Sensor Configuration

When a sensor is modeled in the [XML](/Terminology "wikilink") schema as
a *[Component](/Terminology "wikilink")* or a
*[Device](/Terminology "wikilink")*, it may provide additional
configuration information for the *sensor elements* and the *sensor
interface* itself.

The *Sensor* configuration data provides information required for
maintenance and support of the sensor.

*Sensor* configuration data is only available when the sensor is modeled
as a *[Component](/Terminology "wikilink")* or a
*[Device](/Terminology "wikilink")*. Details specific to
*SensorConfigurationType* are provided below.

When *Sensor* represents the *sensor interface* for multiple *sensing
element(s)*, each *sensing element* is represented by a *Channel*. Each
*Channel* represents one *sensing element* and can have its own
[attributes](/Terminology "wikilink") and *Configuration* data.

[none|500px|thumb|Figure 4: Configuration Data for
Sensors](/File:SensorConfiguration.PNG "wikilink")

| Element                                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Occurrence |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| Configuration(SensorConfigurationType) | An element that can contain descriptive content defining the configuration information for Sensor. For Sensor, the valid configuration is SensorConfiguration. SensorConfiguration provides data from a subset of items commonly found in a transducer electronic data sheet for sensors and actuators called TEDS. TEDS formats are defined in IEEE 1451.0 and 1451.4 transducer interface standards (ref 15 and 16, respectively). MTConnect does not support all of the data represented in the TEDS data, nor does it duplicate the function of the TEDS data sheets. | 0..1       |

### SensorConfiguration Elements

| Element             | Description                                                                                                         | Occurrence |
| ------------------- | ------------------------------------------------------------------------------------------------------------------- | ---------- |
| FirmwareVersion     | Version number for the sensor as specified by the manufacturer                                                      | 1          |
| CalibrationDate     | Date upon which the sensor was last calibrated. Dates MUST be represented in the W3C ISO 8601 format                | 0..1       |
| NextCalibrationDate | Date upon which the sensor is next scheduled to be calibrated. Dates MUST be represented in the W3C ISO 8601 format | 0..1       |
| CalibrationInitials | The initials of the person verifying the validity of the calibration data                                           | 0..1       |
| Channels            | When Sensor represents multiple sensing elements, each sensing element is represented by a Channel for the Sensor.  | 0..1       |

### Sensor Channel Attributes

*Channel* represents each *sensing element* connected to a *sensor
interface*. Each *Sensor Channel* has the following composition:

| Attribute | Description                                                                                                                                                                                                                      | Occurrence |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| Number    | A unique identifier that will only refer to this sensing element. For example, this can be the manufacturer code and the serial number. The Number should be alphanumeric and not exceeding 255 characters. An NMTOKEN XML type. | 1          |
| Name      | The Name of the sensing element. This name should be unique within the machine to allow for easier data integration. An NMTOKEN XML type.                                                                                        | 0..1       |

### Sensor Channel Elements

| Element             | Description                                                                                                                                       | Occurrence |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| Description         | An XML element that can contain any descriptive content. This can contain information about the sensor element and manufacturer specific details. | 0..1       |
| CalibrationDate     | Date upon which the sensor element was last calibrated. Dates MUST be represented in the W3C ISO 8601 format                                      | 0..1       |
| NextCalibrationDate | Date upon which the sensor element is next scheduled to be calibrated. Dates MUST be represented in the W3C ISO 8601 format                       | 0..1       |
| CalibrationInitials | The initials of the person verifying the validity of the calibration data                                                                         | 0..1       |

The following is an example of the configuration data for *Sensor* that
is modeled as a *[Component](/Terminology "wikilink")*. It has
*Configuration* data for the *sensor interface*, one *Channel* named
A/D:1, and two *[DataItems](/Terminology "wikilink")* – Voltage (as a
*[Sample](/Terminology "wikilink")*) and *Voltage* (as a *Condition* or
alarm).

``` xml

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
```

## Sensor Types

For a full list of measurements provided by *Sensor*, [click
here](/Sensor_Types "wikilink").