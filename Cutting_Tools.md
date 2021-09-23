---
title: Cutting Tools
permalink: /Cutting_Tools/
---

An Asset is something that is associated with the manufacturing process
that is not a [component](/Terminology "wikilink") of a
[device](/Terminology "wikilink"), can be removed without detriment to
the function of the [device](/Terminology "wikilink"), and can be
associated with other [devices](/Terminology "wikilink") during their
lifecycle. An asset does not have computational capabilities, but may
carry information in some media physically attached to the asset.

[center|500px|thumb|Figure 1: Assets
Schema](/File:AssetsSchema.PNG "wikilink")

Concrete examples of Assets are things like Cutting Tools, Workholding
Systems, and Fixtures. This page will go over the modeling of Cutting
Tools and the management and communication of asset data using
MTConnect.

## Cutting Tool

A Cutting Tool is an assembly of items for removing material from a
work-piece through a shearing action at the defined cutting edge or
edges of the [Cutting Item](/Cutting_Items "wikilink"). A Cutting Tool
can be a single item or an assembly of one or more Adaptive Items, a
Tool Item and several Cutting Items on a Tool Item.

MTConnect will adopt the ISO 13399 structure when formulating the
vocabulary for cutting tool geometries and structure. MTConnect will
focus on the application of the cutting tool and cutting items. At this
time we are only concerned with two aspects of the cutting tool, the
Cutting Tool and the Cutting Item. The Tool Item, Adaptive Item, and
Assembly Item will be covered in the *CuttingToolDefinition* section of
this page since this section contains the full ISO 13399 information
about a Cutting Tool.

[left|500px|thumb|Figure 2: Cutting Tool
Parts](/File:CuttingToolParts.PNG "wikilink") [center|500px|thumb|Figure
3: Cutting Tool
Composition](/File:CuttingToolComposition.PNG "wikilink")
Figure 2 illustrates the parts of a cutting tool. The cutting tool is
the aggregate of all the components and the cutting item is the part of
the tool that removes the material from the workpiece. These are the
primary focus of MTConnect.

Figure 3 provides another view of the cutting tool composition model.
The adaptive items and tool items will be used for measurements, but
will not be modeled as separate entities. When we are referencing the
cutting tool we are referring to the entirety of the assembly and when
we provide data regarding the cutting item we are referencing each
individual item as illustrated on the left of the previous diagram.

[left|500px|thumb|Figure 4: Cutting Tool, Tool Item, and Cutting
Item](/File:CTTICI.PNG "wikilink") [center|500px|thumb|Figure 5: Cutting
Tool, Tool Item, and Cutting Item](/File:CTTICI2.PNG "wikilink")
Figures 4 and 5 further illustrate the components of the cutting tool.
As we compose the Tool Item, Cutting Item, Adaptive Item, we get a
Cutting Tool. The Tool Item, Adaptive Item, and Assembly Item will only
be in the *CuttingToolDefinition* section that will contain the full ISO
13399 information.

The above diagrams use the ISO 13399 codes for each of the measurements.
These codes will be translated into the MTConnect vocabulary as
illustrated below. The measurements will have a maximum, minimum, and
nominal value representing the tolerance of allowable values for this
dimension.

[center|500px|thumb|Figure 6: Cutting Tool
Measurements](/File:CuttingToolMeasurements.PNG "wikilink")

The MTConnect standard will not define the entire geometry of the
cutting tool, but will provide the information necessary to use the tool
in the manufacturing process. Additional information can be added to the
definition of the cutting tool by means of schema extensions.

Additional diagrams will reference these dimensions by their codes that
will be defined in the measurement tables. The codes are consistent with
the codes used in ISO 13399 and have been standardized. MTConnect will
use the full text name for clarity in the XML document.

[left|500px|thumb|Figure 7: Cutting Tool Asset
Structure](/File:CuttingToolAssetStructure.PNG "wikilink")
[center|500px|thumb|Figure 8: Cutting Tool
Schema](/File:CuttingToolSchema.PNG "wikilink")
**Cutting Tool Attributes:**

