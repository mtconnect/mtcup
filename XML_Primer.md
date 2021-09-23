---
title: XML Primer
permalink: /XML_Primer/
---

## XML

[XML](/Terminology "wikilink") consists of a hierarchy of elements. The
elements can contain sub-elements, [CDATA](/Terminology "wikilink"), or
both. For this specification, however, an element never contains mixed
content or both sub-elements and [CDATA](/Terminology "wikilink").
[Attributes](/Terminology "wikilink") are additional information
associated with an element. The textual representation of an element is
referred to as a [tag](/Terminology "wikilink"). See the following
example:

1\. <Foo name=”bob”>Ack\!</Foo>

An [XML](/Terminology "wikilink") element consists of a named opening
and closing [tag](/Terminology "wikilink"). In the above example,
\<Foo...\> is referred to as the opening [tag](/Terminology "wikilink")
and </Foo> is referred to as the closing [tag](/Terminology "wikilink").
The text *Ack\!* in between the opening and closing
[tags](/Terminology "wikilink") is called the
[CDATA](/Terminology "wikilink"). [CDATA](/Terminology "wikilink") can
be restricted to certain formats, patterns, or words. In the document
when it refers to an element having [CDATA](/Terminology "wikilink"), it
indicates that the element has no sub-elements and only contains data.

When one looks at an [XML](/Terminology "wikilink") Document there are
two parts. The first part is typically referred to as an
[XML](/Terminology "wikilink") declaration and is only a single line. It
looks something like this:

2\.

<?xml version="1.0" encoding="UTF-8"?>

This line indicates the [XML](/Terminology "wikilink") version being
used and the character encoding. Though it is possible to leave this
line off, it is usually considered good form to include this line in the
beginning of the document.

Every [XML](/Terminology "wikilink") Document contains one and only one
root element. In the case of MTConnect, it is the *MTConnectDevices*,
*MTConnectStreams*, *MTConnectAssets*, or *MTConnectError* element. When
these root elements are used in the examples, you will sometimes notice
that it is prefixed with *mt* as in *mt:MTConnectDevices*.

The *mt* is what is referred to as a namespace alias and it refers to
the urn *<urn:mtconnect.org:MTConnectDevices:1.2>* in the case of an
*MTConnectDevices* document. The urn is the important part and MUST be
consistent between the schema and the [XML](/Terminology "wikilink")
document. The namespace alias will be included as an attribute of the
[XML](/Terminology "wikilink") element as in:

``` XML numberLines
<MTConnectDevices
  xmlns:m="urn:mtconnect.org:MTConnectDevices:1.2"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  ns="urn:mtconnect.org:MTConnectDevices:1.2"
  xsi:schemaLocation="urn:mtconnect.org:MTConnectDevices:1.2
  http://www.mtconnect.org/schemas/MTConnectDevices_1.2.xsd">
```

