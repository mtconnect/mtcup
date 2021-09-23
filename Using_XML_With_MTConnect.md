---
title: Using XML With MTConnect
permalink: /Using_XML_With_MTConnect/
---

## This document provides a guideline and definitions for using XML in conjunction with the MTConnect Standard.

  - XML Document: A text based document generated from a data source in
    a specific format that can be interpreted by a software application.
    The basic structure of the document is defined by the XML
    specification and contains one or more XML Elements. The content of
    the document is defined by an XML Schema.

The content of the MTConnect XML document is specified in the MTConnect
standard and encoded in the XML schema. The MTConnect standard defines
four XML document types as follows: MTConnectDevices, MTConnectStreams,
MTConnectAssets, and MTConnectErrors.

  - XML Element: The basic building block of an XML Document. A logical
    expression of information to be communicated from a data source. The
    definition of each XML Element is specified in a XML Schema.

*Note:* An XML Element begins with a start-tag and ends with a matching
end-tag or consists only of an empty-element tag. The characters between
the start- and end-tags, if any, are the element's content, and may
contain markup, including other elements, which are called child
elements.

An element can contain:

  - other elements
  - text
  - [attributes](/Terminology "wikilink")
  - or a mix of all of the above

A sample of XML Elements could include:

<table>
<thead>
<tr class="header">
<th><p>Sample XML</p></th>
<th><p>MTConnect</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="sourceCode" id="cb1"><pre class="sourceCode numberSource xml numberLines"><code class="sourceCode xml"><a class="sourceLine" id="cb1-1" title="1"><span class="kw">&lt;root&gt;</span></a>
<a class="sourceLine" id="cb1-2" title="2">  <span class="kw">&lt;child&gt;</span></a>
<a class="sourceLine" id="cb1-3" title="3">    <span class="kw">&lt;subchild&gt;</span> .... <span class="kw">&lt;/subchild&gt;</span></a>
<a class="sourceLine" id="cb1-4" title="4">  <span class="kw">&lt;/child&gt;</span></a>
<a class="sourceLine" id="cb1-5" title="5"><span class="kw">&lt;/root&gt;</span></a></code></pre></div></td>
<td><div class="sourceCode" id="cb2"><pre class="sourceCode numberSource xml numberLines"><code class="sourceCode xml"><a class="sourceLine" id="cb2-1" title="1"><span class="kw">&lt;MTConnectDevices&gt;</span></a>
<a class="sourceLine" id="cb2-2" title="2">  <span class="kw">&lt;Header&gt;&lt;/Heaader&gt;</span></a>
<a class="sourceLine" id="cb2-3" title="3">  <span class="kw">&lt;Devices&gt;</span></a>
<a class="sourceLine" id="cb2-4" title="4">     <span class="kw">&lt;Device&gt;</span></a>
<a class="sourceLine" id="cb2-5" title="5">        <span class="kw">&lt;Axes&gt;</span></a>
<a class="sourceLine" id="cb2-6" title="6">           <span class="kw">&lt;Rotary&gt;</span>...<span class="kw">&lt;/Rotary&gt;</span></a>
<a class="sourceLine" id="cb2-7" title="7">        <span class="kw">&lt;/Axes&gt;</span></a>
<a class="sourceLine" id="cb2-8" title="8">     <span class="kw">&lt;Device&gt;</span></a>
<a class="sourceLine" id="cb2-9" title="9">  <span class="kw">&lt;/Devices&gt;</span></a>
<a class="sourceLine" id="cb2-10" title="10"><span class="kw">&lt;MTConnectDevices&gt;</span></a></code></pre></div></td>
</tr>
</tbody>
</table>

  - XML Sub-Element: A type of XML Element – also called a Child
    Element. A Sub-Element is a logical or physical portion of the
    parent Element and is used to segregate information specific to a
    defined portion of an XML Element. Sub-Elements are grouped into the
    parent element which in turn functions as a Container.