| Attribute     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                     | Occurrence |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| timestamp     | The time this asset was last modified. Always given in UTC. The timestamp MUST be provided in UTC (Universal Time Coordinate, also known as GMT). This is the time the asset data was last modified.                                                                                                                                                                                                                                            | 1          |
| assetId       | The unique identifier of the instance of this tool. The unique identifier of the instance of this tool. This will be the same as the toolId and serialNumber in most cases. The assetId SHOULD be the combination of the toolId and serialNumber as in toolId.serialNumber or an equivalent implementation dependent identification scheme.                                                                                                     | 1          |
| serialNumber  | The unique identifier for this assembly. The unique identifier for this assembly. This is defined as an XML string type and is implementation dependent.                                                                                                                                                                                                                                                                                        | 1          |
| toolId        | The identifier for the class of cutting tool. The identifier for a class of cutting tools. This is defined as an XML string type and is implementation dependent.                                                                                                                                                                                                                                                                               | 1          |
| deviceUuid    | The device’s UUID that supplied this data. This optional element References to the UUID attribute given in the device element. This can be any series of numbers and letters as defined by the XML type NMTOKEN.                                                                                                                                                                                                                                | 1          |
| manufacturers | The manufacturers of the cutting tool. An optional attribute referring to the manufacturers of this tool, for this element, this will reference the Tool Item and Adaptive Items specifically. The Cutting Items manufacturers’ will be an attribute of the CuttingItem elements. The representation will be a comma (,) delimited list of manufacturer names. This can be any series of numbers and letters as defined by the XML type string. | 0..1       |

## Cutting Tool Elements

The elements associated with this cutting tool are given below. Each
element will be described in more detail below and any possible values
will be presented with full definitions. The elements MUST be provided
in the following order as prescribed by [XML](/Terminology "wikilink").
At least one of *CuttingToolDefinition* or *CuttingToolLifeCycle* MUST
be supplied.

| Element               | Description                                                                                                                                                                                                                                                         | Occurrence |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| Description           | An element that can contain any descriptive content. This can contain configuration information and manufacturer specific details. This element is defined to contain mixed content and XML elements can be added to extend the descriptive semantics of MTConnect. | 0..1       |
| CuttingToolDefinition | Reference to a ISO 13399                                                                                                                                                                                                                                            | 0..1       |
| CuttingToolLifeCycle  | MTConnect data regarding the use phase of this tool.                                                                                                                                                                                                                | 0..1       |

### Description

The description MAY contain mixed content, meaning that an additional
[XML](/Terminology "wikilink") element or plain text may be provided as
part of the content of the description [tag](/Terminology "wikilink").
Currently the description contains no additional
[attributes](/Terminology "wikilink").

### CuttingToolDefinition

[center|500px|thumb|Figure 9: Cutting Tool
Definition](/File:CuttingToolDefinition.PNG "wikilink")

The *CuttingToolDefinition* contains the detailed structure of the
cutting tool. The information contained in this element will be static
during its lifecycle. Currently we are referring to the external ISO
13399 standard to provide the complete definition and composition of the
cutting tool.

***CuttingToolDefinition* Attributes:**

| Attribute | Description                                             | Occurrence |
| --------- | ------------------------------------------------------- | ---------- |
| format    | Format – EXPRESS, XML, TEXT, or UNDEFINED. Default: XML | 0..1       |

***format*:**

The format [attribute](/Terminology "wikilink") describes the expected
representation of the enclosed data. If no value is given, the assumed
format will be *[XML](/Terminology "wikilink")*.

| Value       | Description                                                                        |
| ----------- | ---------------------------------------------------------------------------------- |
| `XML`       | The default value for the definition. The content will be an XML document.         |
| `EXPRESS`   | The document will confirm to the ISO 10303 standard. STEP-NC part 21 file formats. |
| `TEXT`      | The document will be a text representation of the tool data.                       |
| `UNDEFINED` | The document will be provided in an undefined format.                              |

