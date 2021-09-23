---
title: MTConnect XML Overview
permalink: /MTConnect_XML_Overview/
---

## Reply XML Document Structure

At the top level of all MTConnect® [XML](/Terminology "wikilink")
Documents there MUST be one of the following
[XML](/Terminology "wikilink") elements: *MTConnectDevices*,
*MTConnectStreams*, or *MTConnectError*. This element will be the root
for all MTConnect® responses and contains all sub-elements for the
protocol.

All MTConnect® [XML](/Terminology "wikilink") Documents are broken down
into two parts. The first [XML](/Terminology "wikilink") element is the
Header that provides protocol related information like next sequence
number and creation date and the second section provides the content for
*[Devices](/Terminology "wikilink")*,
*[Streams](/Terminology "wikilink")*, or Errors.

The top level [XML](/Terminology "wikilink") elements MUST contain
references to the [XML](/Terminology "wikilink") schema URN and the
schema location. These are the standard [XML](/Terminology "wikilink")
schema attributes:

``` xml numberLines
<MTConnectStreams xmlns:m="urn:mtconnect.com:MTConnectStreams:1.1"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  ns="urn:mtconnect.com:MTConnectStreams:1.1"
  xsi:schemaLocation="urn:mtconnect.com:MTConnectStreams:1.1 http://www.mtconnect.org/schemas/MTConnectStreams.xsd"> …
```

### MTConnectDevices

MTConnectDevices provides the descriptive information about each
[device](/Terminology "wikilink") served by this
[Agent](/Terminology "wikilink") and specifies the [data
items](/Terminology "wikilink") that are available. In an
MTConnectDevices [XML](/Terminology "wikilink") Document, there MUST be
a Header and it MUST be followed by [Devices](/Terminology "wikilink")
section.

[none|500px|thumb|Figure 1: MTConnect Devices
Structure](/File:DevicesStructure.PNG "wikilink")

MTConnectDevices XML Document MUST have the following structure:

``` xml numberLines
<MTConnectDevices xmlns:m="urn:mtconnect.com:MTConnectDevices:1.1"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  ns="urn:mtconnect.com:MTConnectDevices:1.1"
  xsi:schemaLocation="urn:mtconnect.com:MTConnectDevices:1.1
  http://www.mtconnect.org/schemas/MTConnectDevices_1.1.xsd">
  <Header …/>
    <Devices> … </Devices>
</MTConnectDevices>
```

An MTConnectDevices element MUST include the Header for all documents
and the [Devices](/Terminology "wikilink") element.

[none|500px|thumb|Figure 2: MTConnectDevices
Elements](/File:DevicesElements.PNG "wikilink")

### MTConnectStreams

MTConnectStreams contains a timeseries of
[Samples](/Terminology "wikilink"), [Events](/Terminology "wikilink"),
and Condition from [devices](/Terminology "wikilink") and their
[components](/Terminology "wikilink"). In an MTConnectStreams
[XML](/Terminology "wikilink") Document, there MUST be a Header and it
MUST be followed by a [Streams](/Terminology "wikilink") section.

[none|500px|thumb|Figure 3: MTConnectStreams
Structure](/File:StreamsStructure.PNG "wikilink")

An MTConnectStreams [XML](/Terminology "wikilink") Document will have
the following structure:

``` xml numberLines
<MTConnectStreams xmlns:m="urn:mtconnect.com:MTConnectStreams:1.1"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  ns="urn:mtconnect.com:MTConnectStreams:1.1"
  xsi:schemaLocation="urn:mtconnect.com:MTConnectStreams:1.1 http://www.mtconnect.org/schemas/MTConnectStreams.xsd">
  <Header … />
  <Streams> … </Streams>
</MTConnectStreams>
```

An MTConnectStreams document MUST include a Header and a
[Streams](/Terminology "wikilink") element.

[none|500px|thumb|Figure 4: MTConnectStreams
Elements](/File:StreamsElements.PNG "wikilink")

### MTConnectAssets

An MTConnect asset document contains information pertaining to a machine
tool asset, something that is not a direct
[component](/Terminology "wikilink") of the machine and can be relocated
to another [device](/Terminology "wikilink") during its lifecycle. The
Asset will contain transactional data that will be changed as a unit,
meaning that at any point in time the latest version of the complete
state for this asset will be given by this
[device](/Terminology "wikilink").

[none|500px|thumb|Figure 5: MTConnectAssets
Structure](/File:AssetsStructure.PNG "wikilink")

Each [device](/Terminology "wikilink") may have a different set of
information about this asset and it is the responsibility of the
[application](/Terminology "wikilink") to collect and determine the best
data and keep histories if necessary. MTConnect will allow any
[application](/Terminology "wikilink") or other
[device](/Terminology "wikilink") request this information. MTConnect
makes no guarantees that this will be the best information across the
entire set of [devices](/Terminology "wikilink"), only that from this
[devices](/Terminology "wikilink") perspective, it is the latest and
most accurate information it has supplied.

MTConnect allows any [application](/Terminology "wikilink") to request
information about an asset by its *assetId*. This identifier MUST be
unique for all assets. The uniqueness is defined within the domain used
by the implementation, meaning, if MTConnect will be used within a shop,
the *assetId* MUST be unique within that shop. And conversely, if
MTConnect will be used throughout an enterprise, it is advisable to make
the *assetId* unique throughout the enterprise.

