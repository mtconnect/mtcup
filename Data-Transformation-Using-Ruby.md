---
title: Data Transformation using Ruby
description: 
published: true
date: 2023-06-26T12:09:50.494Z
tags: 
editor: markdown
dateCreated: 2023-06-26T12:09:50.494Z
---

# Data Transformation using Ruby

The [Agent 2.0.0.2][agent_2-0-0-2] release introduced an embedded mruby scripting engine for dynamic transformation.

## Configuration

For data transformation using Ruby, add the following in the agent config file:

```
    Ruby {
      module = path/to/module.rb
    }
```

The module specified at the path given will be loaded.

The current functionality is limited to the pipeline transformations from the adapters. Future changes will include adding sources and sinks.

[ruby_plugin_module_example]: /Ruby-Plugin-Module-Example "wikilink"

## Examples

### Template for a Ruby Transformation Module

```ruby
# You may replace the name of the class <UseCaseName> with the custom use case at hand.
class UseCaseName < MTConnect::RubyTransform
  
  # Constructor method: Remains unedited
  def initialize(name, filter)
    @cache = Hash.new
    super(name, filter)
  end

  # Tranformation method: TO BE EDITED
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
  
  # The second argument may differ depending upon use case. It can be :Sample, :Event or :Condition. : TO BE EDITED
  trans = UseCaseName.new('UseCaseName', :Sample)
  
  puts trans
  pipe.splice_before('DeliverObservation', trans)
end
```

#### Template with Transformation count

```ruby
# You may replace the name of the class <UseCaseName> with the custom use case at hand.
class UseCaseName < MTConnect::RubyTransform
  
  # Constructor method: Remains unedited
  def initialize(name, filter)
    @cache = Hash.new
    super(name, filter)
  end

  # Class Variable to keep track of the number of times the transform method is called: MAYBE UPDATED depending upon the what type of count is relevant to the use case.
  @@count = 0

  # Tranformation method: TO BE EDITED
  def transform(obs)
    @@count += 1
    if @@count % 10000 == 0
      puts "---------------------------"
      puts ">  #{ObjectSpace.count_objects}"
      puts "---------------------------"
    end
 
    # Transformation code goes here.
    # Please see examples listed below.

    forward(obs)
  end
end

# Splicing the pipeline of each data source for transformation
MTConnect.agent.sources.each do |s|
  pipe = s.pipeline
  puts "Splicing the pipeline"
  
  # The second argument may differ depending upon use case. It can be :Sample, :Event or :Condition. : TO BE EDITED
  trans = UseCaseName.new('UseCaseName', :Sample)
  
  puts trans
  pipe.splice_before('DeliverObservation', trans)
end
```

### Fix `Execution` state of a device

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

[agent_2-0-0-2]: https://github.com/mtconnect/cppagent/releases/tag/v2.0.0.2