***CuttingToolDefinition* Elements:**

The only acceptable cutting tool definition at present is ISO 13399.
Additional formats MAY be considered in the future.

### ISO 13399

The ISO 13399 data MUST be presented in either
[XML](/Terminology "wikilink") (ISO 10303-28) or EXPRESS format (ISO
10303-21). An [XML](/Terminology "wikilink") schema will be preferred as
this will allow for easier integration with the MTConnect
[XML](/Terminology "wikilink") tools. EXPRESS will also be supported,
but software tools will need to be provided or made available for
handling this data representation.

There will be the root element of the ISO13399 document when
[XML](/Terminology "wikilink") is used. When EXPRESS is used the
[XML](/Terminology "wikilink") element will be replaced by the text
representation.

### CuttingToolLifeCycle

[right|500px|thumb|Figure 10: Cutting Tool Life
Cycle](/File:CuttingToolLifeCycle.PNG "wikilink")

The life cycle refers to the data pertaining the the
[application](/Terminology "wikilink") or the use of the tool. This data
is provided by various [devices](/Terminology "wikilink"), machine tool,
presetters, and statistical process control
[applications](/Terminology "wikilink"). Life cycle data will not remain
static, but will change periodically when a tool is used or measured.
The life cycle has three conceptual parts; tool and cutting item
identity, properties, and measurements. A measurement is defined as a
constrained value that is reported in defined units and as a W3C
floating point format.

The *CuttingToolLifeCycle* contains data for the entire tool assembly.
The specific cutting items that are part of the *CuttingToolLifeCycle*
are contained in the *CuttingItems* element. Each cutting item has
similar properties as the assembly; identity, properties, and
measurements.

***CuttingToolLifeCycle* Elements:**

The elements associated with this cutting tool are given below. Each
element will be described in more detail below and any possible values
will be presented with full definitions. The elements MUST be provided
in the following order as prescribed by [XML](/Terminology "wikilink").

| Element                   | Description                                                                                                                                                                                                                       | Occurrence |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| CutterStatus              | The status of the this assembly. Can be one more of the following values: `NEW`, `AVAILABLE`,`UNAVAILABLE`, `ALLOCATED`, `UNALLOCATED`, `MEASURED`, `RECONDITIONED`, `NOT_REGISTERED`, `USED`, `EXPIRED`, `BROKEN`, or `UNKNOWN`. | 1          |
| ReconditionCount          | The number of times this cutter has been reconditioned.                                                                                                                                                                           | 0..1       |
| ToolLife                  | The cutting tool life as related to this assembly                                                                                                                                                                                 | 0..1       |
| Location                  | The location this tool now resides in.                                                                                                                                                                                            | 0..1       |
| ProgramToolGroup          | The tool group this tool is assigned in the part program.                                                                                                                                                                         | 0..1       |
| ProgramToolNumber         | The number of the tool as referenced in the part program.                                                                                                                                                                         | 0..1       |
| ProcessSpindleSpeed       | The constrained process spindle speed for this tool                                                                                                                                                                               | 0..1       |
| ProcessFeedRate           | The constrained process feed rate for this tool in mm/s.                                                                                                                                                                          | 0..1       |
| ConnectionCodeMachineSide | Identifier for the capability to connect any component of the cutting tool together, except assembly items, on the machine side. Code: CCMS                                                                                       | 0..1       |
| Measurements              | A collection of measurements for the tool assembly.                                                                                                                                                                               | 0..1       |
| CuttingItems              | An optional set of individual cutting items.                                                                                                                                                                                      | 0..1       |

***CutterStatus*:**

The elements of the *CutterStatus* element can be a combined set of
*Status* elements. The standard allows any set of statuses to be
combined, but only certain combinations make sense. A cutting tool
SHOULD not be both `NEW` and `USED` at the same time. There are no rules
in the schema to enforce this, but this is left to the implementer.