In the above example, the alias *m* refers to the *MTConnectDevices*
urn. This document also contains a default namespace on line 4 which is
specified with an *xmlns* attribute without an alias. There is an
additional namespace that is always included in all
[XML](/Terminology "wikilink") documents and usually assigned the alias
*xsi*. This namespace is used to refer to all the standard
[XML](/Terminology "wikilink") data types prescribed by the
[W3C](http://www.w3.org/XML/). An example of this is the
*xsi:schemaLocation* attribute that tells the
[XML](/Terminology "wikilink") parser where the schema can be found.

In [XML](/Terminology "wikilink"), to allow for multiple
[XML](/Terminology "wikilink") Schemas to be used within the same
[XML](/Terminology "wikilink") Document, a namespace will indicate which
[XML](/Terminology "wikilink") Schema is in effect for this section of
the document. This convention allows for multiple
[XML](/Terminology "wikilink") Schemas to be used within the same
[XML](/Terminology "wikilink") Document, even if they have the same
element names. The namespace is optional and is only required if
multiple schemas are required.

An [attribute](/Terminology "wikilink") is additional data that can be
included in each element. For example, in the following MTConnect®
*[DataItem](/Terminology "wikilink")*, there are several attributes
describing the *[DataItem](/Terminology "wikilink")*:

``` xml numberLines
<DataItem name=”Xpos” type=”POSITION” subType=”ACTUAL” category=”SAMPLE” />
```

The *name*, *type*, *subType*, and *category* are
[attributes](/Terminology "wikilink") of the element. Each
[attribute](/Terminology "wikilink") can only occur once within an
element declaration, and it can either be required or optional.

An element can have any number of sub-elements. The
[XML](/Terminology "wikilink") Schema specifies which sub-elements and
how many times a given sub-element can occur. Here’s an example:

``` xml numberLines
<TopLevel>
  <FirstLevel>
    <SecondLevel>
      <ThirdLevel name=”first”></ThirdLevel>
      <ThirdLevel name=”second”></ThirdLevel>
    </SecondLevel>
  </FirstLevel>
</TopLevel>
```

In the above example, the *FirstLevel* has an sub-element *SecondLevel*
which in turn has two sub-elements, *ThirdLevel*, with different names.
Each level is an element and its children are its sub-elements and so
forth.

In [XML](/Terminology "wikilink") we sometimes use elements to organize
parts of the document. A few examples in MTConnect® are
*[Streams](/Terminology "wikilink")*,
*[DataItems](/Terminology "wikilink")*, and
*[Components](/Terminology "wikilink")*. These elements have no
[attributes](/Terminology "wikilink") or data of their own; they only
provide structure to the document and allow for various parts to be
addressed easily.

In the following example *[DataItems](/Terminology "wikilink")* and
*[Components](/Terminology "wikilink")* are only used to contain certain
types of elements and provide structure to the documents. These elements
will be referred to as *Containters* in the standard.

``` xml numberLines
…
  <Device id=”d” name=”Device”>
    <DataItems>
      <DataItem …/>
         …
      </DataItems>
      <Components>
         <Axes … >…</Axes>
      </Components>
   </Device>
```

An [XML](/Terminology "wikilink") Document can be validated. The most
basic check is to make sure it is well-formed, meaning that each element
has a closing [tag](/Terminology "wikilink"), as in <foo>...</foo> and
the document does not contain any illegal characters (\<\>) when not
specifying a [tag](/Terminology "wikilink"). If the closing </foo> was
left off or an extra \> was in the document, the document would not be
well-formed and may be rejected by the receiver. The document can also
be validated against a schema to ensure it is valid. This second level
of analysis checks to make sure that required elements and
[attributes](/Terminology "wikilink") are present and only occur the
correct number of times. A valid document must be well-formed.

All MTConnect® documents must be valid and conform to the
[XML](/Terminology "wikilink") Schema provided along with this
specification. The schema will be versioned along with this
specification. The greatest possible care will be taken to make sure
that the schema is backward compatible.

For more information, visit the [w3c](http://www.w3.org/XML/) website
for the XML Standards documentation: <http://www.w3.org/XML/>

### Markup Conventions

MTConnect® follows industry conventions on
[tag](/Terminology "wikilink") format and notations when developing the
[XMLschema](/Terminology "wikilink"). The general guidelines are as
follows:

1.  All tag names will be specified in Pascal case (first letter of each
    word is capitalized). For example: <ComponentEvents />
2.  Attribute names will also be camel case, similar to Pascal case, but
    the first letter will be lower case. For example:
    <MyElement attributeName=”bob”/>
3.  All values that are part of a limited or controlled vocabulary will
    be in upper case with an _ (underscore) separating words. For
    example: ON, OFF, ACTUAL, COUNTER_CLOCKWISE, etc…
4.  Dates and times will follow the W3C ISO 8601 format with arbitrary
    decimal fractions of a second allowed. Refer to the following
    specification for details: <http://www.w3.org/TR/NOTE-datetime> The
    format will be YYYY-MM-DDThh:mm:ss.ffff, for example
    2007-09-13T13:01.213415. The accuracy and number of decimal
    fractional digits of the timestamp is determined by the capabilities
    of the [device](/Terminology "wikilink") collecting the data. All
    times will be given in UTC (GMT).
5.  [XML](/Terminology "wikilink") element names will be spelled-out and
    abbreviations will be avoided. The one exception is the word
    identifier that will be abbreviated Id. For example: SequenceNumber
    will be used instead of SeqNum.

[MTConnect XML Overview](/MTConnect_XML_Overview "wikilink")