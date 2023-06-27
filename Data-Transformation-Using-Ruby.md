---
title: Data Transformation Using Ruby
description: The Agent 2.0.0.2 release introduced an embedded mruby scripting engine for dynamic transformation.
published: true
date: 2023-06-27T18:11:11.114Z
tags: 
editor: markdown
dateCreated: 2023-06-26T12:09:50.494Z
---


The Agent includes an embedded mruby scripting engine to enable dynamic transformation.

# Data Transformation Pipeline Architecture

The MTConnect Agent uses a data transformation pipeline to provide a flexible and mutable mechanism for processing incoming data from various sources and allowing for reusability of common transform components. Learn more about the architecture at [MTConnect Agent Pipeline Architecture][pipeline_architecture].

# Configuration

For data transformation using Ruby, add the path to the Ruby module in the agent config file as shown below:

```
    Ruby {
      module = path/to/module.rb
    }
```

The module specified at the path given will be loaded.

The current functionality is limited to the pipeline transformations from the adapters. Future changes will include adding sources and sinks.

Following examples will elucidate how to write a Ruby Transform module.

# Examples

## Template

```ruby
# You may replace the name of the class <UseCaseName> with the custom use case at hand.
class UseCaseName < MTConnect::RubyTransform
  
  # Constructor method
  def initialize(name, filter)
    @cache = Hash.new
    super(name, filter)
  end

  # Tranformation method
  def transform(obs)
 
    # Transformation code goes here.
    # Please see examples listed below.

    forward(obs)
  end
end

# Splicing the pipeline of each data source for transformation
MTConnect.agent.sources.each do |s|
  pipe = s.pipeline
  puts "Splicing the pipeline"
  
  # The arguments may differ depending upon the initialization. See examples below to see how
  trans = UseCaseName.new('UseCaseName')
  
	# The method called to modify the pipeline may differ depending upon the usecase. See examples below to see how
	pipe.splice_before('DeliverObservation', trans)
end
```

## Fix `Execution` state of a device

Scenario: The adapter outputs `Execution` state `NOT_READY` as `IDLE`, and `WAIT` as `WAITING` instead. `IDLE` and `WAITING` are not MTConnect semantics. Hence can be transformed to `NOT_READY` and `WAIT` as shown below.

```ruby
class FixExecution < MTConnect::RubyTransform
  def initialize(name, filter)
    @cache = Hash.new
    super(name, filter)
  end

  @@count = 0
  def transform(obs)
    @@count += 1
    if @@count % 10000 == 0
      puts "---------------------------"
      puts ">  #{ObjectSpace.count_objects}"
      puts "---------------------------"
    end
    
    # Get dataItemId of the observation
    dataItemId = obs.properties[:dataItemId]

    # check if the dataitemId is that of `Execution` observation
    if dataItemId == 'execution'
      # get the value of `Execution` observation
      @cache[dataItemId] = obs.value 
      
      # get the device info
      device = MTConnect.agent.default_device 
      
      # get the `Execution` dataitem from the device
      execution = device.data_item(dataItemId)
      
      # Using case statement to create and forward transformed values
      case @cache[dataItemId]
      when 'IDLE'
        # creating and forwarding new observation with value NOT_READY isntead of IDLE
        newobs = MTConnect::Observation.new(execution, 'NOT_READY')
        forward(newobs)
      
      when 'WAITING'
        newobs = MTConnect::Observation.new(execution, 'WAIT')
        forward(newobs)
      else
        # Forwarding original Execution observations only if no transformation required
        forward(obs)
      end
    else
      # Forwarding observations that are not Execution
      forward(obs)
    end
  end
end
    
MTConnect.agent.sources.each do |s|
  pipe = s.pipeline
  puts "Splicing the pipeline"
  # Updated the dataitem type to :Event
  trans = FixExecution.new('FixExecution', :Event)
  puts trans
  pipe.splice_before('DeliverObservation', trans)
end
```

## MQTT Pipeline: Fix `Execution` state of a Device

When using the mruby embedded language, one can write dynamic scripted transformation to support quick corrections or protocol transformations from JSON representations via MQTT. 

An example of a ruby transform takes some data with the topic data and converts `1` to `READY` and `2` to `ACTIVE`. The transform is added as the first transform after the `Start` (the first transform). 

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

The second example is interprestation of MQTT data. This replaces the dummy `JsonMapper` and the guard runs only on JsonMessages. The data in Json format is easily converted to a ruby Hash by just evaluating it. 

```ruby
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

    if obs
      forward(obs)
    else
      nil
    end
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





[agent_2-0-0-2]: https://github.com/mtconnect/cppagent/releases/tag/v2.0.0.2

[pipeline_architecture]: /Pipeline_Architecture "wikilink"