[center|500px|thumb|Figure 11:
*CutterStatus*](/File:CutterStatus.PNG "wikilink")

The following combinations MUST NOT occur:

  - `NEW` MUST NOT be used with `USED`, `RECONDITIONED`, or `EXPIRED`.
  - `UNKNOWN` MUST NOT be used with any other status.
  - `ALLOCATED` and `UNALLOCATED` MUST NOT be used together.
  - `AVAILABLE` and `UNAVAILABLE` MUST NOT be used together.
  - If the tool is `EXPIRED`, `BROKEN`, or `NOT_REGISTERED` it MUST NOT
    be `AVAILABLE`.
  - All other combinations are allowed.

| Element | Description                                                            | Occurrence |
| ------- | ---------------------------------------------------------------------- | ---------- |
| Status  | The status of the cutting tool. There can be multiple Status elements. | 1...INF    |

**Status:**

These are the values for the status of the cutting tool:

| Value            | Description                                                                                                                                                                                                                   |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `NEW`            | A new tool that has not been used or first use. Marks the start of the tool history.                                                                                                                                          |
| `AVAILABLE`      | Indicates the tool is available for use. If this is not present, the tool is currently not ready to be used                                                                                                                   |
| `UNAVAILABLE`    | Indicates the tool is unavailable for use in metal removal. If this is not present, the tool is currently not ready to be used                                                                                                |
| `ALLOCATED`      | Indicates if this tool is has been committed to a device for use and is not available for use in any other device. If this is not present, this tool has not been allocated for this device and can be used by another device |
| `UNALLOCATED`    | Indicates this Cutting Tool has not been committed to a process and can be allocated.                                                                                                                                         |
| `MEASURED`       | The tool has been measured.                                                                                                                                                                                                   |
| `RECONDITIONED`  | The cutting tool has been reconditioned. See ReconditionCount for the number of times this cutter has been reconditioned.                                                                                                     |
| `USED`           | The tool is in process and has remaining tool life.                                                                                                                                                                           |
| `EXPIRED`        | The cutting tool has reached the end of its useful life.                                                                                                                                                                      |
| `BROKEN`         | Premature tool failure.                                                                                                                                                                                                       |
| `NOT_REGISTERED` | This cutting tool cannot be used until it is entered into the system.                                                                                                                                                         |
| `UNKNOWN`        | The cutting tool is an indeterminate state. This is the default value.                                                                                                                                                        |

## Location

[center|500px|thumb|Figure 12: Location](/File:Location.PNG "wikilink")

This is the optional device specific pocket id providing the current
pocket number this tool resides in. This can be any series of numbers
and letters as defined by the [XML NMTOKEN](/Terminology "wikilink").
When a *POT* or *STATION* type is used, the value MUST be a numeric
value. If a *negativeOverlap* or the *positiveOverlap* is provided, the
tool reserves additional locations on either side, otherwise if they are
not given, no additional locations are required for this tool. If the
pot occupies the first or last location, a rollover to the beginning or
the end of the index-able values may occur. For example, if there are 64
pots and the tool is in pot 64 with a *positiveOverlap* of 1, the first
pot MAY be occupied as well.

***Location* attributes:**

| Attribute       | Description                                                                          | Occurrence |
| --------------- | ------------------------------------------------------------------------------------ | ---------- |
| type            | The type of location being identified. Current MUST be one of POT, STATION, or CRIB. | 1          |
| positiveOverlap | The number of locations at higher index value from this location.                    | 0..1       |
| negativeOverlap | The number of location at lower index values from this location.                     | 0..1       |

***type*:**

| Value     | Description                                        |
| --------- | -------------------------------------------------- |
| `POT`     | The number of the pot in the tool handling system. |
| `STATION` | The tool location in a horizontal turning machine. |
| `CRIB`    | The location with regard to a tool crib.           |

### positiveOverlap

The number of locations at higher index values that the cutting tool
occupies due to interference. The value MUST be an integer. If not
provided it is assumed to be 0.

