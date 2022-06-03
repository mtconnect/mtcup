---
title: Adapter Connectivity
description: 
published: true
date: 2022-06-03T01:12:57.141Z
tags: 
editor: markdown
dateCreated: 2022-06-03T01:12:57.141Z
---

# Adapter Connectivity

The adapters communicate with the MTConnect agent using a simple TCP sockets protocol. TCP is merely a convenient streaming character based protocol that has been around for over 30 years and is supported on every machine known to man (except for some manufacturing equipment\!) The semantics were chosen as the simplest possible format to get data from a device to another process with minimal overhead, but still easily verifiable. The format is a pipe delimited line of data terminated with a <LF> or a <CR><LF>, both work. The format is as follows:

```
2013-09-01T21:00:23.0123|name|value
```

The timestamp is in what is referred to as [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) format and it will always be given in UTC – MTConnect does no time zone conversions. The timestamp is optional and if it is not present the agent will supply its own time to the arriving data. A new feature has been added recently to handle clock skew and delta time, I'll cover that later. The format without a timestamp must begin with as | as follows:

```
|name|value
```

To make data collection more efficient, all data that changes at the same time can be represented on a single line as follows: 

```
2013-09-01T21:00:23.0123|name1|value1|name2|value2|name3|value3
```

Each pair will get a separate sequence number, but they will all have the same timestamp. This save bandwidth having to repeat the timestamp multiple times. There are some exceptions to this rule; some data items require more than a single value, they have multiple arguments. These are: Alarm (deprecated), Condition, TimeSeries, and Message. Each of these require a themselves to be on a separate line.

#### Conditions

A condition represents the state of an alarm or warning on the machine. The standard specifies there can be a lot of additional information passed through to represent a condition. These are:

```
time_stamp|name|level|native_code|native_severity|qualifier|error text
```

The level can be one of the following:

  - `UNAVAILABLE`
  - `NORMAL`
  - `WARNING`
  - `FAULT`

The `native_code` is whatever the controller call this alarm, usually a number or pneumonic representation. The `native_severity` is also controller defined and will be merely passed through. The `qualifier` can be currently `HIGH` or `LOW` and the `error_text` is just a pass through from the controller of the text from the controller.

#### Message

The message is simply a `|name|native_code|message` where the `native_code` is similar to the condition and is just what the controller calls the alarm in a numeric or pneumonic form.

#### Time Series

When time series data items are passed through, they must have two additional arguments (the second is optional since it can be defaulted, but an empty cell must be present. The time series is represented as follows:

```
2013-09-01T21:00:23.0123|name|count|rate|data...
```

as in

```
2013-09-01T21:00:23.0123|temp|10|10|18.2 18.4 18.3 18.9 19.0 19.2 19.1 19.3 19.4 19.7
```

Here we have ten samples at 10 hertz. If the 10 hertz was set as part of the data item, it can be omitted here:

```
2013-09-01T21:00:23.0123|temp|10||18.2 18.4 18.3 18.9 19.0 19.2 19.1 19.3 19.4 19.7
```