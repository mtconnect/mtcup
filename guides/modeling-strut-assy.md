---
title: Modeling Technique - Strut Assembly Station
description: 
published: true
date: 2022-01-17T16:23:34.347Z
tags: 
editor: markdown
dateCreated: 2022-01-16T23:54:35.386Z
---

# Overview

This guide is a walk-through of modeling techniques based on a discussion in 'Industry 4.0 Community' Discord.

# Machine Operation

The 'Strut Assembly Station' accepts strut housing and rod assemblies of varying sizes and applies proper torque to rod end fittings mated by the operator.

![strut-assy-front.png](/strut-assy/strut-assy-front.png)

The operator inputs job information and production quantity into the human machine interface, presents a strut assembly, inserts end fittings, and cycles the machine.  At cycle end, a label is printed and applied with relevant job information, and the assembly is removed.

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

## Simulated Controller Inputs

- SIG_POWER - Station power supply switch.
- SIG_JOB - Operator job information input.
- SIG_PRESENT_PART - Operator action of presenting part to fixture, engaging part detection proximity sensors.
- SIG_REMOVE_PART - Operator action of removing part from fixture, disengaging part detection proximity sensors.
- SIG_OPERATOR_START - Operator action pressing cycle start button.
- SIG_INTERRUPT - Operator action pressing E-Stop, breaking light curtain, or internal machine fault during cycle.
- SIG_RESET_FAULT - Operator action resetting fault via E-Stop or reset button.

## Simulated Controller Outputs

- Devices/PowerSwitch (0,1)
- Devices/ResetPushButton (0,1)
- Devices/HousingPartPresent (0,1)
- Devices/RodPartPresent (0,1)
- Devices/ValveManifold (0,1)
- Devices/EStop (0,1)
- Data/Job
- Data/CycleTime
- State/off
- State/on+setup
- State/on+idle
- State/on+ready
- State/on+running
- State/on+cycle_ended
- State/on+faulted

## Iteration #1