### negativeOverlap

The number of locations at lower index values that the cutting tool
occupies due to interference. The value MUST be an integer. If not
provided it is not assumed to be 0.

The tool number assigned in the part program and is used for cross
referencing this tool information with the process parameters. The value
MUST be an integer.

### ProgramToolGroup

The optional identifier for the group of cutting tools when multiple
tools can be used interchangeably. This is defined as an
[XML](/Terminology "wikilink") string type and is implementation
dependent.

### ProgramToolNumber

The tool number assigned in the part program and is used for cross
referencing this tool information with the process parameters. The value
MUST be an integer.

### ReconditionCount

[center|500px|thumb|Figure 13: Cutting Tool Life
Cycle](/File:ReconditionCount.PNG "wikilink")

This element MUST contain an integer value as the
[CDATA](/Terminology "wikilink") that represents the number of times the
cutter has been reconditioned.

***ReconditionCount* attributes:**

| Attribute    | Description                                                | Occurrence |
| ------------ | ---------------------------------------------------------- | ---------- |
| maximumCount | The maximum number of times this tool may be reconditioned | 0..1       |

### Tool Life

[center|500px|thumb|Figure 14: Tool Life](/File:ToolLife.PNG "wikilink")

The value is the current value for the tool life. The value MUST be a
number. Tool life is an option element which can have three types,
either minutes for time based, part count for parts based, or wear based
using a distance measure. One tool life element can appear for each
type, but there cannot be two entries of the same type. Additional types
can be added in the future.

***ToolLife* attributes:**

| Attribute      | Description                                                                                                                                                                                                                       | Occurrence |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| type           | The type of tool life being accumulated. `MINUTES`, `PART_COUNT`, or `WEAR`                                                                                                                                                       | 1          |
| countDirection | Indicates if the tool life counts from zero to maximum or maximum to zero, The values MUST be one of `UP` or `DOWN`.                                                                                                              | 1          |
| warning        | The point at which a tool life warning will be raised.                                                                                                                                                                            | 0..1       |
| limit          | The end of life limit for this tool. If the countDirection is `DOWN`, the point at which this tool should be expired, usually zero. If the countDirection is `UP`, this is the upper limit for which this tool should be expired. | 0..1       |
| initial        | The initial life of the tool when it is new.                                                                                                                                                                                      | 0..1       |

***ToolLife type* attribute:**

The value of *type* must be one of the following:

| Value        | Description                                                                                                                                                                                                                                                     |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `MINUTES`    | The tool life measured in minutes. All units for minimum, maximum, and warningLevel MUST be provided in minutes.                                                                                                                                                |
| `PART_COUNT` | The tool life measured in parts. All units for minimum, maximum, and warningLevel MUST be provided supplied as the number of parts.                                                                                                                             |
| `WEAR`       | The tool life measured in tool wear. Wear MUST be provided in millimeters as an offset to nominal. All units for minimum, maximum, and warningLevel MUST be given as millimeter offsets as well. The standard will only consider dimensional wear at this time. |

***ToolLife countDirection* attributes:**

The value of *type* must be one of the following:

| Value  | Description                                         |
| ------ | --------------------------------------------------- |
| `DOWN` | The tool life counts down from the maximum to zero. |
| `UP`   | The tool life counts up from zero to the maximum.   |

### ProcessSpindleSpeed

[center|500px|thumb|Figure 15: Process Spindle
Speed](/File:ProcessSpindleSpeed.PNG "wikilink")

The Process Spindle Speed MUST be specified in revolutions/minute (RPM).
The [CDATA](/Terminology "wikilink") MAY contain the process target
spindle speed if available. The maximum and minimum speeds MAY be
provided as attributes. At least one value MUST be provided.

***ProcessSpindleSpeed* attributes:**

