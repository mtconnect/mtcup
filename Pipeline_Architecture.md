---
title: Reference Agent Pipeline Architecture
description: Internal architecture of the MTConnect Agent to extend data transformations
published: true
date: 2023-01-23T10:34:46.172Z
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

## Pipelines

A pipeline is references has a start transform and a build method to construct it. All data source must have a pipeline to deliver data to the agent, sometimes the pipeline is simple, just a delivery transform, as with the loopback pipeline.

Pipelines are also capable of mutating themselves when plugins or scripts are loaded. There are many methods to modify the pipeline. 

* `spliceBefore(const std::string target, TransformPtr transform)`: Adds this transform directly before the `target` transform. Will replace all references in the lists of next transforms.
  * ruby: `splice_before(target, transform)`
* `spliceAfter(const std::string target, TransformPtr transform)`: Adds this transform immediately after the `target` transform. Will replace all references in the lists of next transforms.
  * ruby: `splice_after(target, transform)`
* `replace(const std::string target, TransformPtr transform)`: replaces the `target` transform.
  * ruby: `replace(target, transform)`
* `firstAfter((const std::string target, TransformPtr transform)`: Inserts the transform at the beginning of the next list of `target` transform's.
  * ruby: `first_after(target, transform)`
* `lastAfter((const std::string target, TransformPtr transform)`: Inserts the transform at the end of the next list of `target` transform's.
  * ruby: `last_after(target, transform)`

## Entities used in the pipelines

![Pipeline Entities](/images/pipelineentities.png)

## SHDR Adapter Pipeline

![SHDR Pipeline](/images/shdrpipeline.png)

## MQTT Adapter Pipeline

Based on the incoming message and topic, the agent attempts to map the topic to a device and data item. Following the attempt to determine the device and data item, it will look at the first character. If it is a `{` it will create a JsonMessage and forward it to the `JsonMapper`.

At present the `JsonMapper` cannot decode the data, so it ignores it. Otherwise the payload is assumed to be SHDR and is passed to the SHDR mapper.

![MQTT Pipeline](/images/mqttpipeline.png)

## Ruby Transforms

When using the mruby embedded language, one can write dynamic scripted transformation to support quick corrections or protocol transformations from JSON representations via MQTT. 

An example of a ruby transform takes 

```ruby
class MapMqttData < MTConnect::RubyTransform
  def initialize
    super("MapMqttData")
    guard = lambda { |e|
      p e.topic
      if e.topic =~ /^\/data/
        return :RUN
      else
        return :CONTINUE
      end
    }
  end
  
  def transform(entity)
    puts "*** received #{entity.name} with value: #{entity.value}"
    value = "UNAVAILABLE"    
    case entity.value
    when "1"
      value = "READY"
      
    when "2"
      value = "ACTIVE"
    end

    puts "**** Setting execution to #{value}"

    puts "Creating timestamped"
    obs = MTConnect::Timestamped.new("Timestamped", {VALUE: value })
    obs.timestamp = Time.now
    obs.tokens = ["execution", value]
    forward(obs)
  end
end

MTConnect.agent.sources.each do |s|
  if s.name =~ /^mqtt/
    MTConnect::Logger.info "Splcing token mapper for #{s.name}"
    pipe = s.pipeline
    trans = MapMqttData.new()
    pipe.first_after("Start", trans)
    mapper, = pipe.find("ShdrTokenMapper")
    trans.bind(mapper)
  end
end
```

Another example:

```ruby
MTConnect::Logger.info "Declaring class MapMqttData"

class MapMqttData < MTConnect::RubyTransform
  def initialize
    super("MapMqttData")
    guard = lambda { |e|
      if e.name == "JsonMessage"
        return :RUN
      else
        return :CONTINUE
      end
    }
  end
  
  def transform(entity)
    # {"dataItemId":"execution","timestamp":"2023-01-11T21:36:06.371Z","value":"STOPPED"}
    puts "*** received #{entity.name} with value: #{entity.value}"
    data = eval entity.value
    p data

    data_item = MTConnect.agent.default_device.data_item(data[:dataItemId])
    if !data_item
      MTConnect::Logger.warning "cannot find data item for #{data[:dataItemId]}"
      return nil
    end

    cat = data_item['category']
    puts "DataItem category: #{cat}"
    
    obs = nil
    case cat
    when 'EVENT'
      obs = MTConnect::Event.new(data_item, {"VALUE" => data[:value]}, Time.now) # data[:timestamp])

    when 'SAMPLE'
      obs = MTConnect::Sample.new(item, {"VALUE" => data[:value]}, data[:timestamp])

    else
      MTConnect::Logger.warning "Not doing conditions at the moment"
    end
    
    return forward(obs) if obs
    nil
  end
end



MTConnect.agent.sources.each do |s|
  if s.name =~ /^mqtt/
    MTConnect::Logger.info "Splcing token mapper for #{s.name}"
    pipe = s.pipeline
    trans = MapMqttData.new()
    pipe.replace("JsonMapper", trans)
  end
end
```