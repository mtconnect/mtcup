---
title: C++ Agent
permalink: /C++_Agent/
---

## Reference MTConnect Agent

The MTConnect implementation that has been most closely tracking the
MTConnect standard and is used in most production implementation is the
C++ Agent that resides on the MTConnect project pages of github [C++
Agent](http://github.com/mtconnect/cppagent) . As part of the effort, we
also provide pre-built Windows x86 binaries that are statically linked
and can be run on any Windows computer dating from around Windows XP SP2
and later. Earlier builds are possible, but these will be left up to the
reader. The pre-built binaries can be found at: [C++ Agent
Releases](http://github.com/mtconnect/cppagent/releases).

The C++ Agent provides the fullest implementation of the standard
available at the time of release. The version number (Major, Minor,
Patch, Build) are tied to the MTConnect standard. The first three
(Major, Minor, Build) are based directly on the MTConnect standard
release nomenclature and the build number will increment as new versions
of the C++ agent is released. When the agent is in pre-release for a
upcoming version of the standard, it will be suffixed with a RCn where
this stands for Release Candidate n as in RC1.

The agent has a rich level of configuration and is capable of serving up
content from the local file system and allowing changes to be pushed. To
get the full details on the configuration, again, visit the [C++ Agent
github page](http://github.com/mtconnect/cppagent) and scroll down to
the readme.md. There are many features built in to the agent, some
documented better than others. We will try to document some of the more
interesting capabilities on these pages.

#### Responsibilities

There are certain responsibilities the Agent has and certain
responsibilities the Adapter has in this architecture. The adapter is
responsible for the following functionality:

1.  Connect to the controller's proprietary interface.
2.  Make the requisite calls to the controllers interfaces to retrieve
    the necessary data.
3.  Convert the controller state to events in the proper vocabulary. For
    example convert from Register 0x32 bit 6-9 to READY, ACTIVE,
    STOPPED.
4.  Format the data in the pipe delimited format described below.
5.  Respond to PING requests from the Agent in a timely manor to provide
    keep-alive heartbeats.
6.  Provide a complete snapshot of the data when an agent connects.

The agent is responsible for the following:

1.  Receive RESTful requests from application and respond with proper
    MTConnect XML.
2.  Provide push based streaming data when requested.
3.  Connect to the adapters and maintain the connections using
    heartbeats. Detect failures and cleanup connections.
4.  Receive the data from the adapter and parse the pipe delimited
    values.
5.  Minimally validate the data.
6.  Convert numeric values from the native units to the MTConnect units
    if required.
7.  Maintain the circular storage of events and assets.
8.  Handle many-to-many mappings from adapters to devices.
9.  Manage time series data in efficient manor.
10. Handle near-real-time updates to applications that have requested
    interval=0 notifications.
11. Efficiently format XML for applications.
12. Serve static XSD and other static assets as configured.
13. Handle timestamps from adapters or overwrite the timestamps using
    relative time adjustment or current clock time.

The agent is by far the more complex piece of software and takes a lot
of work to get right. The adapters can be written in as little as 5
lines of Python or Ruby script and be perfectly adequate. The
architecture was designed to operate in this way to make reduce the
complexity of the adapter tools. Visit the C++ adapter SDK and the C\#
adapter SDK to get a feel for what it takes to write an adapter. There
are also videos on how to develop adapters here
[Other_Resources](/Other_Resources "wikilink").

### Adapter Connectivity

The adapters communicate with the MTConnect agent using a simple TCP
sockets protocol. TCP is merely a convenient streaming character based
protocol that has been around for over 30 years and is supported on
every machine known to man (except for some manufacturing equipment\!)
The semantics were chosen as the simplest possible format to get data
from a device to another process with minimal overhead, but still easily
verifiable. The format is a pipe delimited line of data terminated with
a <LF> or a <CR><LF>, both work. The format is as follows:

` 2013-09-01T21:00:23.0123|name|value`

The timestamp is in what is referred to as
[ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) format and it will
always be given in UTC – MTConnect does no time zone conversions. The
timestamp is optional and if it is not present the agent will supply its
own time to the arriving data. A new feature has been added recently to
handle clock skew and delta time, I'll cover that later. The format
without a timestamp must begin with as | as follows:

` |name|value`

To make data collection more efficient, all data that changes at the
same time can be represented on a single line as follows:

` 2013-09-01T21:00:23.0123|name1|value1|name2|value2|name3|value3`

Each pair will get a separate sequence number, but they will all have
the same timestamp. This save bandwidth having to repeat the timestamp
multiple times. There are some exceptions to this rule; some data items
require more than a single value, they have multiple arguments. These
are: Alarm (deprecated), Condition, TimeSeries, and Message. Each of
these require a themselves to be on a separate line.

#### Conditions

A condition represents the state of an alarm or warning on the machine.
The standard specifies there can be a lot of additional information
passed through to represent a condition. These are:

` time_stamp|name|level|native_code|native_severity|qualifier|error text`

The level can be one of the following:

  - `UNAVAILABLE`
  - `NORMAL`
  - `WARNING`
  - `FAULT`

The `native_code` is whatever the controller call this alarm, usually a
number or pneumonic representation. The `native_severity` is also
controller defined and will be merely passed through. The `qualifier`
can be currently `HIGH` or `LOW` and the `error_text` is just a pass
through from the controller of the text from the controller.

#### Message

The message is simply a `|name|native_code|message` where the
`native_code` is similar to the condition and is just what the
controller calls the alarm in a numeric or pneumonic form.

#### Time Series

When time series data items are passed through, they must have two
additional arguments (the second is optional since it can be defaulted,
but an empty cell must be present. The time series is represented as
follows:

` 2013-09-01T21:00:23.0123|name|count|rate|data...`

as in

` 2013-09-01T21:00:23.0123|temp|10|10|18.2 18.4 18.3 18.9 19.0 19.2 19.1 19.3 19.4 19.7`

Here we have ten samples at 10 hertz. If the 10 hertz was set as part of
the data item, it can be omitted here:

` 2013-09-01T21:00:23.0123|temp|10||18.2 18.4 18.3 18.9 19.0 19.2 19.1 19.3 19.4 19.7`

#### Installing C++ Agent on Ubuntu

[Installing C++ Agent on
Ubuntu](/Installing_C++_Agent_on_Ubuntu "wikilink")

#### Installing C++ Agent on Raspberry Pi

[Installing C++ Agent on Raspberry
Pi](/Installing_C++_Agent_on_Raspberry_Pi "wikilink")