| Attribute | Description                                           | Occurrence |
| --------- | ----------------------------------------------------- | ---------- |
| maximum   | The upper bound for the tool’s target spindle speed   | 0..1       |
| minimum   | The lower bound for the tools spindle speed.          | 0..1       |
| nominal   | The nominal speed the tool is designed to operate at. | 0..1       |

### ProcessFeedRate

[center|500px|thumb|Figure 16: Process Feed
Rate](/File:ProcessFeedRate.PNG "wikilink")

The Process Feed Rate MUST be specified in millimeters/second (mm/s).
The [CDATA](/Terminology "wikilink") MAY contain the process target feed
rate if available. The maximum and minimum rates MAY be provided as
[attributes](/Terminology "wikilink"). At least one value MUST be
provided.

***ConnectionCodeMachineSide***:

This is an optional identifier for implementation specific connection
component of the cutting tool on the machine side. Code: CCMS. The
[CDATA](/Terminology "wikilink") MAY be any valid string according to
the referenced connection code standards.

***ProcessSpindleSpeed* attributes:**

| Attribute | Description                                               | Occurrence |
| --------- | --------------------------------------------------------- | ---------- |
| maximum   | The upper bound for the tool’s process target feed rate   | 0..1       |
| minimum   | The lower bound for the tools feed rate.                  | 0..1       |
| nominal   | The nominal feed rate the tool is designed to operate at. | 0..1       |

## Measurements

The *Measurements* element is a collection of one or more constrained
scalar values associated with this cutting tool. The contents MUST be a
subtype of *CommonMeasurement* or *AssemblyMeasurement*. The following
section will define the abstract *Measurement* type used in both
*CuttingToolLifeCycle* and *CuttingItem*. This section will then
describe the *AssemblyMeasurement* types. The *CuttingItemMeasurement*
types will be described at the end of the *CuttingItem* section.

A measurement is specific to a process and a machine tool at a
particular shop. The tool zero reference point or gauge line will be
different depending on the particular implementation and will be assumed
to be consistent within the shop. MTConnect does not standardize the
manufacturing process or the definition of the zero point.

[center|500px|thumb|Figure 17:
Measurement](/File:Measurement.PNG "wikilink")

A measurement MUST be a scalar floating point value that MAY be
constrained to a maximum and minimum value. Since the
*CuttingToolLifeCycle*’s main responsibility is to track aspects of the
tool that change over it’s use in the shop, MTConnect represents the
current value of the measurement MUST be in the
*[CDATA](/Terminology "wikilink")* (text between the start and end
element) as the most current valid value.

The minimum and maximum MAY be supplied if they are known or relevant to
the measurement. A nominal value MAY be provided to show the reference
value for this measurement.

There are three subtypes of *Measurement*: *CommonMeasurement*,
*AssemblyMeasurement*, and *CuttingItemMeasurement*. These abstract
types MUST NOT appear in an *MTConnectAssets* document, but are used in
the schema as a way to separate which measurements MAY appear in the
different sections of the document. Only subtypes that have extended
these types MAY appear in the *MTConnectAssets*
[XML](/Terminology "wikilink").

Measurements in the *CuttingToolLifeCycle* section MUST refer to the
entire assembly and not to an individual cutting item. Cutting item
measurements MUST be located in the measurements associated with the
individual Cutting Item.

Measurements MAY provide an optional *units*
[attribute](/Terminology "wikilink") to reinforce the given units. The
units MUST always be given in the predefined MTConnect units. If *units*
are provided, they are only for documentation purposes. *nativeUnits*
MAY optionally be provided to indicate the original units provided for
the measurements.

***Measurements* attributes:**

