---
title: HTTP Primer
description: 
published: true
date: 2021-09-25T02:14:11.305Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:31:37.640Z
---

## Architectural Overview

MTConnect® is built upon the most prevalent standards in the industry.
This maximizes the number of tools available for implementation and
provides the highest level of interoperability with other standards and
protocols.

MTConnect® MUST use the [HTTP](/Terminology "wikilink") protocol as the
underlying transport for all messaging. The data MUST be sent back in
valid [XML](/Terminology "wikilink"), according to this standard. Each
MTConnect® [Agent](/Terminology "wikilink") MUST represent at least one
[device](/Terminology "wikilink"). The [Agent](/Terminology "wikilink")
MAY represent more than one [device](/Terminology "wikilink") if
desired.

MTConnect® is composed of a few basic conceptual parts. They are as
follows:

**Header** Protocol related information.

**Components** The building blocks of the
[device](/Terminology "wikilink").

**DataItems** The description of the data available from the
[device](/Terminology "wikilink").

**Streams** A set of [Samples](/Terminology "wikilink"),
[Events](/Terminology "wikilink"), or Conditon for
[components](/Terminology "wikilink") and
[devices](/Terminology "wikilink").

**Assets** An Asset is something that is associated with the
manufacturing process that is not a [component](/Terminology "wikilink")
of a [device](/Terminology "wikilink"), can be removed without detriment
to the function of the [device](/Terminology "wikilink"), and can be
associated with other [devices](/Terminology "wikilink") during their
lifecycle.

**Samples** A point-in-time measurement of a [data
item](/Terminology "wikilink") that is continuously changing.

**Events** Discrete changes in state that can have no intermediate
value. They indicate the state of a specific
[attribute](/Terminology "wikilink") of a
[component](/Terminology "wikilink").

**Condition** A piece of information the
[device](/Terminology "wikilink") provides as an indicator of its health
and ability to function. A condition can be one of Normal, Warning,
Fault, or Unavailable. A single condition type can have multiple Faults
or Warnings at any given time. This behavior is different from
[Events](/Terminology "wikilink") and [Samples](/Terminology "wikilink")
where a [data item](/Terminology "wikilink") MUST only have a single
value at a given time.

## Request Structure

An MTConnect® request SHOULD NOT include any body in the
[HTTP](/Terminology "wikilink") request. If the
[Agent](/Terminology "wikilink") receives any additional data, the
[Agent](/Terminology "wikilink") MAY ignore it. There will be no cookies
or additional information considered; the only information the
[Agent](/Terminology "wikilink") MUST consider is the
[URI](/Terminology "wikilink") in the [HTTP](/Terminology "wikilink")
GET (Type a [URI](/Terminology "wikilink") into the browser’s address
bar, hit return, and a GET is sent to the server. In fact, with
MTConnect® one can do just that. To test the
[Agent](/Terminology "wikilink"), one can type the
[Agent](/Terminology "wikilink")’s [URI](/Terminology "wikilink") into
the browser’s address bar and view the results.)

## Process Workflow

What follows is the typical interaction between four entities in the
MTConnect® architecture: the *Name Service* (an
[LDAP](/Terminology "wikilink") server that translates
[device](/Terminology "wikilink") names to the
[Agent](/Terminology "wikilink")’s [URI](/Terminology "wikilink")), the
*[Application](/Terminology "wikilink")* (a user
[application](/Terminology "wikilink") that makes special use of the
[device](/Terminology "wikilink")’s data), the
*[Agent](/Terminology "wikilink")* (the process collecting data from the
[device](/Terminology "wikilink") and delivering it to the
[applications](/Terminology "wikilink")), and the
[Device](/Terminology "wikilink") (the physical piece of equipment).

### Agent Initialization

For this example, the [agent](/Terminology "wikilink") first
authenticates itself with the Name Server (if used). In the second part
of the example, it shows how the entities interrelate in an
architecture.

![AgentInitialization.PNG](/images/AgentInitialization.PNG)

Figure 1 above illustrates the initialization of the
[Agent](/Terminology "wikilink") and communication with the
[device](/Terminology "wikilink") “mill”. Implementors Note: This is the
recommended architecture and implementations SHOULD refer to this when
developing their MTConnect® [Agents](/Terminology "wikilink").

**Step 1.** The [Agent](/Terminology "wikilink") connects and
authenticates itself with the Name Service
([LDAP](/Terminology "wikilink") server).

