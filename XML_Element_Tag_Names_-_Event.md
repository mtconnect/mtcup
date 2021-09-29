---
title: XML Element Tag Names - Event
description: 
published: true
date: 2021-09-25T02:30:16.802Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:32:59.694Z
---

The following is a list of all the [event](/Terminology "wikilink")
elements that may be placed within the
*[Events](/Terminology "wikilink")* section of the *ComponentStream*:

**ActiveAxes** The set of axes being controlled by a Path. The value
MUST be a space delimited set of axes names. For example: \<*ActiveAxes*
…\>X Y Z C\</*ActiveAxes*\> If this is not provided, it MUST assumed the
Path is controlling all the axes.

**ActuatorState** An actuator state represents a device for moving or
controlling a mechanism or system. The CDATA MUST be as follows:

![ActuatorStateTags.PNG](/images/ActuatorStateTags.PNG)

**Availabilty** Represents the component’s ability to communicate its
availability. This MUST be provided for the device and MAY be provided
for all other components.

![AvailabilityTags.PNG](/images/AvailabilityTags.PNG)

**AxisCoupling** Describes the way the axes will be associated to each
other. This is used in conjunction with *COUPLED_AXES* to indicate the
way they are interacting.

![AxisCouplingTags.PNG](/images/AxisCouplingTags.PNG)

**Block** A block of code is a command being executed by the Controller.
The *Block* MUST include the entire command with all the parameters.

**ControllerMode** The *Mode* of the Controller. The CDATA MUST be one
of the following:

![ControllerModeTags.PNG](/images/ControllerModeTags.PNG)

**CoupledAxes** As a *Linear* or *Rotary* axis data item, refers to the
set of associated axes to be used in conjunction with *AxisCoupling*.
The value will be a space delimited set of axes names. For example:
\<*CoupledAxes* …\>Y1 Y2\</*CoupledAxes*\>

**Direction** A Direction indicates the direction of rotation. The CDATA
MUST be as follows:

![DirectionTags.PNG](/images/DirectionTags.PNG)

**DoorState** A *DoorState* represents an opening that can be opened or
closed. The CDATA MUST be as follows:

![DoorStateTags.PNG](/images/DoorStateTags.PNG)

**Execution** The *Execution* state of the Controller. The CDATA MUST be
one of the following:

![ExecutionTags.PNG](/images/ExecutionTags.PNG)

**EmergencyStop** The emergency stop state of the machine, device, or
controller path. The CDATA MUST be one of the following:

![EmergencyStopTags.PNG](/images/EmergencyStopTags.PNG)

**Line** This [event](/Terminology "wikilink") refers to the optional
program line number. For example in RS274/NGC, the line number begins
with an N and is followed by 1 to 5 digits (0 – 99999). If there is not
an assigned line number in the programming system as in RS274, the line
number will refer to the position in the executing program. The line
number MUST be any positive integer from 0 to 2³²-1.

**Message** A text notification. Format MAY be any valid text string.

**PalletId** This is a reference to an identifier for the current pallet
available at the device.

**PartCount** The number of parts produced. This will not be counted by
the agent and MUST only be supplied if the controller provides the
count.

**PartId** This is a reference to an identifier for the current part
being machined. It is a placeholder for now and can be used at the
discretion of the implementation.

**PathMode** The *PathMode* is provided for devices that are controlling
multiple motion paths and their associated axes. When *PathMode* is not
provided, it MUST be assumed to be *INDEPENDENT*.

![PathModeTags.PNG](/images/PathModeTags.PNG)

**PowerState** Power state of a device or component. DEPRECATION
WARNING: MAY be deprecated in the future.

![PowerStateTags.PNG](/images/PowerStateTags.PNG)

**Program** The name of the program executing in the controller. This is
usually the name of the file containing the program instructions.

**RotaryMode** The mode in which the rotary axis is currently operating.
The CDATA MUST be one of the following:

![RotaryModeTags.PNG](/images/RotaryModeTags.PNG)

**ToolAssetId** This is a reference to an identifier for the current
tool in use by the *Path*.

**WorkholdingId** This is a reference to an identifier for the current
work holding or part clamp available to the device.