---
title: Data Item Types - CONDITION
description: 
published: true
date: 2021-09-25T02:52:11.819Z
tags: 
editor: markdown
dateCreated: 2021-09-25T02:52:09.555Z
---

*Condition* is a *[DataItem](/Terminology "wikilink")* that indicates
the [device](/Terminology "wikilink")’s health and ability to operate.
They are reported differently than *[Samples](/Terminology "wikilink")*
or *[Events](/Terminology "wikilink")*: they MUST be reported as NORMAL,
WARNING, FAULT, or UNAVAILABLE. Unlike the other two categories, a
*[Component](/Terminology "wikilink")* or
*[Device](/Terminology "wikilink")* MAY have a *Condition* type
*[DataItem](/Terminology "wikilink")* that has multiple concurrently
active values at any point in time. Additionally, these items MAY be
further defined to provide differentiation for different condition
states; example an AMPERAGE *Condition* may differentiate between HIGH
amperage and LOW amperage.

All *[DataItems](/Terminology "wikilink")* in the
*[Sample](/Terminology "wikilink")* category MAY have associated
*Condition* states. *Condition* states indicate whether the value
reported for the data item is within an expected range (NORMAL) or the
value is unexpected or out of tolerance for the data item (WARNING or
FAULT).

While all *[DataItems](/Terminology "wikilink")* in the
*[Event](/Terminology "wikilink")* category MAY have associated
*Condition* states, many typically will not based on the type of data
that they represent.

The following table lists the *Condition* types which have been defined
to represent the health and fault status of *[Structural
Elements](/Terminology "wikilink")*

| Data Item type    | Description                                                                                                    |
| ----------------- | -------------------------------------------------------------------------------------------------------------- |
| `ACTUATOR`        | An actuator's status.                                                                                          |
| `CHUCK_INTERLOCK` | An indication of the operational condition of the interlock function for an electronically controlled chuck.   |
| `COMMUNICATIONS`  | A communications failure indicator.                                                                            |
| `DATA_RANGE`      | Information provided is outside of expected value range.                                                       |
| `DIRECTION`       | An indication of a fault associated with the direction of motion of a Structural Element.                      |
| `END_OF_BAR`      | An indication that the end of a piece bar stock has been reached.                                              |
| `HARDWARE`        | The hardware subsystem of the Structural Element's operation condition.                                        |
| `INTERFACE_STATE` | An indication of the operation condition of an *Interface*                                                     |
| `LOGIC_PROGRAM`   | An indication that an error has occurred in the logic program or PLC associated with a *Controller* component. |
| `MOTION_PROGRAM`  | An indication that an error has occurred in the motion program associated with a *Controller* component.       |
| `SYSTEM`          | A *Condition* representing something that is not the operator, program, or hardware.                           |