| Attribute         | Description                                                                                                                                                         | Occurrence |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| code              | A shop specific code for this measurement. ISO 13399 codes MAY be used to for these codes as well.                                                                  | 0..1       |
| maximum           | The maximum value for this measurement. Exceeding this value would indicate the tool is not usable.                                                                 | 0..1       |
| minimum           | The minimum value for this measurement. Exceeding this value would indicate the tool is not usable.                                                                 | 0..1       |
| nominal           | The as advertised value for this measurement.                                                                                                                       | 0..1       |
| significantDigits | The number of significant digits in the reported value. This is used by applications to determine accuracy of values. This MAY be specified for all numeric values. | 0..1       |
| units             | The units for the measurements. MTConnect defines all the units for each measurement, so this is mainly for documentation sake.                                     | 0..1       |
| nativeUnits       | The units the measurement was originally recorded in. This is only necessary if they differ from units.                                                             | 0..1       |

### CuttingToolMeasurement subtypes

These measurements are specific to the entire assembly and MUST NOT be
used for the measurement pertaining to a *CuttingItem*. The following
diagram will be used to for reference for the assembly specific
measurements.

The Code in the following table will refer to the acronyms in the
diagrams. We will be referring to many diagrams to disambiguate all
measurements of the *CuttingTool* and *CuttingItem*.

[left|500px|thumb|Figure 18: Cutting Tool Measurement Diagram
1](/File:CTMeasurementDiagram.PNG "wikilink") [center|500px|thumb|Figure
19: Cutting Tool Measurement Diagram
2](/File:CTMeasurementDiagram2.PNG "wikilink")

| Measurement        | Code   | Description                                                                                                                                                                                                                                                                                                                                                                                                   | Units |
| ------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----- |
| BodyDiameterMax    | `BDX`  | The largest diameter of the body of a tool item.                                                                                                                                                                                                                                                                                                                                                              | mm    |
| BodyLengthMax      | `LBX`  | The distance measured along the X axis from that point of the item closest to the workpiece, including the cutting item for a tool item but excluding a protruding locking mechanism for an adaptive item, to either the front of the flange on a flanged body or the beginning of the connection interface feature on the machine side for cylindrical or prismatic shanks.                                  | mm    |
| DepthOfCutMax      | `APMX` | The maximum engagement of the cutting edge or edges with the workpiece measured perpendicular to the feed motion.                                                                                                                                                                                                                                                                                             | mm    |
| CuttingDiameterMax | `DC`   | The maximum diameter of a circle on which the defined point Pk of each of the master inserts is located on a tool item. The normal of the machined peripheral surface points towards the axis of the cutting tool.                                                                                                                                                                                            | mm    |
| FlangeDiameterMax  | `DF`   | The dimension between two parallel tangents on the outside edge of a flange.                                                                                                                                                                                                                                                                                                                                  | mm    |
| OverallToolLength  | `OAL`  | The largest length dimension of the cutting tool including the master insert where applicable.                                                                                                                                                                                                                                                                                                                | mm    |
| ShankDiameter      | `DMM`  | The dimension of the diameter of a cylindrical portion of a tool item or an adaptive item that can participate in a connection.                                                                                                                                                                                                                                                                               | mm    |
| ShankHeight        | `H`    | The dimension of the height of the shank.                                                                                                                                                                                                                                                                                                                                                                     | mm    |
| ShankLength        | `LS`   | The dimension of the length of the shank.                                                                                                                                                                                                                                                                                                                                                                     | mm    |
| UsableLengthMax    | `LUX`  | maximum length of a cutting tool that can be used in a particular cutting operation including the non-cutting portions of the tool.                                                                                                                                                                                                                                                                           | mm    |
| ProtrudingLength   | `LPR`  | The dimension from the yz-plane to the furthest point of the tool item or adaptive item measured in the -X direction.                                                                                                                                                                                                                                                                                         | mm    |
| Weight             | `WT`   | The total weight of the cutting tool in grams. The force exerted by the mass of the cutting tool.                                                                                                                                                                                                                                                                                                             | grams |
| FunctionalLength   | `LF`   | The distance from the gauge plane or from the end of the shank to the furthest point on the tool, if a gauge plane does not exist, to the cutting reference point determined by the main function of the tool. The CuttingTool functional length will be the length of the entire tool, not a single cutting item. Each CuttingItem can have an independent FunctionalLength represented in its measurements. | mm    |