---
title: Cutting Items
permalink: /Cutting_Items/
---

[center|500px|thumb|Figure 1: Cutting
Items](/File:CuttingItems.PNG "wikilink")

An optional collection of cutting items that SHOULD be provided for each
independent edge or insert. If the CuttingItems are not present; it
indicates there is no specific information with respect to each of the
cutting items. This does not imply there are no cutting items – there
MUST be at least one cutting item – but there is no specific
information.

***CuttingItems* attributes:**

| Attribute | Description                  | Occurrence |
| --------- | ---------------------------- | ---------- |
| count     | The number of cutting items. | 1          |

A cutting item is the portion of the tool that physically removes the
material from the workpiece by shear deformation. The cutting item can
be either a single piece of material attached to the tool item or it can
be one or more separate pieces of material attached to the tool item
using a permanent or removable attachment. A cutting item can be
comprised of one or more cutting edges. Cutting items include:
replaceable inserts, brazed tips and the cutting portions of solid
cutting tools.

MTConnect considers Cutting Items as part of the Cutting Tool. A Cutting
Item MUST NOT exist in MTConnect unless it is attached to a cutting
tool. Some of the measurements, such as *FunctionalLength*, MUST be made
with reference to the entire cutting tool to be meaningful.

[center|500px|thumb|Figure 2: Cutting
Item](/File:CuttingItem.PNG "wikilink")

### CuttingItem attributes

| Attribute     | Description                                                                          | Occurrence |
| ------------- | ------------------------------------------------------------------------------------ | ---------- |
| indices       | The number or numbers representing the individual cutting item or items on the tool. | 1          |
| itemId        | The manufacturer identifier of this cutting item                                     | 0..1       |
| manufacturers | The manufacturers of the cutting item                                                | 0..1       |
| grade         | The material composition for this cutting item                                       | 0..1       |

**indicies**

An identifier that indicates the cutting item or items these data are
associated with. The value MUST be single numbers (“1”) or a comma
separated set of individual elements ("1,2,3,4"), or as a inclusive
range of values as in ("1-10") or any combination of ranges and numbers
as in "1-4,6-10,22". There MUST NOT be spaces or non-integer values in
the text representation.

Indices SHOULD start numbering with the inserts or cutting items
furthest from the gauge line and increasing in value as the items get
closer to the gauge line. Items at the same distance MAY be arbitrarily
numbered.

**itemID**

The manufactures’ identifier for this cutting item that MAY be the its
catalog or reference number. The value MUST be an [XML
NMTOKEN](/Terminology "wikilink") value of numbers and letters.

**manufacturers**

This optional element references the manufacturers of this tool. At this
level the manufacturers will reference the Cutting Item specifically.
The representation will be a comma (,) delimited list of manufacturer
names. This can be any series of numbers and letters as defined by the
[XML](/Terminology "wikilink") type *string*.

**grade**

This provides an implementation specific designation for the material
composition of this cutting item.

### CuttingItem elements

| Element      | Description                                                  | Occurrence |
| ------------ | ------------------------------------------------------------ | ---------- |
| Description  | A free-form description of the cutting item.                 | 0..1       |
| Locus        | A free form description of the location on the cutting tool. | 0..1       |
| ItemLife     | The life of this cutting item.                               | 0..3       |
| Measurements | A collection of measurements relating to this cutting item.  | 0..1       |

**Description**

An optional free form text description of this cutting item.

**Locus**