```diagram
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSIxMjJweCIgaGVpZ2h0PSIyMjFweCIgdmlld0JveD0iLTAuNSAtMC41IDEyMiAyMjEiIGNvbnRlbnQ9IiZsdDtteGZpbGUgaG9zdD0mcXVvdDtlbWJlZC5kaWFncmFtcy5uZXQmcXVvdDsgbW9kaWZpZWQ9JnF1b3Q7MjAyMi0wMS0xN1QxNToxMTowMC40OTRaJnF1b3Q7IGFnZW50PSZxdW90OzUuMCAoTWFjaW50b3NoOyBJbnRlbCBNYWMgT1MgWCAxMF8xNV83KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBDaHJvbWUvOTcuMC40NjkyLjcxIFNhZmFyaS81MzcuMzYmcXVvdDsgdmVyc2lvbj0mcXVvdDsxNi4yLjAmcXVvdDsgZXRhZz0mcXVvdDsxSnhOUmF2RmRpN2lTY1hhX1VSTyZxdW90OyB0eXBlPSZxdW90O2VtYmVkJnF1b3Q7Jmd0OyZsdDtkaWFncmFtIGlkPSZxdW90O1NKc2JLb1VIVjNTYk5ZRlNqaklWJnF1b3Q7IG5hbWU9JnF1b3Q7UGFnZS0xJnF1b3Q7Jmd0OzNWWk5iK01nRVAwMXZsWW0xRkgydUp2MDQxSzFVZzdkSGhITTJrallSQVRIOXY3NjRnSTJtRlJ0OTlDdG1rczhqNWxoNXMzenlCbmUxdjJOSW9mcVRqSVEyU3BuZllaMzJXcUZFRWJtYjBRR2k2eUx3Z0tsNHN3NXpjQ2Uvd1VINWc1dE9ZTmo1S2lsRkpvZllwREtwZ0dxSTR3b0pidlk3WThVOGEwSFVrSUM3Q2tSS2ZySW1hNHN1aW55R2I4RlhsYitacFM3azVwNFp3Y2NLOEprRjBENEtzTmJKYVcyVDNXL0JUR1M1M214Y2Rldm5FNkZLV2owZXdKV051QkVST3Q2MjhHSlUzRFY2Y0czckdUYk1CaWo4Z3ovNmlxdVlYOGdkRHp0ekpBTlZ1bGFHQXVaUjVjVWxJYisxY0xRMUs3UkNjZ2F0QnFNaXd2d1hEcUZlTDY2bVc3a3NTcWdldTB3NGlaY1RvbG5Fc3lENCtFOEp6amhaQ3NicmFRUW9MNGFMNU95UG9PWXk0U1lCeVVwSEkvM2xMYktkUEQxZElNMm44aFBrZkNUMEFFTit6a3VJR00xc29HNC9aZ3IwN0FhZm8vR1JlSE5KK2Y1WXV6NjBITTNlS3ZuT2dnejFsTndNZ2VOaG8reGRRSkwxdDZDZHRPTGJCV0Y2RVhSUkpXZ2czMlNEaWRndnpoRHZzY1VDS0w1S1M3aTNFVGNEUStTbS9LbTJTK0hmNGtYUTdYRnU2aHdLeTRTNFR4T2hEZUxSTGJsSk5HTFFLYTIzNldaOWJmWHpQOVNBOHAvWEd6eTRJZk9iNGFQaXVPdHZNdE44czlhTWViOElXRGQ1ODhwZlBVTSZsdDsvZGlhZ3JhbSZndDsmbHQ7L214ZmlsZSZndDsiPjxkZWZzLz48Zz48cmVjdCB4PSIwIiB5PSIwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiBmaWxsPSJyZ2IoMjU1LCAyNTUsIDI1NSkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDMwcHg7IG1hcmdpbi1sZWZ0OiAxcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6IHJnYigwLCAwLCAwKTsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDAsIDAsIDApOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5EZXZpY2U8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iNjAiIHk9IjM0IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+RGV2aWNlPC90ZXh0Pjwvc3dpdGNoPjwvZz48cmVjdCB4PSIwIiB5PSI4MCIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgZmlsbD0icmdiKDI1NSwgMjU1LCAyNTUpIiBzdHJva2U9InJnYigwLCAwLCAwKSIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3QgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxMTBweDsgbWFyZ2luLWxlZnQ6IDFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPkNvbnRyb2xsZXI8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iNjAiIHk9IjExNCIgZmlsbD0icmdiKDAsIDAsIDApIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPkNvbnRyb2xsZXI8L3RleHQ+PC9zd2l0Y2g+PC9nPjxyZWN0IHg9IjAiIHk9IjE2MCIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgZmlsbD0icmdiKDI1NSwgMjU1LCAyNTUpIiBzdHJva2U9InJnYigwLCAwLCAwKSIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3QgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxOTBweDsgbWFyZ2luLWxlZnQ6IDFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPlByb2Nlc3NPY2N1cnJlbmNlPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjYwIiB5PSIxOTQiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5Qcm9jZXNzT2NjdXJyZW5jZTwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA2MCA4MCBMIDYwIDYwIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA1OS44IDE2MCBMIDU5LjggMTQwIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PC9nPjxzd2l0Y2g+PGcgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5Ii8+PGEgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMCwtNSkiIHhsaW5rOmhyZWY9Imh0dHBzOi8vd3d3LmRpYWdyYW1zLm5ldC9kb2MvZmFxL3N2Zy1leHBvcnQtdGV4dC1wcm9ibGVtcyIgdGFyZ2V0PSJfYmxhbmsiPjx0ZXh0IHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTBweCIgeD0iNTAlIiB5PSIxMDAlIj5UZXh0IGlzIG5vdCBTVkcgLSBjYW5ub3QgZGlzcGxheTwvdGV4dD48L2E+PC9zd2l0Y2g+PC9zdmc+
```


<details>
<summary>
Model (click to expand)		
</summary>

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

</details>

## Iteration #2

