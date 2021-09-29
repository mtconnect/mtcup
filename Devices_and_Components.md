---
title: Devices and Components
description: 
published: true
date: 2021-09-25T02:02:51.558Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:31:13.006Z
---

MTConnect organizes information and data from a data source (typically a
machine) into an information model that defines the relationship between
each piece of data and the source of that data. This information model
allows an [application](/Terminology "wikilink") to interpret the data
received from a data source and correlate that data to its original
definition, value, and context.

The basic MTConnect information model contains three primary containers:
*[Devices](/Terminology "wikilink")*,
*[Device](/Terminology "wikilink")*, and
*[Components](/Terminology "wikilink")*. These containers are the
building blocks used to organize information about a piece of equipment.
They also define how the various parts of a piece of equipment relate to
each other.

![ContainerStructure.PNG](/images/ContainerStructure.PNG)

The first, or highest, level container in the MTConnect data structure
is the *[Devices](/Terminology "wikilink")* container. The
*[Devices](/Terminology "wikilink")* container is comprised of one or
more *[Device](/Terminology "wikilink")* [XML](/Terminology "wikilink")
Element(s). The *[Devices](/Terminology "wikilink")* container provides
a mechanism for grouping data from multiple
*[Device](/Terminology "wikilink")* elements that are providing their
data through a common MTConnect *[Agent](/Terminology "wikilink")*.
*[Devices](/Terminology "wikilink")* has no
[attributes](/Terminology "wikilink") and is only used to group data
from *[Device](/Terminology "wikilink")* elements together.

The next level container is *[Device](/Terminology "wikilink")*. A
*[Device](/Terminology "wikilink")* typically represents a single piece
of equipment or a machine. However, it can also represent any logical
grouping of [components](/Terminology "wikilink") that operate together
to perform a function. Every *[Device](/Terminology "wikilink")* in
MTConnect® MUST have an *Availability* [data
item](/Terminology "wikilink"). *Availability* represents the device’s
ability to provide information about itself. The
[Device](/Terminology "wikilink") container is compromised of one or
more [Components](/Terminology "wikilink")
[XML](/Terminology "wikilink") Elements.

The third container in the MTConnect Data Structure is the
*[Components](/Terminology "wikilink")* container(s).
*[Components](/Terminology "wikilink")* provides a mechanism for
grouping sub-elements of a *[Device](/Terminology "wikilink")* into
logical groups that are associated with each other.
*[Components](/Terminology "wikilink")* has no
[attributes](/Terminology "wikilink") and is only used to group
*[Component](/Terminology "wikilink")* elements together. The
*[Components](/Terminology "wikilink")* container is compromised of one
or more *[Component](/Terminology "wikilink")*
[XML](/Terminology "wikilink") Elements.

## Devices

A *[Device](/Terminology "wikilink")* is a
[XML](/Terminology "wikilink") Element that holds all the
*[Components](/Terminology "wikilink")* associated with a piece of
equipment. This can be a logical grouping of
*[Component](/Terminology "wikilink")* [XML](/Terminology "wikilink")
Elements that perform a particular function. The
*[Device](/Terminology "wikilink")* MUST have an *Availability* [data
item](/Terminology "wikilink") that indicates if this
*[device](/Terminology "wikilink")* is available to provide information.

In the MTConnect® schema, a *[Device](/Terminology "wikilink")* is
actually a unique type of '['Component](/Terminology "wikilink")''. A
*[Device](/Terminology "wikilink")* supports all of the functions and
capabilities defined for a *[Component](/Terminology "wikilink")*.
However, it MUST be uniquely identified throughout the MTConnect®
Standard and schema as a *[Device](/Terminology "wikilink")* to clearly
define the difference between a logical collection of
*[components](/Terminology "wikilink")* that function together as a
*[Device](/Terminology "wikilink")* and the identification of each
*[Component](/Terminology "wikilink")* that forms the structure within a
*[Device](/Terminology "wikilink")*.

![DeviceSchema.PNG](/images/DeviceSchema.PNG)

