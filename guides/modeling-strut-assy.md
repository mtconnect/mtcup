---
title: Modeling Technique - Strut Assembly Station
description: 
published: true
date: 2022-01-16T23:54:35.386Z
tags: 
editor: markdown
dateCreated: 2022-01-16T23:54:35.386Z
---

# Overview

This guide is a walk-through of modeling techniques based on a discussion in 'Industry 4.0 Community' Discord.

# Machine Operation

The 'Strut Assembly Station' accepts strut housing and rod assemblies of varying sizes and applies proper torque to rod end fittings mated by the operator.

![strut-assy-front.png](/strut-assy/strut-assy-front.png)

The operator inputs a job number and production quantity into the human machine interface, presents a strut assembly, inserts end fittings, and cycles the machine.  At cycle end, a label is printed and applied with relevant job information, and the assembly is removed.

![strut-assy-view.png](/strut-assy/strut-assy-view.png)