---
title: Agent Plug-ins
description: 
published: true
date: 2023-06-26T12:07:12.155Z
tags: 
editor: markdown
dateCreated: 2023-06-26T12:07:12.155Z
---

# Agent Plug-ins

[Agent 2.0.0.2][agent_2-0-0-2] release introduced an extensible plug-in architecture for incoming and outgoing protocols; and data transformations. 

## Data Transformation

The Agent includes an embedded mruby scripting engine for dynamic transformation. Learn more about data transformation using Ruby [here][ruby_transformation]. 

## Third Party Plug-ins

Third party plugins include: OPC-UA MTConnect Companion Spec, NC-Link, Kafka, Influx-DB and other manufacturing standard and/or technologies.

### Implementation Example

[MetaAgent][metagent] utilizes this plug-in architecture and provides rich set of plugins that allow for seamless data integration between MTConnect, NC-Link, and OPC UA. It also enables collection of data continuously in near real-time and time-series databases like RethinkDB, InfluxDB, ElasticSearch, and Kafka. 


[agent_2-0-0-2]: https://github.com/mtconnect/cppagent/releases/tag/v2.0.0.2

[metagent]: https://www.metalogi.io/products/metaagent

[ruby_transformation]: /Data-Transformation-Using-Ruby "wikilink"