**Step 2.** The [Agent](/Terminology "wikilink") registers its
[URI](/Terminology "wikilink") with the Name Service so it can be
located.

**Step 3.** The [Agent](/Terminology "wikilink") connects to the
[Device](/Terminology "wikilink") using the
[device](/Terminology "wikilink")’s API or another specialized process.

**Step 4.** The [device](/Terminology "wikilink") sends data to the
[Agent](/Terminology "wikilink") or the [Agent](/Terminology "wikilink")
polls the [device](/Terminology "wikilink") for data.

### Application Communication

![ApplicationCommunication.PNG](/images/ApplicationCommunication.PNG)

Figure 2 above shows how all major [components](/Terminology "wikilink")
of an MTConnect® architecture inter-relate and how the four basic
operations are used to locate and communicate with the
[Agent](/Terminology "wikilink") regarding the
[device](/Terminology "wikilink").

**Step 1.** The [device](/Terminology "wikilink") is continually sending
information to the [Agent](/Terminology "wikilink"). The
[Agent](/Terminology "wikilink") is collecting the information and
saving it based on its ability to store information. The data flow from
the [device](/Terminology "wikilink") to the
[agent](/Terminology "wikilink") is implementation dependant. The data
flow can begin once a request has been issued from a client
[application](/Terminology "wikilink") at the discretion of the
[agent](/Terminology "wikilink").

**Step 2.** The [Application](/Terminology "wikilink") locates the
[device](/Terminology "wikilink") using the *Name Service* with the
standard [LDAP](/Terminology "wikilink") syntax that is interpreted as
follows: the mill is in the organizational unit of Equip which is in the
example.com domain. The [LDAP](/Terminology "wikilink") record for this
[device](/Terminology "wikilink") will contain a
[URI](/Terminology "wikilink") that the
[Application](/Terminology "wikilink") can use to contact the
[Agent](/Terminology "wikilink").

**Step 3.** The [Application](/Terminology "wikilink") has the
[URI](/Terminology "wikilink") to contact the
[Agent](/Terminology "wikilink") for the mill
[device](/Terminology "wikilink"). The first step is a request for the
[device](/Terminology "wikilink")’s descriptive information using the
*[probe](/Terminology "wikilink")* request. The
*[probe](/Terminology "wikilink")* will return the
[component](/Terminology "wikilink") composition of the
[device](/Terminology "wikilink") as well as all the [data
items](/Terminology "wikilink") available.

**Step 4.** The [Application](/Terminology "wikilink") requests the
*[current](/Terminology "wikilink")* state for the
[device](/Terminology "wikilink"). The results will contain the
[device](/Terminology "wikilink") [stream](/Terminology "wikilink") and
all the [component](/Terminology "wikilink")
[streams](/Terminology "wikilink") for this
[device](/Terminology "wikilink"). Each of the [data
items](/Terminology "wikilink") will report their values as
[Samples](/Terminology "wikilink"), [Events](/Terminology "wikilink") or
Condition. The [application](/Terminology "wikilink") will receive the
*nextSequence* number from the [Agent](/Terminology "wikilink") to use
in the subsequent [sample](/Terminology "wikilink") request.

**Step 5.** The [Application](/Terminology "wikilink") uses the
*nextSequence* number to sample the data from the
[Agent](/Terminology "wikilink") starting at sequence number 208. The
results will be [Events](/Terminology "wikilink"), Condition, and
[Samples](/Terminology "wikilink"); and since the count is not
specified, it defaults to 100 [Events](/Terminology "wikilink"),
[Samples](/Terminology "wikilink"), and Conditions.

## MTConnect Agent Data Storage

The MTConnect [Agent](/Terminology "wikilink") stores a fixed amount of
data. This makes the process remain at a fixed maximum size since it is
only required to store a finite number of
[events](/Terminology "wikilink"), [samples](/Terminology "wikilink")
and conditions. The data storage for MTConnect can be thought of as a
tube where data is pushed in one end. When the tube fills up, the oldest
piece of data falls out the other end. As seen in Figure 3 below, the
capacity, or *bufferSize*, of the MTConnect
[Agent](/Terminology "wikilink") in this example is 8. As each piece of
data is inserted into the tube it is assigned a sequentially increasing
number.

![DataStorage.PNG](/images/DataStorage.PNG)