Locus represents the location of the cutting item with respect to the
cutting tool. For clarity, the words `FLUTE`, `INSERT`, and `CARTRIDGE`
SHOULD be used to assist in noting the location of a cutting item. The
Locus MAY be any free form text, but SHOULD adhere to the following
rules:

  -
    1\. The location numbering SHOULD start at the furthest cutting item
    (\#1) and work it’s way back to the cutting item closest to the
    gauge line.

<!-- end list -->

  -
    2\. Flutes SHOULD be identified as such using the word `FLUTE`:. For
    example:

`FLUTE`: 1, `INSERT`: 2 - would indicate the first flute and the second
furthest insert from the end of the tool on that flute.

  -
    3\. Other designations such as `CARTRIDGE` MAY be included, but
    should be identified using upper case and followed by a colon (:).

## ItemLife

[center|500px|thumb|Figure 3: Item Life](/File:ItemLife.PNG "wikilink")

The value is the current value for the tool life. The value MUST be a
number. Tool life is an option element which can have three types,
either minutes for time based, part count for parts based, or wear based
using a distance measure. One tool life can appear for each type, but
there cannot be two entries of the same type. Additional types can be
added in the future.

### ItemLife attributes

These are optional attributes that can be used to further classify the
operation type.

| Attribute      | Description                                                                                                                                                                                                                         | Occurrence |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| type           | The type of tool life being accumulated. `MINUTES`, `PART_COUNT`, or `WEAR`                                                                                                                                                         | 1          |
| countDirection | Indicates if the tool life counts from zero to maximum or maximum to zero, The values MUST be one of `UP` or `DOWN`.                                                                                                                | 1          |
| warning        | The point at which a tool life warning will be raised.                                                                                                                                                                              | 0..1       |
| limit          | The end of life limit for this tool. If the countDirection is `DOWN`, the point at which this tool should be expired, usually zero. If the *countDirection* is `UP`, this is the upper limit for which this tool should be expired. | 0..1       |
| initial        | The initial life of the tool when it is new.                                                                                                                                                                                        | 0..1       |

### ItemLife *type* attribute

The value of *type* must be one of the following:

| Value        | Description                                                                                                                                                                                      |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `MINUTES`    | The tool life measured in minutes. All units for minimum, maximum, and warningLevel MUST be provided in minutes.                                                                                 |
| `PART_COUNT` | The tool life measured in parts. All units for minimum, maximum, and warningLevel MUST be provided supplied as the number of parts.                                                              |
| `WEAR`       | The tool life measured in tool wear. Wear MUST be provided in millimeters as an offset to nominal. All units for minimum, maximum, and warningLevel MUST be given as millimeter offsets as well. |

### ItemLife *direction* attribute

The value of direction must be one of the following:

| Value  | Description                                         |
| ------ | --------------------------------------------------- |
| `DOWN` | The tool life counts down from the maximum to zero. |
| `UP`   | The tool life counts up from zero to the maximum.   |

### *CuttingItemMeasurement* subtypes

These measurements are specific to an individual cutting item and MUST
NOT be used for the measurement pertaining to an assembly. The following
diagram will be used to for reference for the cutting item specific
measurements.

The Code in the following table will refer to the acronym in the
diagram. We will be referring to many diagrams to disambiguate all
measurements of the cutting tools and items.

[left|500px|thumb|Figure 4: Cutting
Tool](/File:CuttingTool.PNG "wikilink") [center|500px|thumb|Figure 5:
Cutting Item](/File:CuttingItem2.PNG "wikilink")
[left|500px|thumb|Figure 6: Cutting Item Measurement
Diagram](/File:CuttingItemMeasurementDiagram.PNG "wikilink")
[center|500px|thumb|Figure 7: Cutting Item Drive
Angle](/File:CuttingItemDriveAngle.PNG "wikilink")









The following *CuttingItemMeasurements* will refer to the diagrams
above:

| Measurement            | Code   | Description                                                                                                                                                                                                                                                                                             | Units  |
| ---------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| CuttingReferncePoint   | `CRP`  | The theoretical sharp point of the cutting tool from which the major functional dimensions are taken.                                                                                                                                                                                                   | mm     |
| CuttingEdgeLength      | `L`    | The theoretical length of the cutting edge of a cutting item over sharp corners.                                                                                                                                                                                                                        | mm     |
| DriveAngle             | `DRVA` | Angle between the driving mechanism locator on a tool item and the main cutting edge                                                                                                                                                                                                                    | degree |
| FlangeDiameter         | `DF`   | The dimension between two parallel tangents on the outside edge of a flange.                                                                                                                                                                                                                            | mm     |
| FunctionalWidth        | `WF`   | The distance between the cutting reference point and the rear backing surface of a turning tool or the axis of a boring bar.                                                                                                                                                                            | mm     |
| IncribedCircleDiameter | `IC`   | The diameter of a circle to which all edges of a equilateral and round regular insert are tangential.                                                                                                                                                                                                   | mm     |
| PointAngle             | `SIG`  | The angle between the major cutting edge and the same cutting edge rotated by 180 degrees about the tool axis.                                                                                                                                                                                          | degree |
| ToolCuttingEdgeAngle   | `KAPR` | The angle between the tool cutting edge plane and the tool feed plane measured in a plane parallel the xy-plane.                                                                                                                                                                                        | degree |
| ToolLeadAngle          | `PSIR` | The angle between the tool cutting edge plane and a plane perpendicular to the tool feed plane measured in a plane parallel the xy-plane.                                                                                                                                                               | degree |
| ToolOrientation        | `N/A`  | The angle of the tool with respect to the workpiece for a given process. The value is application specific.                                                                                                                                                                                             | degree |
| WiperEdgeLength        | `BS`   | The measure of the length of a wiper edge of a cutting item.                                                                                                                                                                                                                                            | mm     |
| StepDiameterLength     | `SDLx` | The length of a portion of a stepped tool that is related to a corresponding cutting diameter measured from the cutting reference point of that cutting diameter to the point on the next cutting edge at which the diameter starts to change.                                                          | mm     |
| StepIncludedAngle      | `STAx` | The angle between a major edge on a step of a stepped tool and the same cutting edge rotated 180 degrees about its tool axis.                                                                                                                                                                           | degree |
| CuttingDiameter        | `DCx`  | The nominal radius of a rounded corner measured in the XY-plane.                                                                                                                                                                                                                                        | mm     |
| CuttingHeight          | `HF`   | The distance from the basal plane of the tool item to the cutting point.                                                                                                                                                                                                                                | mm     |
| CornerRadius           | `RE`   | The nominal radius of a rounded corner measured in the X Y-plane.                                                                                                                                                                                                                                       | mm     |
| Weight                 | `WT`   | The total weight of the cutting tool in grams. The force exerted by the mass of the cutting tool.                                                                                                                                                                                                       | grams  |
| FunctionalLength       | `LFx`  | The distance from the gauge plane or from the end of the shank of the cutting tool, if a gauge plane does not exist, to the cutting reference point determined by the main function of the tool. This measurement will be with reference to the Cutting Tool and MUST NOT exist without a cutting tool. | mm     |
| ChamferFlatLength      | `BCH`  | The flat length of a chamfer.                                                                                                                                                                                                                                                                           | mm     |
| ChamferWidth           | `CHW`  | The width of the chamfer.                                                                                                                                                                                                                                                                               | mm     |
| InsertWidth            | `W1`   | `W1` is used for the insert width when an inscribed circle diameter is not practical.                                                                                                                                                                                                                   | mm     |