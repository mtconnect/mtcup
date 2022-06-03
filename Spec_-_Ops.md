---
title: Spec-Ops
description: 
published: true
date: 2022-06-03T12:54:31.142Z
tags: 
editor: markdown
dateCreated: 2022-06-03T12:54:31.142Z
---

# Spec-Ops

Standards-based Platform for Enterprise Communication enabling Optimal Production and Self-awareness

**DMDII Project Call 15-03**
PARC
System Insights
ITAMCO
MTConnect Institute

## Overview

There is significant complexity in connecting and configuring heterogeneous machine tools, PLCs, sensors and devices on the shop floor. On-site process data and information is disconnected from higher-level decision making systems such as Enterprise Resource Planning (ERP) software. Disconnect is largely due to the lack of standardized communication.

Current ERP and Manufacturing Execution Systems (MES) therefore provide little to no support for dynamic planning, scheduling, or optimizing equipment utilization; nor do they support the needs of contract manufacturers as the industry moves towards flexible, agile design and manufacturing services to accommodate a wider variety of customer requirements. 

Self-awareness where flexible systems can communicate, reconfigure, and describe themselves is a possible solution. The key to achieving this vision is the seamless exchange of information.



## Spec-Ops Architecture

![specops.png](/images/specops.png)

Here is a model of how the information flows among the system components when dynamic planning and scheduling are incorporated. 

The MTConnect Agent forms the core of the system, with all of the data exchange passing through that entity. 

ERP / MES data passes through a gateway into the MTConnect Agent. 
Near real-time machine tool data comes from the shop floor into the Agent as well. The analytics engine uses real-time MTConnect streaming data to produce intervals that are sent to the Agent; this data is used by the CBM gateway to calculate CBM KPIs. The planner uses information about the jobs (STEP files) along with machine tool capabilities outputs plans in the MTConnect standard using the Process model; it then sends these to the MTConnect agent. 

The scheduler receives the plans from the agent, and uses those along with the job requirements, machine specifications and availability, and CBM data to generate schedules as MTConnect Part instances. These Part models go through the Agent as well and from there back to the MES / ERP for access by shop personnel.

This enables integrated predictive process control, dynamic scheduling, and process analytics with significantly reduced implementation cost.

