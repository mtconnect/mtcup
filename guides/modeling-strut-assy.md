---
title: Modeling Technique - Strut Assembly Station
description: 
published: true
date: 2022-01-17T00:13:41.985Z
tags: 
editor: markdown
dateCreated: 2022-01-16T23:54:35.386Z
---

# Overview

This guide is a walk-through of modeling techniques based on a discussion in 'Industry 4.0 Community' Discord.

# Machine Operation

The 'Strut Assembly Station' accepts strut housing and rod assemblies of varying sizes and applies proper torque to rod end fittings mated by the operator.

![strut-assy-front.png](/strut-assy/strut-assy-front.png)

The operator inputs a job information and production quantity into the human machine interface, presents a strut assembly, inserts end fittings, and cycles the machine.  At cycle end, a label is printed and applied with relevant job information, and the assembly is removed.

![strut-assy-view.png](/strut-assy/strut-assy-view.png)

# Model Planning

## Available Data

Below lists the available data points from the Rockwell logic controller.

![strut-assy-tags.png](/strut-assy/strut-assy-tags.png)

After some discussion we arrived the the following draft of the controller states.

![strut-assy-sm.png](/strut-assy/strut-assy-sm.png)

# Model Design

Using the [XState](https://xstate.js.org/) library we created the controller state machine for later simulation and the MTConnect model itself.

![strut-assy-xstate-vis.png](/strut-assy/strut-assy-xstate-vis.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>

<MTConnectDevices 
    xmlns:m="urn:mtconnect.org:MTConnectDevices:1.7" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns="urn:mtconnect.org:MTConnectDevices:1.7" 
    xsi:schemaLocation="urn:mtconnect.org:MTConnectDevices:1.7 https://schemas.mtconnect.org/schemas/MTConnectDevices_1.7.xsd">
  <Header 
    creationTime="2021-09-20T00:00:00+00:00" 
    deviceModelChangeTime="2021-09-20T00:00:00+00:00" 
    sender="localhost" instanceId="12345678" bufferSize="131072" assetBufferSize="1000" assetCount="0" version="1.7.0.3" />
  <Devices>
    <Device id="strut-assy" name="Strut Assembly" uuid="sa1" sampleInterval="1000">
      <DataItems>
        <!--
          https://model.mtconnect.org/#Architecture__efa904df-e614-4dde-81d4-3d9d41ea5596
          origin: Devices\PowerSwitch, Devices\PowerSupply

        -->
        <DataItem category="EVENT" id="avail" name="availability" type="AVAILABILITY" />

        <!--
          https://model.mtconnect.org/#Architecture__f43a20be-9afb-4479-8d7a-a0d87a490143
          values: 
            UNAVAILABLE: initial
            SETUP: setup state
            PRODUCTION: not setup state
            TEARDOWN: idle state && job_completed=true
        -->
        <DataItem category="EVENT" id="func" name="functional_mode" type="FUNCTIONAL_MODE" />

      </DataItems>
      <Components>
        <Controller id="controller" name="controller">
          <Compositions>

            <!-- safety -->
            <Composition id="comp-estop" type="SWITCH"/>
            <Composition id="comp-light-curtain" type="SENSING_ELEMENT"/>
            <Composition id="comp-safety-relay" type="SWITCH"/>

            <!-- work holding -->
            <Composition id="comp-housing-clamp" type="CLAMP"/>
            <Composition id="comp-valve-manifold" type="VALVE"/>
            <Composition id="comp-housing-part-present" type="SENSING_ELEMENT"/>
            <Composition id="comp-rod-part-present" type="SENSING_ELEMENT"/>

            <!-- other -->
            <Composition id="comp-reset-pushbutton" type="SWITCH"/>
            <Composition id="comp-power-switch" type="SWITCH"/>
            <Composition id="comp-power-supply" type="POWER_SUPPLY"/>

          </Compositions>

          <DataItems>
            <!--
              origin: Devices/EStop
            -->
            <DataItem category="EVENT" id="estop" type="COMPOSITION_STATE" subType="SWITCHED" compositionId="comp-estop"/>

            <!--
              origin: Devices/LightCurtain
            -->
            <DataItem category="EVENT" id="light-curtain" type="COMPOSITION_STATE" subType="ACTION" compositionId="comp-light-curtain"/>

            <!--
              origin: Devices/SafetyRelay
            -->
            <DataItem category="EVENT" id="safety-relay" type="COMPOSITION_STATE" subType="SWITCHED" compositionId="comp-safety-relay"/>

            <!--
              origin: Devices/HousingClamp
            -->
            <DataItem category="EVENT" id="housing-clamp" type="COMPOSITION_STATE" subType="MOTION" compositionId="comp-housing-clamp"/>

            <!--
              origin: Devices/ValveManifold
            -->
            <DataItem category="EVENT" id="valve-manifold" type="COMPOSITION_STATE" subType="ACTION" compositionId="comp-valve-manifold"/>

            <!--
              origin: Devices/HousingPartPresent
            -->
            <DataItem category="EVENT" id="housing-part-present" type="COMPOSITION_STATE" subType="ACTION" compositionId="comp-housing-part-present"/>

            <!--
              origin: Devices/RodPartPresent
            -->
            <DataItem category="EVENT" id="rod-part-present" type="COMPOSITION_STATE" subType="ACTION" compositionId="comp-rod-part-present"/>

            <!--
              origin: Devices/ResetPushbutton
            -->
            <DataItem category="EVENT" id="reset-pushbutton" type="COMPOSITION_STATE" subType="SWITCHED" compositionId="comp-reset-pushbutton"/>

            <!--
              origin: Devices/PowerSwitch
            -->
            <DataItem category="EVENT" id="power-switch" type="COMPOSITION_STATE" subType="SWITCHED" compositionId="comp-power-switch"/>

            <!--
                origin: Devices/EthernetSwitch
                origin: Devices/HousingNutRunner
                origin: Devices/IOBlock
                origin: Devices/LabelPrinter
                origin: Devices/LabelPresent
                origin: Devices/LabelScanner
                origin: Devices/PLC

            -->

            <!--<DataItem category="EVENT" id="power-supply" type="COMPOSITION_STATE" subType="SWITCHED" compositionId="comp-power-supply"/>-->

            <!--
              https://model.mtconnect.org/#Architecture__4e32a979-4ce0-40d2-b725-4488705ce7c5

            -->
            <DataItem category="SAMPLE" id="uptime" type="EQUIPMENT_TIMER" subType="POWERED" />

            <!--
              https://model.mtconnect.org/#Architecture__550918a4-def4-4def-a2db-30d85c4e066e

            -->
            <DataItem category="EVENT" id="execution" type="EXECUTION" />

            <!--
              https://model.mtconnect.org/#Architecture__b5a9fb61-fd07-4e9d-a7a5-83ead254c35d

            -->
            <DataItem category="CONDITION" id="system-condition" name="condition" type="SYSTEM" />

            <!--
              https://model.mtconnect.org/#Architecture__a2153eb8-f351-45a5-8b3d-5f7bc48aba8f
              origin: Status/CycleCount

            -->
            <DataItem category="SAMPLE" id="cycle-count" type="CYCLE_COUNT" subType="ALL" />

            <!--
              https://model.mtconnect.org/#Architecture__6557d357-6c9a-43e2-ac74-5b1037f62770
              origin: Devices/HousingPartPresent, Devices/RodPartPresent

            -->
            <DataItem category="EVENT" id="part-present" type="PART_DETECT" />
            
          </DataItems>

          <Components>
            <ProcessOccurrence id="process_occurrence">
              <DataItems>
                <!--
                  https://model.mtconnect.org/#Architecture__ab12b881-f1da-4f5a-b91e-2d25b8a5a03e
                  origin: Production/WorkOrderNumber

                -->
                <DataItem category="EVENT" id="job-number" type="PROCESS_AGGREGATE_ID" subType="ORDER_NUMBER" />

                <!--

                  origin: Production/PartNumber
                  origin: Production/CustomerNumber

                -->

                <!--
                  https://model.mtconnect.org/#Architecture__686755ea-ff11-4439-91a6-0a3b81216302
                  origin: work order info arrival

                -->
                <DataItem category="EVENT" id="job-start" type="PROCESS_TIME" subType="START" />

                <!--
                  https://model.mtconnect.org/#Architecture__ef33cc84-1908-412a-bcf1-72ad572bf837
                  origin: part-count.remaining == part-count.target

                -->
                <DataItem category="EVENT" id="job-end" type="PROCESS_TIME" subType="COMPLETE" />

                <!--
                  https://model.mtconnect.org/#Architecture__be00b027-3408-4419-bcbe-e6fbf0814d20

                -->
                <DataItem category="EVENT" id="proc-state" type="PROCESS_STATE" />

                <!--
                  https://model.mtconnect.org/#Architecture__683746cd-c233-4e1d-8f37-d11f8d0856fd
                  origin: Status/CycleTime

                -->
                <DataItem category="SAMPLE" id="cycle-time" type="PROCESS_TIMER" subType="PROCESS" units="SECOND" />

                <!--
                  https://model.mtconnect.org/#Architecture__a2c0c83f-b61b-4944-8bfa-d1d5385d62d3
                  origin: Status/Operator

                -->
                <DataItem category="EVENT" id="operator" type="USER" subType="OPERATOR"/>

                <!--

                  origin: Production/Quantity
                -->

                <DataItem category="EVENT" id="pc-target" type="PART_COUNT" subType="TARGET" />

                <DataItem category="EVENT" id="pc-complete" type="PART_COUNT" subType="COMPLETE" />

                <DataItem category="EVENT" id="pc-remain" type="PART_COUNT" subType="REMAINING" />

                <DataItem category="EVENT" id="pc-failed" type="PART_COUNT" subType="FAILED"/>

                <DataItem category="EVENT" id="pc-good" type="PART_COUNT" subType="GOOD"/>

              </DataItems>
            </ProcessOccurrence>

          </Components>
        </Controller>
      </Components>
    </Device>
  </Devices>
</MTConnectDevices>
```

# Simulation

Controller and MTConnect output simulated using NodeRed.

[Flow](http://143.198.106.54:1880/)
[User Interface](http://143.198.106.54:1880/ui)

![strut-assy-1.gif](/strut-assy/strut-assy-1.gif)