<!-- end list -->

  - XML Tree: A graphical representation of an XML Document. This
    graphical representation defines the XML Elements and the
    relationship between the elements based on the XML Schema.

The actual format of an XML Tree can vary depending on the tools used to
generate the tree.

Samples of XML Trees:

[<File:xmlspy_pict1.png>](/File:xmlspy_pict1.png "wikilink")
[<File:device_hierarchy.png>](/File:device_hierarchy.png "wikilink")

  - Container: An XML Element that groups related or homogeneous XML
    Elements. A Container typically contains Child Elements or
    Sub-Elements.

In MTConnect, Containers are used to group like types of information
relative to a *Device* or *Asset*.

By convention in the MTConnect Standard, a container will be named in
the following formats:

  - Container of XML Elements all of the same type will be defined using
    the element type name with an “s” (plural). The one exception is
    `Condition` in the streams document. Example: `Devices`, `Systems`,
    `Axes`, etc.
  - Container of related XML Elements that represent a logical grouping
    of information associated with a common reference will be defined
    using the element type name without an “s” (singular). Example:
    `Device`, `Controller`, `Asset`, etc.

<!-- end list -->

  - Abstract Element: An XML element that can be substituted for another
    element that is a sub-type of that element. An abstract element
    cannot appear in the document itself. For example, you cannot have a
    `Component` element in the `MTConnectDevices`, you can only have a a
    sub-type of `Component` like `Controller` or `Axes`.

<!-- end list -->

  - Container of Abstract Elements: XML provides a mechanism to force
    the substitution for a particular element or type of element. When
    an element or type of element is declared in the XML Schema to be
    “Abstract”, it cannot be used in the XML Document. When an element
    is declared to be abstract, a member of that element’s sub-group
    must appear in the XML Document.

A Virtual or Abstract Container is a Container is defined in the XML
Schema to provide structure and context. These type containers do not
occur in the XML Document - only the actual XML Elements that make up
the content of the container occur in the XML Document. For Example: if
you were constructing an XML Document that organizes the names of all
the National Football League (NFL) teams in the United States based on
the states in which they reside, their roster, etc, you would likely
start by defining the XML Elements of Country, State, and Teams in your
XML Schema – these are all Container type XML Elements.

``` xml numberLines
<Country name=”United States”>
  <State name=”New York”>
    <Teams>
    </Teams>
  </State>
</Country>
```

In this example, “Teams” is a Container for all teams. In the XML
Schema, the next logical XML Element to define would be “Team” so that
you can identify each individual team by their unique name. When
developing your XML Schema, there are at least two different ways you
mach choose to define a Container such as “Team”.

If “Team” is a large data set and/or a dynamic data set, you typically
will define “Team” as an XML Element with an Attribute called “name” so
that you can list all of the teams. It would look something like:

<table>
<tbody>
<tr class="odd">
<td><div class="sourceCode" id="cb1"><pre class="sourceCode numberSource xml numberLines"><code class="sourceCode xml"><a class="sourceLine" id="cb1-1" title="1"><span class="kw">&lt;Country</span><span class="ot"> name=</span><span class="er">”United</span> <span class="er">States”&gt;</span>                                 </a>
<a class="sourceLine" id="cb1-2" title="2">  <span class="er">&lt;State</span> <span class="er">name=”New</span> <span class="er">York”&gt;</span></a>
<a class="sourceLine" id="cb1-3" title="3">    <span class="er">&lt;Teams&gt;</span></a>
<a class="sourceLine" id="cb1-4" title="4">       <span class="er">&lt;Team</span> <span class="er">name=”Jets”&gt;</span></a>
<a class="sourceLine" id="cb1-5" title="5">         <span class="er">&lt;Roster/&gt;</span></a>
<a class="sourceLine" id="cb1-6" title="6">       <span class="er">&lt;/Team&gt;</span></a>
<a class="sourceLine" id="cb1-7" title="7">       <span class="er">&lt;Team</span> <span class="er">name=”Giants”&gt;</span></a>
<a class="sourceLine" id="cb1-8" title="8">         <span class="er">&lt;Roster/&gt;</span></a>
<a class="sourceLine" id="cb1-9" title="9">       <span class="er">&lt;/Team&gt;</span>                 </a>
<a class="sourceLine" id="cb1-10" title="10">     <span class="er">&lt;/Teams&gt;</span></a>
<a class="sourceLine" id="cb1-11" title="11">   <span class="er">&lt;/State&gt;</span></a>
<a class="sourceLine" id="cb1-12" title="12"><span class="er">&lt;/Country&gt;</span></a></code></pre></div></td>
<td><p><a href="/File:team_roster_hier1.png" title="wikilink">thumbnail|right</a></p></td>
</tr>
</tbody>
</table>