Note: Some *[components](/Terminology "wikilink")* may not be integral
to a parent *[device](/Terminology "wikilink")* or another
*[component](/Terminology "wikilink")*. These
*[components](/Terminology "wikilink")* may function independently or
produce data that is not relevant to a parent
*[device](/Terminology "wikilink")*. An example would be a temperature
sensor installed in a plant to monitor the ambient air temperature. In
this case, the *[Component](/Terminology "wikilink")* MAY be modeled in
the MTConnect schema as a *[Device](/Terminology "wikilink")*. When
modeled as a *[Device](/Terminology "wikilink")*, the
*[component](/Terminology "wikilink")* MUST provide all of the data and
capabilities defined for a *[Device](/Terminology "wikilink")*. It is
also possible for these *[components](/Terminology "wikilink")* to be
defined as a *[Component](/Terminology "wikilink")* of a parent
*[device](/Terminology "wikilink")* and simultaneously as an independent
*[Device](/Terminology "wikilink")*; communicating data associated with
the parent *[Device](/Terminology "wikilink")* incorporated into that
*[device](/Terminology "wikilink")*’s data
[stream](/Terminology "wikilink") and independently communicating
additional data in a separate data [stream](/Terminology "wikilink")
using its own uuid.

**Device Attributes**:

| Attribute      | Description                                                                                                                                                                                                                                                                                                                                                                                          | Occurrence |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| iso841Class    | DEPRECATED in Release 1.1.0                                                                                                                                                                                                                                                                                                                                                                          | \-         |
| uuid           | A unique identifier that will only refer to this Device. For example, this may be the manufacturer’s code and the serial number. The uuid should be alphanumeric and not exceeding 255 characters. An NMTOKEN XML type.                                                                                                                                                                              | 0..1\*     |
| name           | The name of the Device. This name should be unique within the machine to allow for easier data integration. An NMTOKEN XML type.                                                                                                                                                                                                                                                                     | 1          |
| nativeName     | The name the device manufacturer assigned to this Device. If the native name is not provided, it MUST be the name.                                                                                                                                                                                                                                                                                   | 0..1       |
| id             | The unique identifier for this Device in the document. An id MUST be unique across all the id attributes in the document. An XML ID-type.                                                                                                                                                                                                                                                            | 1          |
| sampleRate     | DEPRECATED IN REL. 1.2 (REPLACED BY sampleInterval)                                                                                                                                                                                                                                                                                                                                                  | \-         |
| sampleInterval | The interval in milliseconds between the completion of the reading of one sample of data from a device until the beginning of the next sampling of that data. This is the number of milliseconds between data captures. If the sample interval is smaller than one millisecond, the number can be represented as a floating point number. For example, an interval of 100 microseconds would be 0.1. | 0..1\*\*   |

Notes:

  -
    \*The uuid MUST be provided for the
    *[Device](/Terminology "wikilink")*. It is optional for all other
    *[Component](/Terminology "wikilink")* types.
    \*\*The sampleInterval is used to aid an
    [application](/Terminology "wikilink") in interpolating values. This
    is the desired sample interval and may vary depending on the
    capabilities of the *[device](/Terminology "wikilink")*.

**Device Elements:**

| Element       | Description                                                                                                                            | Occurrence |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| Description   | An XML element that can contain any descriptive content. This can contain configuration information and manufacturer specific details. | 0..1       |
| Configuration | An XML element that can contain descriptive content defining the configuration information for a Device.                               | 0..1       |
| Components    | A container for lower level Component XML Elements associated with this Device.                                                        | 0..INF\*   |
| DataItems     | The data items (defined below) provided by this Device. The data items define the measured values to be reported by this Device.       | 0..INF\*   |

Note:

  -
    \*At least one of *[Components](/Terminology "wikilink")* or
    *[DataItems](/Terminology "wikilink")* MUST be provided.

## Components

A *[Component](/Terminology "wikilink")* [XML](/Terminology "wikilink")
Element defines physical or logical sub-element of a
*[device](/Terminology "wikilink")*.
*[Component](/Terminology "wikilink")* is an abstract type and will
never appear in the MTConnect [XML](/Terminology "wikilink") document.
*[Component](/Terminology "wikilink")* elements are represented as
[XML](/Terminology "wikilink") Element sub-types such as *Axes*,
*Controller*, *Door*, etc.

*[Component](/Terminology "wikilink")* elements contain information and
data defining the element’s operational state, the environment in which
it is functioning, and its health or status. This information and the
measured values associated with a *[component](/Terminology "wikilink")*
are defined as *[DataItems](/Terminology "wikilink")*. For more
information on [data items](/Terminology "wikilink") click here.

