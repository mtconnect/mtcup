---
title: Streams
description: 
published: true
date: 2021-09-25T02:21:12.596Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:32:40.511Z
---

The MTConnect *[Agent](/Terminology "wikilink")* collects data from
various sources and delivers it to
[applications](/Terminology "wikilink") in response to
*[Sample](/Terminology "wikilink")* or
*[Current](/Terminology "wikilink")* requests. (See
[Protocol](/Protocol "wikilink") for more information) All the data is
collected into [streams](/Terminology "wikilink") and organized by
[device](/Terminology "wikilink") and then by
[component](/Terminology "wikilink").

Every MTConnect® response MUST contain a header as the first
[XML](/Terminology "wikilink") element below the root element of any
MTConnect® [XML](/Terminology "wikilink") Document sent back to an
[application](/Terminology "wikilink").

![StreamsHeaderSchema.PNG](/images/StreamsHeaderSchema.PNG)

## Streams Structure

A *[Streams](/Terminology "wikilink")* [XML](/Terminology "wikilink")
element is the high level container for all
[device](/Terminology "wikilink") [streams](/Terminology "wikilink").
Its function is to contain *DeviceStream* sub-elements. There MUST be no
[attributes](/Terminology "wikilink") or other type
[XML](/Terminology "wikilink") elements within the
*[Streams](/Terminology "wikilink")* element.

![StreamsSchemaDiagram.PNG](/images/StreamsSchemaDiagram.PNG)

| Elements     | Description                                                   | Occurrence |
| ------------ | ------------------------------------------------------------- | ---------- |
| DeviceStream | The stream of Samples, Events, and Condition for each device. | 1..INF     |

*[Streams](/Terminology "wikilink")* MUST have at least one
*DeviceStream* and the *DeviceStream* MAY have one or more
*ComponentStream* elements, depending on whether there are
[events](/Terminology "wikilink") or [samples](/Terminology "wikilink")
available for the [component](/Terminology "wikilink"). If there are no
*ComponentStream* elements, then no data will be delivered for this
request.

The following diagram illustrates the structure of the
*[Streams](/Terminology "wikilink")* with some
*[Samples](/Terminology "wikilink")*,
*[Events](/Terminology "wikilink")*, and *Condition* at the lowest
level:

![StreamsExampleStructure.PNG](/images/StreamsExampleStructure.PNG)

Below is an example [XML](/Terminology "wikilink") Document response for
an *[Agent](/Terminology "wikilink")* with two
[devices](/Terminology "wikilink"), mill-1 and mill-2. The data is
reported in two separate [device](/Terminology "wikilink")
[streams](/Terminology "wikilink").

``` xml

<MTConnectStreams …>
  <Header … />
  <Streams>
    <DeviceStream name="mill-1" uuid="1">
      <ComponentStream component="Device" name="mill-1" componentId="d1">
        <Events>
          <Availability dataItemId="avail1" name=="avail" sequence="5"
              timestamp="2010-04-06T06:19:35.153141">AVAILABLE</Availability>
        </Events>
      </ComponentStream>
    </DeviceStream>
    <DeviceStream name="mill-2" uuid="2">
      <ComponentStream component="Device" name="mill-2" componentId="d2">
        <Events>
          <Availability dataItemId="avail2" name="avail" sequence="15"
              timestamp="2010-04-06T06:19:35.153141">AVAILABLE</Availability>
        </Events>
      </ComponentStream>
    </DeviceStream>
  </Streams>
</MTConnectStreams>
```

The sequence numbers are unique across the two
[devices](/Terminology "wikilink") in the example above. The
[applications](/Terminology "wikilink") MUST NOT assume that the
[event](/Terminology "wikilink") and [sample](/Terminology "wikilink")
sequence numbers are strictly in sequence. All sequence numbers MAY NOT
be included. An example of this case would occur when a Path argument is
provided and all the *[Samples](/Terminology "wikilink")*,
*[Events](/Terminology "wikilink")*, and *Condition* are not selected or
when the *[Agent](/Terminology "wikilink")* is supporting more than one
[device](/Terminology "wikilink") and data from only one
[device](/Terminology "wikilink") is requested. Refer to the
[Protocol](/Protocol "wikilink") section for more information.

## DeviceStream

A *DeviceStream* is created to hold the
[device](/Terminology "wikilink")-specific information so it does not
need to be repeated for every [event](/Terminology "wikilink") and
[sample](/Terminology "wikilink"). This is done to reduce the size of
each [event](/Terminology "wikilink") and
[sample](/Terminology "wikilink") so they only carry the information
that is being reported. A *DeviceStream* MAY contain one or more
*ComponentStream* elements. If the request is valid and there are no
[events](/Terminology "wikilink") or [samples](/Terminology "wikilink")
that match the criteria, an empty *DeviceStream* element MUST be created
to indicate that the [device](/Terminology "wikilink") exists, but there
was no data available.

![DeviceStreamSchema.PNG](/images/DeviceStreamSchema.PNG)

***DeviceStream* Attributes:**

| Attributes | Description                             | Occurrence |
| ---------- | --------------------------------------- | ---------- |
| name       | The device’s name. An NMTOKEN XML type. | 1          |
| uuid       | The device’s unique identifier          | 1          |

***DeviceStream* Elements:**

