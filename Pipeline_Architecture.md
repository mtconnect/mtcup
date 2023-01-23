---
title: Reference Agent Pipeline Architecture
description: Internal architecture of the MTConnect Agent to extend data transformations
published: true
date: 2023-01-23T10:19:28.825Z
tags: 
editor: markdown
dateCreated: 2023-01-22T18:46:52.518Z
---

# MTConnect Agent Pipeline Architecture

## Overview

The MTConnect Agent uses a data transformation pipeline to provide a flexible and mutable mechanism for processing incoming data from various sources and allowing for reusability of common transform components. 

## Transforms

The transfrom is a simple class that takes checks to see if it can transform an entity and, if applicable, applies a transformation to the entity. The result is another entity, 

![Transforms](/images/transforms.png)

A transform has two required capabilities: `Guard` and `Transform`. The `Guard` allows the transformation to decide if the data is something it can handle if what action should be taken next. 

The transform has a list of downstream transforms, each of which is evaluated sequentually. The action is taken based on the result of the Guard. The Guard is a function that matches based on the type of the entity or some other characteristics like the name. 

The guard can return one of the three following responses:

* `CONTINUE`: the search for a match will continue to the next transform in the list. 
* `SKIP`: The search stops, but the transform is not run. The transform's list of next transforms is evaluated to determine the next step. 
* `RUN`: The transform is executed.

## Entities used in the pipelines

![Pipeline Entities](/images/pipelineentities.png)

## SHDR Adapter Pipeline

![SHDR Pipeline](/images/shdrpipeline.png)x`

## MQTT Adapter Pipeline

Based on the incoming message and topic, the agent attempts to map the topic to a device and data item. Following the attempt to determine the device and data item, it will look at the first character. If it is a `{` it will create a JsonMessage and forward it to the `JsonMapper`.

At present the `JsonMapper` cannot decode the data, so it ignores it. Otherwise the payload is assumed to be SHDR and is passed to the SHDR mapper.

![MQTT Pipeline](/images/mqttpipeline.png)