*[Component](/Terminology "wikilink")* can be further sub-divided into
smaller *[Components](/Terminology "wikilink")*
[XML](/Terminology "wikilink") Elements to provide additional detail on
the structure and configuration of a
*[Component](/Terminology "wikilink")*. These sub-elements have all the
characteristics and capabilities of the parent
*[component](/Terminology "wikilink")*.

![ComponentDiagram.PNG](/images/ComponentDiagram.PNG)

While these sub-elements are by definition
*[Components](/Terminology "wikilink")*, they SHALL be called
*subcomponents* within the MTConnect Standard to provide clarity on the
relationship between the parent *[component](/Terminology "wikilink")*
and its associated sub-elements (subcomponents). Additionally,
subcomponents may be further subdivided into additional
*[Components](/Terminology "wikilink")*, as required, to provide a
complete description of a *[device](/Terminology "wikilink")* and its
measured values (*[DataItems](/Terminology "wikilink")*).
*[Components](/Terminology "wikilink")* and related subcomponents are
represented in the [XML](/Terminology "wikilink") schema as follows:

``` xml

    <Devices>
        <Device>
             <Components>
                 <Axes(Component Type Subcomponent)>
                     <Components>
                        <Linear (Component Type Subcomponent) >
                             < Components>
                               <Etc.  >
```

![ComponentSchema.PNG](/images/ComponentSchema.PNG)

Every component has the following composition:

| Attribute      | Description                                                                                                                                                                                                                                                                                                                                                                                             | Occurrence |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| uuid           | A unique identifier that will only refer to this Component. For example, this can be the manufacturer’s code and the serial number. The uuid should be alphanumeric and not exceeding 255 characters. An NMTOKEN XML type.                                                                                                                                                                              | 0..1\*     |
| name           | The name of the Component. This name should be unique within the machine to allow for easier data integration. An NMTOKEN XML type.                                                                                                                                                                                                                                                                     | 1          |
| nativeName     | The name the device manufacturer assigned to the Component. If the native name is not provided it MUST be the name.                                                                                                                                                                                                                                                                                     | 0..1       |
| id             | The unique identifier for this Component in the document. An id MUST be unique across all the id attributes in the document. An XML ID-type.                                                                                                                                                                                                                                                            | 1          |
| sampleRate     | DEPRECATED IN REL. 1.2 (REPLACED BY sampleInterval)                                                                                                                                                                                                                                                                                                                                                     | \-         |
| sampleInterval | The interval in milliseconds between the completion of the reading of one sample of data from a component until the beginning of the next sampling of that data. This is the number of milliseconds between data captures. If the sample interval is smaller than one millisecond, the number can be represented as a floating point number. For example, an interval of 100 microseconds would be 0.1. | 0..1\*\*   |

Notes:

  -
    \*The uuid MUST be provided for the
    *[Device](/Terminology "wikilink")*. It is optional for all other
    *[Component](/Terminology "wikilink")* types.
    \*\*The sampleInterval is used to aid the
    [application](/Terminology "wikilink") in interpolating values. This
    is the desired [sample](/Terminology "wikilink") Interval and may
    vary depending on the capabilities of the
    *[component](/Terminology "wikilink")*.

**Component Elements:**

| Element       | Description                                                                                                                              | Occurrence |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| Description   | An element that can contain any descriptive content. This can contain information about the Component and manufacturer specific details. | 0..1       |
| Components    | A container for lower level Component XML Elements associated with this Component.                                                       | 0..INF\*   |
| Configuration | An element that can contain descriptive content defining the configuration information for a Component.                                  | 0..1       |
| DataItems     | The data items this component provides. The data items define the measured values to be reported by this Component.                      | 0..INF\*   |

Note:

  -
    \*At least one of *[Components](/Terminology "wikilink")* or
    *[DataItems](/Terminology "wikilink")* MUST be provided.

The [CDATA](/Terminology "wikilink") of *Description* is any additional
descriptive information the implementer chooses to include regarding the
*[Component](/Terminology "wikilink")*. An example of a *Description* is
as follows:

``` xml

 <Description manufacturer="Example Co" serialNumber="A124FFF"
   station="2"> Example Co Simulated Vertical 3 Axis Machining center.
 </Description>
```

The information can be provided for any
*[component](/Terminology "wikilink")*. For example, an electrical power
sensor can be defined as follows:

``` xml

<Description manufacturer="Example Co"
   serialNumber="EXCO-TT-099PP-XXXX"> Advanced Pulse watt-hour transducer
   with pulse output
</Description>
```