| Element         | Description                                         | Occurrence |
| --------------- | --------------------------------------------------- | ---------- |
| ComponentStream | One component’s stream for each component with data | 0..INF     |

## ComponentStream

![ComponentStreamSchema.PNG](/images/ComponentStreamSchema.PNG)

A *ComponentStream* is similar to the *DeviceStream*. It contains the
information specific to the [component](/Terminology "wikilink") within
the *[Device](/Terminology "wikilink")*. The
[uuid](/Terminology "wikilink") only needs to be specified if the
*[Component](/Terminology "wikilink")* has a
[uuid](/Terminology "wikilink") assigned.

***ComponentStream* Attributes:**

| Attribute   | Description                                                                                                         | Occurrence |
| ----------- | ------------------------------------------------------------------------------------------------------------------- | ---------- |
| name        | This component’s name within the device. An NMTOKEN XML type.                                                       | 1          |
| nativeName  | The name the device manufacturer assigned to the component. If the native name is not provided it MUST be the name. | 0..1       |
| component   | The XLM element name for the component                                                                              | 1          |
| uuid        | The component’s unique identifier                                                                                   | 0..1       |
| componentId | Corresponds to the id attribute of the component in the probe request (Refer to Probe in Part 1).                   | 1          |

The [XML](/Terminology "wikilink") elements of the *ComponentStream*
classify the data into *[Events](/Terminology "wikilink")*,
*[Samples](/Terminology "wikilink")*, and *Condition*. The
*ComponentStream* MUST NOT be empty. It MUST include an
*[Events](/Terminology "wikilink")* and/or a
*[Samples](/Terminology "wikilink")* [XML](/Terminology "wikilink")
element.

***ComponentStream* Elements:**

| Element   | Description                          | Occurrence |
| --------- | ------------------------------------ | ---------- |
| Events    | The events for this component stream | 0..1       |
| Samples   | The samples for this component       | 0..1       |
| Condition | The condition of the device.         | 0..1       |

## Types and Subtypes of Data Items

What follows is the association between the various types and subtypes
of [data items](/Terminology "wikilink"). Each [data
item](/Terminology "wikilink") type MUST be translated into a
*[Sample](/Terminology "wikilink")*, *[Event](/Terminology "wikilink")*,
or *Condition* with the following rules:

:\* The type name will be all in capitals with an underscore (_)
between words.

:\* The [XML](/Terminology "wikilink") element of the
[event](/Terminology "wikilink") or [sample](/Terminology "wikilink")
will be the transformation of the [data item](/Terminology "wikilink")
type by capitalizing the first character of each word and then removing
the underscore. For example, the [data item](/Terminology "wikilink")
type *DOOR_STATE* is *DoorState*, *POSITION* is *Position*, and
*ROTARY_VELOCITY* is *RotaryVelocity*.

:\* The font used for the type name and the
[XML](/Terminology "wikilink") element MUST be *Courier New*.

The following example shows the transformation between the
*[DataItem](/Terminology "wikilink")* name as returned in a
*[Probe](/Terminology "wikilink")* request and the corresponding
structured data returned in a *[Stream](/Terminology "wikilink")*
[XML](/Terminology "wikilink") element returned from a
*[Current](/Terminology "wikilink")* or
*[Sample](/Terminology "wikilink")* request. In the
*[Probe](/Terminology "wikilink")* request, each
*[DataItem](/Terminology "wikilink")* defines its
*[DataItem](/Terminology "wikilink")* type, category, and (if
applicable) the *subType*.

The *[probe](/Terminology "wikilink")* request will return the response
below:

``` xml

<Path name="path" id="p1">
   <DataItems>
      <DataItem type="PATH_POSITION" category="EVENT" id="p2" subType="ACTUAL" name="Zact"/>
      <DataItem type="CONTROLLER_MODE" category="EVENT" id="p3" name="mode" />
      <DataItem type="PROGRAM" category="EVENT" id="p4" name="program" />
      <DataItem type="EXECUTION" category="EVENT" id="p5" name="execution" />
      <DataItem type="BLOCK" category="EVENT" id="p6" name="block" />
   </DataItems>
</Path>
```

The transformation from the *[Probe](/Terminology "wikilink")*to the
*[Current](/Terminology "wikilink")* or
*[Sample](/Terminology "wikilink")* will occur per the example below.
This example also illustrates how the *subType* is placed in the
*ComponentStream*. In the *[Current](/Terminology "wikilink")* and
*[Sample](/Terminology "wikilink")* request, [data
items](/Terminology "wikilink") will be returned in the
*ComponentStream* grouped into their respective categories. Also note
how the *CONTROLLER_MODE* was changed to *ControllerMode* in the
[current](/Terminology "wikilink") request below.

``` xml

<ComponentStream componentId="p1" component="Path"
    name="path">
  <Events>
     <PathPosition dataItemId="p2" timestamp="2009-03-04T19:45:50.458305" subType="ACTUAL" name="Zact" sequence="150651130">7.02</PathPosition>
     <Block dataItemId="p6" timestamp="2009-03-04T19:45:50.458305" name="block" sequence="150651134">x0.371524 y-0.483808</Block>
     <ControllerMode dataItemId="p3" timestamp="2009-02-26T02:02:35.716224" name="mode" sequence="182">AUTOMATIC</ControllerMode>
  </Events>
</ComponentStream>
```