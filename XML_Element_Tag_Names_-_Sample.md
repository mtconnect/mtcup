---
title: XML Element Tag Names - Sample
description: 
published: true
date: 2021-09-24T00:33:05.624Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:33:03.034Z
---

[XML](/Terminology "wikilink") elements that can be placed in the
*[Samples](/Terminology "wikilink")* section of the *ComponentStream*:

**Acceleration** The acceleration of a linear component MUST always be
reported in *MILLIMETER/SECOND^2*. An *Acceleration* MUST have a numeric
value.

**AccumulatedTime** The accumulated time associated with a component.
The *AccumulatedTime* MUST have a numeric value and MUST be reported in
*SECOND*.

**Amperage** The current in an electrical circuit. The *Amperage* MUST
have a numeric value and MUST be reported in *AMPERE*.

**Angle** An *Angle* MUST always be reported in *DEGREE* and MUST always
have a numeric CDATA value as a floating point number.

**AngularAcceleration** The angular acceleration of the component as
measured in *DEGREE/SECOND^2*. An *Acceleration* MUST have a numeric
value.

**AngularVelocity** An angular velocity represents the rate of change in
angle. An *AngularVelocity* MUST always be reported in *DEGREE/SECOND*
and MUST always have a numeric CDATA value as a floating point number.

**AxisFeedrate** Axis Feedrate is defined as the rate of motion of the
linear axis of the tool relative to the workpiece. An *AxisFeedrate*
MUST always be reported in *MILLIMETER/SECOND* or *PERCENT* for
*OVERRIDE* and MUST always have a numeric CDATA value as a floating
point number.

**ClockTime** The reading of a timing device at a specific point in
time. The time MUST have a value reported in W3C ISO 8601 format of
*YYYY-MM-DDThh:mm:ss.ffff*

**Concentration** Percentage of one component within a mixture of
components. The *Concentration* MUST have a value reported in *PERCENT*.

**Conductivity** The ability of a material to conduct electricity. The
*Conductivity* MUST have a value reported in *SIEMENS/METER*.

'''Displacement ''' The displacement measured as the change in position
of an object. The *Displacement* MUST have a value reported in
*MILLIMETER*.

**ElectricalEnergy** The measurement of electrical energy consumed by a
component. *ElectricalEnergy* MUST have a value reported in
*WATT_SECOND*.

**Flow** The rate of flow of a fluid. The *Flow* MUST have a value
reported in *LITER/SECOND*.

**Frequency** The rate measurement of the number of occurrences of a
repeating event per unit time. The *Frequency* MUST have a numeric value
and MUST be reported in *HERTZ*.

**FillLevel** The measurement of the amount of a substance remaining
compared to the planned maximum amount of that substance. The
*FillLevel* MUST be reported in *PERCENT*.

**LinearForce** The measurement of the amount of push or pull introduced
by an actuator or exerted on an object. The *LinearForce* MUST be
reported in *NEWTON*.

**Load** The measurement of the percent of the standard rating of a
device. The *Load* MUST always be reported in *PERCENT* and MUST always
have a numeric CDATA value as a floating point number.

**Mass** The measurement of the mass of an object(s) or an amount of
material. The *Mass* MUST be reported in *KILOGRAM*.

**PathFeedrate** Path Feedrate is defined as the rate of motion of the
feed path of the tool relative to the workpiece . A *PathFeedrate* MUST
always be reported in *MILLIMETER/SECOND* or *PERCENT* for *OVERRIDE*
and MUST always have a numeric CDATA value as a floating point number.

**PathPosition** The program position as given in 3 dimensional space.
This position MUST default to *WORK* coordinates, if the *WORK*
coordinates are defined, and MUST be given as a space delimited vector
of floating point numbers given in *MILLIMETER_3D* units. The
*PathPosition* will be given in the following format and MUST be listed
in order X, Y, and Z: \<*PathPosition* …\>10.123 55.232
100.981\</*PathPosition*\> Where X = 10.123, Y = 55.232, and Z=100.981.

**PH** The measure of acidity or akalinity. The *PH* MUST be a numeric
value and MUST be provided in *PH*.

**Position** A position represents the location along a linear axis. A
*Position* MUST always be reported in *MILLIMETER* and MUST always have
a numeric CDATA value as a floating point number. The default coordinate
system for *Position* MUST be *MACHINE_COORDINATES*.

**PowerFactor** The measurement of the ratio of real power flowing to a
load to the apparent power in that AC circuit. The *PowerFactor* MUST be
a numeric value and MUST be provided in *PERCENT*.

**Pressure** The force per unit area exerted by a gas or liquid.
*Pressure* MUST be a numeric value and MUST be provided in *PASCAL*.

**Resistance** The measure of the degree to which an object opposes an
electrical current through it. The *Resistance* MUST be a numeric value
and MUST be provided in *OHM*.

**RotaryVelocity** The rate of rotation of a rotary axis. A
*RotaryVelocity* speed MUST always be reported in *REVOLUTION/MINUTE* or
*PERCEN*T for *OVERRIDE*.

**SoundLevel** The measure of acoustic sound level or sound pressure
level. The *SoundLevel* MUST be provided in *DECIBEL*.

**Strain** The measured amount of deformation per unit length of an
object. *Strain* MUST be reported as *PERCENT*.

**Temperature** *Temperature* MUST always be reported in degrees
*CELSIUS* and MUST always have a numeric CDATA value as a floating point
number.

**Tilt** The measured amount of angular displacement of an object.
*Tilt* MUST be reported as *MICRO_RADIAN*.

**Torque** The turning force exerted on an object or by an object.
*Torque* MUST be reported in units of *NEWTON_METER* and MUST have a
numeric CDATA value as a floating point number.

**Velocity** A velocity represents the rate of change in position along
one or more linear axis. When given as a sample for the *Axes*
component, it represents the magnitude of the velocity vector for all
given axis, similar to a path feedrate. A *Velocity* MUST always be
reported in *MILLIMETER/SECOND* and MUST always have a numeric CDATA
value as a floating point number.

**Viscosity** The measurement of a fluid’s resistance to flow.
*Viscosity* MUST be reported as *PASCAL_SECOND*.

**Voltage** The measurement of electrical potential between two points.
The *Voltage* MUST have a numeric value and MUST be reported in *VOLT*.

**VoltAmpere** The measurement of apparent power in an electrical
circuit, equal to the product of the RMS voltage and RMS current. The
*VoltAmpere* MUST have a numeric value and MUST be reported in
*VOLT_AMPERE*.

**VoltAmpereReactive** The measurement of reactive power in an AC
electrical circuit. The *VoltAmpereReactive* MUST have a numeric value
and MUST be reported in *VOLT_AMPERE_REACTIVE*.

**Wattage** The electrical power (volt-amps) consumed or dissipated by
an electrical circuit or device. The *Wattage* MUST have a numeric value
and MUST be reported in *WATT*.