As a client requests data from the MTConnect
[Agent](/Terminology "wikilink") it can specify the sequence number from
which it will start returning data and the number of items to inspect.
In Figure 3, the request starts at 15 (*from*) and requests three items
(*count*). This will set the next sequence number (*nextSequence*) to 18
and the last sequence number will always be the last number in the tube.
In this example the (*lastSequence*) is 19.

If the request goes off the end of the tube, the next sequence is set to
the *lastSequence* + 1. As long as no more data is added to the
*[Agent](/Terminology "wikilink")* and the request exceeds the length of
the data available, the *nextSequence* will remain the same, in this
case 20.

The *current* request MUST provide the last value for each [data
item](/Terminology "wikilink") even if it is no longer in the buffer.
Even if the [event](/Terminology "wikilink"),
[sample](/Terminology "wikilink"), or condition has been removed from
the buffer, the [Agent](/Terminology "wikilink") MUST retain a copy
associated with the last value for any subsequent *current* request.
Therefor if the item 11 above was the last value for the X Position, the
*current* will still provide the value of 11 when requested.

## MTConnect Agent Asset Storage

MTConnect stores assets in a similar fashion. The
[Agent](/Terminology "wikilink") provides a limited number of assets
that can be stored at one time and uses the same method of pushing out
the oldest asset when the buffer is full. The buffer size for the asset
storage is maintained separately from the
[sample](/Terminology "wikilink"), [event](/Terminology "wikilink"), and
condition storage.

Assets also behave like a key/value in memory database. In the case of
the asset the key is the *assetId* and the value is the
[XML](/Terminology "wikilink") describing the asset. The key can be any
string of letters, punctuation or digits and represent the domain
specific coding scheme for their assets. Each asset type will have a
recommended way to construct a unique *assetId*, for example, a Cutting
Tool SHOULD be identified by the tool ID and serial number as a composed
synthetic identifier.

![AssetStorage.PNG](/images/AssetStorage.PNG)

As in this Figure 4, each of the assets is referred to by their key. The
key is independent of the order in the storage buffer.

Asset protocol:

  - Request an asset by ID:

    - url: http://example.com/asset/hh1
     Returns the *MTConnectAssets* document for asset hh1

  - Request multiple assets by ID:

    - url: http://example.com/asset/hh1;cc;123;g5
    Returns the *MTConnectAssets* document for asset hh1, cc, 123, and g5.

  - Request for all the assets in the *[Agent](/Terminology  "wikilink")*:

    - url: http://example.com/assets  
    Returns all available MTConnect assets in the [Agent](/Terminology "wikilink"). MTConnect MAY return a limited set if there are too many asset records. The assets MUST be added to the beginning with the most recently modified assets.

  - Request for all assets of a given type in the *[Agent](/Terminology "wikilink")*:

    - url: http://example.com/assets?type=”CuttingTool”  
    Returns all available *CuttingTool* assets from the MTConnect *[Agent](/Terminology "wikilink")*. MTConnect MAY return a limited set if there are too many asset records. The assets MUST be added to the beginning with the most recently modified assets.

When an asset is added or modified, an *AssetChanged*
[event](/Terminology "wikilink") will be sent to inform us that new
asset data is available. The [application](/Terminology "wikilink") can
request the new asset data from the [device](/Terminology "wikilink") at
that time. Every time the asset data is modified the *AssetChanged*
[event](/Terminology "wikilink") will be sent. Since the asset data is
transactional, meaning multiple values change at the same time, the
system will send out a single *AssetChanged*
[event](/Terminology "wikilink") for the entire set of changes, not for
the individual changed fields.

The tool data MUST remain constant until the *AssetChanged*
[event](/Terminology "wikilink") is sent. Once it is sent the data MUST
change to reflect the new content at that instant. The timestamp of the
asset will reflect the time the last change was made to the asset data.

Every time an asset is modified or added it will be moved to the end of
the buffer and become the newest asset. As the buffer fills up, the
oldest asset will be pushed out and its information will be removed.
MTConnect does not specify the maximum size of the buffer, and if the
implementation desires, permanent storage MAY be used to store the
assets. A value of 4,294,967,296 or 2³² can be given to indicate
unlimited storage.

There is no requirement for persistent asset storage. If the
[Agent](/Terminology "wikilink") fails, all existing assets MAY be lost.
It is the responsibility of the implementation to restore the lost asset
data and it is the responsibility of the
[application](/Terminology "wikilink") to persist the asset data. The
MTConnect [agent](/Terminology "wikilink") MAY make no guarantees about
availability of asset data after the [Agent](/Terminology "wikilink")
stops.