```diagram
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSI3MDFweCIgaGVpZ2h0PSIyNzJweCIgdmlld0JveD0iLTAuNSAtMC41IDcwMSAyNzIiIGNvbnRlbnQ9IiZsdDtteGZpbGUgaG9zdD0mcXVvdDtlbWJlZC5kaWFncmFtcy5uZXQmcXVvdDsgbW9kaWZpZWQ9JnF1b3Q7MjAyMi0wMS0xN1QxNTozMzozNy44ODNaJnF1b3Q7IGFnZW50PSZxdW90OzUuMCAoTWFjaW50b3NoOyBJbnRlbCBNYWMgT1MgWCAxMF8xNV83KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBDaHJvbWUvOTcuMC40NjkyLjcxIFNhZmFyaS81MzcuMzYmcXVvdDsgdmVyc2lvbj0mcXVvdDsxNi4yLjcmcXVvdDsgZXRhZz0mcXVvdDtaaFVzRzN2Wk5lRXNwdk1JbWxvayZxdW90OyB0eXBlPSZxdW90O2VtYmVkJnF1b3Q7Jmd0OyZsdDtkaWFncmFtIGlkPSZxdW90O2dZLVBZSGNKQVpqM2tZMXdNNHZpJnF1b3Q7IG5hbWU9JnF1b3Q7UGFnZS0xJnF1b3Q7Jmd0OzVabE5zNW93RklaL0RkczdCQVIxMmFwdE41M2VHUmR0bHd3Y0pUTkFuQkFVK3VzYlN2aElnalBxUlhPOVhaa2NUcExEODU1OG9lV3UwdklyRFE3eGR4SkJZamwyVkZydTJuSWNoRnpFZjJwTDFWaDh6MnNNZTRvajRkUWJ0dmdQQ0tNdHJBV09JSmNjR1NFSnd3ZlpHSklzZzVCSnRvQlNjcExkZGlTUlJ6MEVlOUFNMnpCSWRPdFBITEc0c1M0OHU3ZC9BN3lQMjVHUkxaNmtRZXNzREhrY1JPUTBNTGtieTExUlFsaFRTc3NWSkRXOGxrdlQ3c3VacDExZ0ZESjJTUU9uYVhBTWtrSzgyeHFPT0FRUkhhdmFWNmFreUNLb1c5bVcrL2tVWXdiYlF4RFdUMDljWkc2TFdacndHdUpGMFNsUUJ1WFp3RkQzdWp4UGdLVEFhTVZkUkFPM0pWVEowcDk2M3FoMWlRZXNmV0VMaE1UN3J1ZWVBaThJRU9OUVhBM0tpbVNNa2lRQmFoeU15c1Y1SUppWkJ1YVZraER5L0VjWUZwUy93VHRJSElXUDgwZytuc1puVytVTTB0dzRGVzA2UFJLTHIySFpKSHhacGpnMHpnVXRES2JMWEo5T0dSUjhqM2dIWUdhK1FUQ0xzWFdHOFpUQlIvTUxqRzhiSkxQVXAxSVdiWFk3em9hWTM1dlVWZWFoYU5vVHdvQ05CZ1N5NkZOOS91TzFqR1FnQTVCcFFZblpyN3I4NG9uYTc4R1RkVG1zVktMU2pBZVJkbnBVQVBLWVNFRkRHQnczZEtZRFp0NElzdFpHSVFuK3pRdnBrRHZDVVl6d1NqQ1BwWk5zS1N1MlZJUm80aFJ0aHVkSXBSdFhXVWZuU2o4c29IdGdXajljaktBYXVCMXFoL3g4dEl2UmFDOE5TbmJuaFdiNFByczYrcGNsSEhyU2hQTk1KcHc3bDBWQmFLS1VVMVAzVE1yZElyUitTM29Pb1dkR2hWYjJBdmRHblJXWnV4UGJIWFRXTDM3UG9iTnZVbWYxQkgycnp1cDh2bFRvYS9jUVo2WUViRiszaXlqK2I5OUc5RnYxVzdMdTBnd3l0aXljd1hsMXZ2anFEVVh0YU1LRlFiL1kvMWNTT1JOTjZYdEtwSDlrK01nU2VXajVNdmVuVVdtc3J6c0twWC8xK01oQytkNTBRbzMxTlpsUXZOci83ZEM0OTMvZXVKdS8mbHQ7L2RpYWdyYW0mZ3Q7Jmx0Oy9teGZpbGUmZ3Q7Ij48ZGVmcy8+PGc+PHJlY3QgeD0iMzAwIiB5PSIwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiBmaWxsPSJyZ2IoMjU1LCAyNTUsIDI1NSkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDMwcHg7IG1hcmdpbi1sZWZ0OiAzMDFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPkRldmljZTwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSIzNjAiIHk9IjM0IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+RGV2aWNlPC90ZXh0Pjwvc3dpdGNoPjwvZz48cmVjdCB4PSIwIiB5PSIxMTAiIHdpZHRoPSIxMjAiIGhlaWdodD0iNjAiIGZpbGw9InJnYigyNTUsIDI1NSwgMjU1KSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTQwcHg7IG1hcmdpbi1sZWZ0OiAxcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6IHJnYigwLCAwLCAwKTsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDAsIDAsIDApOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5Db250cm9sbGVyPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjYwIiB5PSIxNDQiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5Db250cm9sbGVyPC90ZXh0Pjwvc3dpdGNoPjwvZz48cmVjdCB4PSIwIiB5PSIyMTAiIHdpZHRoPSIxMjAiIGhlaWdodD0iNjAiIGZpbGw9InJnYigyNTUsIDI1NSwgMjU1KSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMjQwcHg7IG1hcmdpbi1sZWZ0OiAxcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6IHJnYigwLCAwLCAwKTsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDAsIDAsIDApOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5Qcm9jZXNzT2NjdXJyZW5jZTwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSI2MCIgeT0iMjQ0IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+UHJvY2Vzc09jY3VycmVuY2U8L3RleHQ+PC9zd2l0Y2g+PC9nPjxyZWN0IHg9IjMwMCIgeT0iMTEwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiBmaWxsPSJyZ2IoMjU1LCAyNTUsIDI1NSkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDE0MHB4OyBtYXJnaW4tbGVmdDogMzAxcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6IHJnYigwLCAwLCAwKTsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDAsIDAsIDApOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5TeXN0ZW1zPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjM2MCIgeT0iMTQ0IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+U3lzdGVtczwvdGV4dD48L3N3aXRjaD48L2c+PHJlY3QgeD0iMTYwIiB5PSIyMTAiIHdpZHRoPSIxMjAiIGhlaWdodD0iNjAiIGZpbGw9InJnYigyNTUsIDI1NSwgMjU1KSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMjQwcHg7IG1hcmdpbi1sZWZ0OiAxNjFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPkVsZWN0cmljPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjIyMCIgeT0iMjQ0IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+RWxlY3RyaWM8L3RleHQ+PC9zd2l0Y2g+PC9nPjxyZWN0IHg9IjQ0MCIgeT0iMjEwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiBmaWxsPSJyZ2IoMjU1LCAyNTUsIDI1NSkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDI0MHB4OyBtYXJnaW4tbGVmdDogNDQxcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6IHJnYigwLCAwLCAwKTsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDAsIDAsIDApOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5QbmV1bWF0aWM8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iNTAwIiB5PSIyNDQiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5QbmV1bWF0aWM8L3RleHQ+PC9zd2l0Y2g+PC9nPjxyZWN0IHg9IjU4MCIgeT0iMjEwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiBmaWxsPSJyZ2IoMjU1LCAyNTUsIDI1NSkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDI0MHB4OyBtYXJnaW4tbGVmdDogNTgxcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6IHJnYigwLCAwLCAwKTsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDAsIDAsIDApOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5Qcm90ZWN0aXZlPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjY0MCIgeT0iMjQ0IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+UHJvdGVjdGl2ZTwvdGV4dD48L3N3aXRjaD48L2c+PHJlY3QgeD0iMzAwIiB5PSIyMTAiIHdpZHRoPSIxMjAiIGhlaWdodD0iNjAiIGZpbGw9InJnYigyNTUsIDI1NSwgMjU1KSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMjQwcHg7IG1hcmdpbi1sZWZ0OiAzMDFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPkVuZEVmZmVjdG9yPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjM2MCIgeT0iMjQ0IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+RW5kRWZmZWN0b3I8L3RleHQ+PC9zd2l0Y2g+PC9nPjxwYXRoIGQ9Ik0gNjAgMTEwIEwgNjAgODAgTCAzNjAgODAgTCAzNjAgNjAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDM2MCAxMTAgTCAzNjAgODAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDYwIDIxMCBMIDYwIDE3MCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gMjIwIDIxMCBMIDIyMCAxOTAgTCAzNjAgMTkwIEwgMzYwIDE3MCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gMzYwIDE5MCBMIDY0MCAxOTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDM2MCAyMTAgTCAzNjAgMTkwIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA0OTkuNzYgMjEwIEwgNDk5Ljc2IDE5MCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gNjM5Ljc2IDIxMCBMIDYzOS43NiAxOTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48L2c+PHN3aXRjaD48ZyByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiLz48YSB0cmFuc2Zvcm09InRyYW5zbGF0ZSgwLC01KSIgeGxpbms6aHJlZj0iaHR0cHM6Ly93d3cuZGlhZ3JhbXMubmV0L2RvYy9mYXEvc3ZnLWV4cG9ydC10ZXh0LXByb2JsZW1zIiB0YXJnZXQ9Il9ibGFuayI+PHRleHQgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMHB4IiB4PSI1MCUiIHk9IjEwMCUiPlRleHQgaXMgbm90IFNWRyAtIGNhbm5vdCBkaXNwbGF5PC90ZXh0PjwvYT48L3N3aXRjaD48L3N2Zz4=
```

TODO: revise model

# Simulation

Controller and MTConnect output simulated using NodeRed.

[Live Flow](http://143.198.106.54:1880/)  
[Live User Interface](http://143.198.106.54:1880/ui)  
[Download Flows](/strut-assy/flows-strut-assy.json)


![strut-assy-1.gif](/strut-assy/strut-assy-1.gif)