However, since the list of NFL Teams is a limited data set and is fairly
stable, you may also choose to use a Virtual Container and name each
team within your schema. When you do this, the Virtual Container “Team”
will be defined in the XML Schema and will be shown in the XML Tree, but
does not appear in the XML Document. It would look something like:

<table>
<tbody>
<tr class="odd">
<td><div class="sourceCode" id="cb1"><pre class="sourceCode numberSource xml numberLines"><code class="sourceCode xml"><a class="sourceLine" id="cb1-1" title="1"><span class="kw">&lt;Country</span><span class="ot"> name=</span><span class="er">”United</span> <span class="er">States”&gt;</span>                                 </a>
<a class="sourceLine" id="cb1-2" title="2">  <span class="er">&lt;State</span> <span class="er">name=”New</span> <span class="er">York”&gt;</span></a>
<a class="sourceLine" id="cb1-3" title="3">    <span class="er">&lt;Teams&gt;</span></a>
<a class="sourceLine" id="cb1-4" title="4">       <span class="er">&lt;Jets&gt;</span></a>
<a class="sourceLine" id="cb1-5" title="5">         <span class="er">&lt;Roster/&gt;</span></a>
<a class="sourceLine" id="cb1-6" title="6">       <span class="er">&lt;/Jets&gt;</span></a>
<a class="sourceLine" id="cb1-7" title="7">       <span class="er">&lt;Giants&gt;</span></a>
<a class="sourceLine" id="cb1-8" title="8">         <span class="er">&lt;Roster/&gt;</span></a>
<a class="sourceLine" id="cb1-9" title="9">       <span class="er">&lt;/Giants&gt;</span>                 </a>
<a class="sourceLine" id="cb1-10" title="10">     <span class="er">&lt;/Teams&gt;</span></a>
<a class="sourceLine" id="cb1-11" title="11">   <span class="er">&lt;/State&gt;</span></a>
<a class="sourceLine" id="cb1-12" title="12"><span class="er">&lt;/Country&gt;</span></a></code></pre></div></td>
<td><p><a href="/File:team_roster_hier2.png" title="wikilink">thumbnail|right</a></p></td>
</tr>
</tbody>
</table>

Note: The Virtual Container “Team” appears in the XML Schema and XML
Tree, but does not appear in the XML Document.

The MTConnect Standard uses Virtual Containers when specifying specific
sub-parts of a Device or Asset. Examples of Virtual Containers in the
MTConnect Standard are Component and xxxxxxxxxxxx.

  - Markup and Content: The characters making up an XML document are
    divided into markup and content, which may be distinguished by the
    application of simple syntactic rules. Generally, strings that
    constitute markup either begin with the character \< and end with a
    \>, or they begin with the character *&* and end with a *;*. Strings
    of characters that are not markup are content.

However, in a CDATA section, the delimiters are classified as markup,
while the text between them is classified as content. In addition,
whitespace before and after the outermost element is classified as
markup.

  - Tag: A markup construct that begins with \< and ends with \> and
    used to reference an instance of an XML element. Tags come in three
    flavors:

<!-- end list -->

  - *start-tags*; for example: <code>
    <section>
    </code>
  - *end-tags*; for example: <code>
    </section>
    </code>
  - *empty-element tags*; for example: <line-break />

<!-- end list -->

  - Attributes: Information about an XML Element that provides
    additional descriptive details to further define the element.

In MTConnect, Attributes are used to provide additional information
about XML Elements that are defined in the standard. Examples of
Attributes for a Device would be name, id, manufacturer, etc.

  - CDATA: CData means character data. It is information that may be
    included in an XML document that contains a text string of data
    associated with the XML Element. CData is not parsed by an XML
    parser.

One example would be a Message type XLM Element:

`   <Message ...>This is some text`</Message>

Another example would be the “value” provided for an XML Element such as
“Speed”

In MTConnect, CData is used to represent the values provided in the
Streams XML Document for Data Items: Position, Velocity, Voltage, etc.
It is also used to represent the textual content for Alarms, Messages,
etc.

## XML Terms Defined in the MTConnect Standard

  - Devices: An XML Container that represents a logical grouping of one
    or more Device XML Elements.

<!-- end list -->

  - Device: An XML Element defined in the MTConnect Standard to
    represent a piece of equipment capable of performing an operation. A
    Device may be composed of a set of components that provide data to a
    software application. The Device is a separate entity with at least
    one component or data item providing information about the device.

<!-- end list -->

  - MTConnectAssets: An XML Container that represents a logical grouping
    of one or more Asset XML Elements.

<!-- end list -->

  - Asset: An XML Element defined in the MTConnect Standard to represent
    something that is associated with the manufacturing process that is
    not a component of a device, can be removed without detriment to the
    function of the device, and can be associated with other devices
    during their lifecycle. An asset does not have computational
    capabilities, but may carry information in some media physically
    attached to the asset.

In MTConnect, examples of Assets would include Tooling, Parts, Fixtures,
etc. Stream: An XML Document type that contains the response provided by
an MTConnect Agent to a request for information from a client software
application. The XML Schema for Stream is defined in Part 3 of the
MTConnect Standard.

  - Components: An XML Container the represents all of the logical
    and/or physical portions of a device (piece of equipment).

<!-- end list -->

  - Component: A logical or physical portion of a device (piece of
    equipment) that can be described independently and has data and/or
    other descriptive information (attributes) defined within the XML
    Schema.

In MTConnect, Component is a Virtual or Abstract Type XML Element.
Therefore, Component will never appear in a XML document. Specific
Component types defined in the MTConnect Stanard will be substituted in
the XML for each occurrence of Component – ex. Controller, Door, Axes,
etc.

Components are organized as sub-elements of the Components container.

  - Sub-Component: A logical or physical part of a Component that can be
    described independently and has data and/or other descriptive
    information (attributes) defined within the XML Schema.

In MTConnect, Sub-Component is an XML Element type. Sub-Component itself
does not appear as an XML Element in the XML Schema. However, specific
sub-components are defined in the schema provided in the MTConnect
Standard and are represented in the XML Document.

Examples of Sub-Components of the Axes component are *Linear* and
*Rotary*.

Sub-Components are organized as sub-elements of the parent Component
container.

Sub-Components may also be further decomposed into additional levels of
Sub-Components.

  - DataItems: An XML Container the represents all of the data types
    associated with a XML Element. In MTConnect, DataItems can be
    associated with Device, Components, any Component Type, or a
    Sub-Component type, or Asset.

<!-- end list -->

  - DataItem: An XML Element that provides the descriptive information
    associated with a XML Element - Device, Components, any Component
    Type, or a Sub-Component type, or Asset. Examples of DataItem
    include Voltage, Position, Velocity, Current, Temperature, etc.