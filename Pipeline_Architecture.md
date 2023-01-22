---
title: Reference Agent Pipeline Architecture
description: Internal architecture of the MTConnect Agent to extend data transformations
published: true
date: 2023-01-22T18:51:35.850Z
tags: 
editor: markdown
dateCreated: 2023-01-22T18:46:52.518Z
---

# MTConnect Agent Pipeline Architecture

## Overview

## Entities used in the pipelines

![Pipeline Entities](/images/pipelineentities.png)

## SHDR Adapter Pipeline

![SHDR Pipeline](/images/shdrpipeline.png)

## MQTT Adapter Pipeline

Based on the incoming message and topic, the agent attempts to map the topic to a device and data item. Following the attempt to determine the device and data item, it will look at the first character. If it is a `{` it will create a JsonMessage and forward it to the `JsonMapper`.

At present the `JsonMapper` cannot decode the data, so it ignores it. Otherwise the payload is assumed to be SHDR and is passed to the SHDR mapper.

![MQTT Pipeline](/images/mqttpipeline.png)