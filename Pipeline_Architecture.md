---
title: Reference Agent Pipeline Architecture
description: Internal architecture of the MTConnect Agent to extend data transformations
published: true
date: 2023-01-22T19:16:13.895Z
tags: 
editor: markdown
dateCreated: 2023-01-22T18:46:52.518Z
---

# MTConnect Agent Pipeline Architecture

## Overview

The MTConnect Agent uses a data transformation pipeline to provide a flexible and mutable mechanism for processing incoming data from various sources and allowing for reusability of common transform components. 

![Transforms](/images/transforms.png)

The `Start` transform begins all pipelines and passes the incoming data to the downstream transforms. A transform has two required capabilities: `Guard` and `Transform`. The `Guard` allows the transformation to decide if the data is something it can handle if what action should be taken next. 

If the guard returns `CONTINUE`, the search for a match will continue to the next transform in the list. If the guard returns `SKIP`, the transform will be skipped and the data will be passed to the transform's list of subsequent transforms. 

## Entities used in the pipelines

![Pipeline Entities](/images/pipelineentities.png)

## SHDR Adapter Pipeline

![SHDR Pipeline](/images/shdrpipeline.png)

## MQTT Adapter Pipeline

Based on the incoming message and topic, the agent attempts to map the topic to a device and data item. Following the attempt to determine the device and data item, it will look at the first character. If it is a `{` it will create a JsonMessage and forward it to the `JsonMapper`.

At present the `JsonMapper` cannot decode the data, so it ignores it. Otherwise the payload is assumed to be SHDR and is passed to the SHDR mapper.

![MQTT Pipeline](/images/mqttpipeline.png)