``` xml numberLines
<?xml version="1.0" encoding="UTF-8"?>
<MTConnectAssets xsi:schemaLocation="urn:mtconnect.org:MTConnectAssets:1.2 ../MTConnectAssets_1.2.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ns="urn:mtconnect.org:MTConnectAssets:1.2" xmlns:mt="urn:mtconnect.org:MTConnectAssets:1.2">
  <Header creationTime="2001-12-17T09:30:47Z" sender="localhost" version="1.2" bufferSize="131000" instanceId="1" />
  <Assets>
    <CuttingTool serialNumber="1234" timestamp="2001-12-17T09:30:47Z" assetId="1234-112233">
      <Description>Cutting Tool</Description>
        <ToolDefinition>…</ToolDefinition>
        <ToolLifeCycle deviceUuid="1222" toolId="1234">…
        </ToolLifeCycle>
      </CuttingTool>
    </Assets>
</MTConnectAssets>
```

The document is broken down into two sections, the tool definition (line
7) and the tool life cycle (lines 8-9).

### MTConnectError

[none|500px|thumb|Figure 6: MTConnectError
Structure](/File:ErrorStructure.PNG "wikilink")

An *MTConnectError* document contains information about an *error* that
occurred in processing the request. In an *MTConnectError*
[XML](/Terminology "wikilink") Document, there MUST be a Header and it
must be followed by an *Errors* container that can contain a series of
*Error* elements:

``` xml numberLines
<?xml version="1.0" encoding="UTF-8"?>
<MTConnectError ns="urn:mtconnect.org:MTConnectError:1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:mtconnect.org:MTConnectError:1.1 http://www.mtconnect.org/schemas/MTConnectError_1.1.xsd">
  <Header creationTime="2010-03-12T12:33:01" sender="localhost" version="1.1" bufferSize="131072" instanceId="1268463594" />
  <Errors>
    <Error errorCode="OUT_OF_RANGE" >Argument was out of range</Error>
    <Error errorCode="INVALID_XPATH" >Bad path</Error>
  </Errors>
</MTConnectError>
```

For purposes of backward compatibility, a single *error* can have a
single Error element.

``` xml numberLines
<?xml version="1.0" encoding="UTF-8"?>
<MTConnectError ns="urn:mtconnect.org:MTConnectError:1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:mtconnect.org:MTConnectError:1.1 http://www.mtconnect.org/schemas/MTConnectError_1.1.xsd">
  <Header creationTime="2010-03-12T12:33:01" sender="localhost" version="1.1" bufferSize="131072" instanceId="1268463594" />
  <Error errorCode="OUT_OF_RANGE" >Argument was out of range</Error>
</MTConnectError>
```

An MTConnect® document MUST include the Header for all documents and one
*Error* element.

[none|500px|thumb|Figure 7: MTConnectError
Elements](/File:ErrorElements.PNG "wikilink")

## Header Attributes

[right|500px|Figure 7: Header
Attributes](/File:HeaderAttributes.PNG "wikilink")

The *nextSequence*, *firstSequence*, and *lastSequence* number MUST be
included in [sample](/Terminology "wikilink") and
[current](/Terminology "wikilink") responses. These values MAY be used
by the client [application](/Terminology "wikilink") to determine if the
sequence values are within range. The testIndicator MAY be provided as
needed.

Details on the meaning of various fields and how they relate to the
protocol are described on the [protocol](/protocol "wikilink") page. The
standard specifies how the protocol MUST be implemented to provide
consistent MTConnect® [Agent](/Terminology "wikilink") behavior.

The *instanceId* MAY be implemented using any unique information that
will be guaranteed to be different each time the sequence number counter
is reset. This will usually happen when the MTConnect®
[Agent](/Terminology "wikilink") is restarted. If the
[Agent](/Terminology "wikilink") is implemented with the ability to
recover the [event](/Terminology "wikilink")
[stream](/Terminology "wikilink") and the next sequence number when it
is restarted, then it MUST use the same *instanceId* when it restarts.

The *instanceId* allows the MTConnect® [Agents](/Terminology "wikilink")
to forgo persistence of [Events](/Terminology "wikilink"), Condition,
and [Samples](/Terminology "wikilink") and restart clean each time.
Persistence is a decision for each implementation to be determined. This
is further discussed on the [fault
tolerance](/Fault_Tolerance "wikilink") page.

The sender MUST be included in the header to indicate the identity of
the [Agent](/Terminology "wikilink") sending the response. The sender
MUST be in the following format: <http://>

<address>

\[:port\]/. The *port* MUST only be specified if it is NOT the default
[HTTP](/Terminology "wikilink") port 80.

The *bufferSize* MUST contain the maximum number of results that can be
stored in the [Agent](/Terminology "wikilink") at any one instant. This
number can be used by the [application](/Terminology "wikilink") to
determine how frequently it needs to [sample](/Terminology "wikilink")
and if it can recover in case of failure. It is the decision of the
implementer to determine how large the buffer should be.

As a general rule, the buffer SHOULD be sufficiently large to contain at
least five minutes’ worth of [Events](/Terminology "wikilink"),
Condition, and [Samples](/Terminology "wikilink"). Larger buffers are
more desirable since they allow longer
[application](/Terminology "wikilink") recovery cycles. If the buffer is
too small, data can be lost. The [Agent](/Terminology "wikilink") SHOULD
NOT be designed so it becomes burdensome to the
[device](/Terminology "wikilink") and could cause any interruption to
normal operation.