---
title: Fault Tolerance
description: 
published: true
date: 2021-09-25T02:04:07.545Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:31:19.595Z
---

## Fault Tolerance and Recovery

MTConnect® does not provide a guaranteed delivery mechanism. The
[protocol](/protocol "wikilink") places the responsibility for recovery
on the [application](/Terminology "wikilink").

### Application Failure

The [application](/Terminology "wikilink") failure scenario is easy to
manage if the [application](/Terminology "wikilink") persists the next
sequence number after it processes each response. The MTConnect®
protocol provides a simple recovery strategy that only involves
reissuing the previous request with the recovered next sequence number.

There is the risk of missing some [Events](/Terminology "wikilink"),
[Samples](/Terminology "wikilink"), and Condition if the time between
requests exceeds the capacity of the [Agent](/Terminology "wikilink")’s
buffer. In this case, there is no record of the missing information and
it is lost. If the [application](/Terminology "wikilink") automatically
restarts after failure, the intervening data can be quickly recovered.
If this cannot be done, the current state of the
[device](/Terminology "wikilink") can be retrieved and the
[application](/Terminology "wikilink") can continue from that point
onward. See Figure 1.

![Figure1.PNG](/images/Figure1.PNG)

### Agent Failure

[Agent](/Terminology "wikilink") failure is the more complex scenario
and requires the use of the instanceId. The instanceId was created to
facilitate recovery when the [Agent](/Terminology "wikilink") fails and
the [application](/Terminology "wikilink") is unaware. Since
[HTTP](/Terminology "wikilink") is a connectionless protocol, there is
no way for the [application](/Terminology "wikilink") to easily detect
that the [Agent](/Terminology "wikilink") has restarted, the buffer has
been lost, and the sequence number has been reset to 1. It should also
be noted that all values will be reinitialized to UNAVAILABLE upon
[Agent](/Terminology "wikilink") restart except for [data
items](/Terminology "wikilink") that are constrained to single values.
See Unavailability of Data below for a full explanation.

In Figure 2, the instanceId is increased from 1 to 2 indicating that
there was a discontinuity in the sequence numbers and all values for the
[data items](/Terminology "wikilink") are reset to UNAVAILABLE. When the
[application](/Terminology "wikilink") detects the change in instanceId,
it MUST reset its next sequence number and retry its request from
sequence number 1. The next request will retrieve all data starting from
the first available [event](/Terminology "wikilink") or
[sample](/Terminology "wikilink").

![Figure2.PNG](/images/Figure2.PNG)

### Data Persistence and Recovery

The implementer of the [Agent](/Terminology "wikilink") can decide on
the strategy regarding the storage of [Events](/Terminology "wikilink"),
Condition, and [Samples](/Terminology "wikilink"). In the simplest form,
the [Agent](/Terminology "wikilink") can persist no data and hold all
the results in volatile memory. If the [Agent](/Terminology "wikilink")
has a method of persisting the data fast enough and has sufficient
storage, it MAY save as much or as little data as is practical in a
recoverable storage system.

If the [Agent](/Terminology "wikilink") can recover data and sequence
numbers from a storage system, it MUST NOT change the instanceId when it
restarts. This will indicate to the
[application](/Terminology "wikilink") that it need not reset the next
sequence number when it requests the next set of data from the
[Agent](/Terminology "wikilink").

If the [Agent](/Terminology "wikilink") persists no data, then it MUST
change the instanceId to a different value when it restarts. This will
ensure that every [application](/Terminology "wikilink") receiving
information from the [Agent](/Terminology "wikilink") will know to reset
the next sequence number.

The instanceId can be any unique number that will be guaranteed to
change every time the [Agent](/Terminology "wikilink") restarts. If the
[Agent](/Terminology "wikilink") will take longer than one second to
start, the [UNIX time](http://en.wikipedia.org/wiki/Unix_time) (seconds
since January 1, 1970) MAY be used for identification an
[instance](/Terminology "wikilink") of the MTConnect®
[Agent](/Terminology "wikilink") in the instanceId.

### Unavailability of Data

Every time the [Agent](/Terminology "wikilink") is initialized all
values MUST be set to UNAVAILABLE unless they are constant valued [data
items](/Terminology "wikilink"). Even during restarts this MUST occur so
that the [application](/Terminology "wikilink") can detect a
discontinuity of data and easily determine that gap between the last
reported valid values.

In the event no data is available, the value for the [data
item](/Terminology "wikilink") in the [stream](/Terminology "wikilink")
MUST be UNAVAILABLE. This value indicates that the value is currently
indeterminate and no assumptions are possible. MTConnect® supports
multiple data sources per [device](/Terminology "wikilink"), and for
that reason, every [data item](/Terminology "wikilink") MUST be
considered independent and MUST maintain its own connection status.

In the following example, the data source for a temperature sensor
becomes temporarily disconnected from the
[Agent](/Terminology "wikilink"). At this point the value changes from
the current temperature to UNAVAILABLE since the temperature can no
longer be determined. In Figure 3, the temperatures range around 100
until it becomes disconnected and then in the future it reconnects and
the temperature is 30. Between these two points assumptions SHOULD NOT
be made as to the temperature since no information was available.

![UnavailableData.PNG](/images/UnavailableData.PNG)

If data for multiple [data items](/Terminology "wikilink") are delivered
from one source and that source becomes unavailable, all [data
items](/Terminology "wikilink") associated with that source MUST have
the value UNAVAILABLE. This MUST be a synchronous operation where all
related [data items](/Terminology "wikilink") will get that value with
the same time stamp. The value will remain UNAVAILABLE until the data
source has reconnected.