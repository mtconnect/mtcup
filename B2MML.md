---
title: B2MML
description: 
published: true
date: 2022-06-05T11:03:16.591Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:30:32.989Z
---

## Overview of B2MML Relationship with MTConnect

In June 2017, MESA (Manufacturing Enterprise Solutions Association
International) and the MTConnect Institute signed a memorandum of
understanding to provide a mechanism for MESA and MTConnect to
collaborate to extend the reach of their existing manufacturing
information data modeling standards and implementation technologies in
order to:

* Define the interaction between existing standards from each organization to provide a platform for improved manufacturing technology interoperability.

* Provide a forum for the exchange of information to support future continuous improvement of standards and specifications overseen by each body.

* Provide a mechanism for the exchange of insights, identification of overlaps, and harmonization of the works of both organizations; where appropriate.

* Provide a roadmap for implementers to leverage the capabilities of the standards and specifications of both bodies.

MESA is a not-for-profit global community of companies and academia
encompassing a wide array of expertise focused on improving and
extending the capabilities of the manufacturing environment through the
application of advanced information technologies and world-class
management practices. <http://www.mesa.org/>

One of the standards developed by MESA is the B2MML Standard. B2MML
(Business To Manufacturing Markup Language) is based on the ANSI/ISA-95,
Enterprise-Control System Integration, family of standards (ISA-95).
These standards as also known internationally as IEC/ISO 62264. B2MML
provides an XML representation of the ISA-95 standard. The B2MML
Standard is the specific technology provided by MESA that is the focus
of this document. <http://www.mesa.org/en/B2MML.asp>

The outcome of the agreement is a document called MTConnect-B2MML
Companion Specification. The MTConnect-B2MML Companion Specification is
an open-source resource.

While much of the information covered by the B2MML specification is
outside the scope of the MTConnect Standard, there is significant
opportunity to improve the capabilities of manufacturing software
systems through the integration of these two manufacturing standards.

The MTConnect-B2MML companion specification describes a methodology for:

* Providing a platform for managing and transporting B2MML documents between software applications within a manufacturing information system as an MTConnect Asset document.

* Define the mapping of data types that are common to both standards to support interoperability, harmonization, and      consistency for the flow of information from the shop floor to higher level software systems and from those systems down to the shop floor.
      
The information presented in this Companion Specification is non-proprietary; meaning it is built on open standards, backed  by both MESA and the MTConnect Institute whom together represent hundreds of companies, individuals, government organizations and nonprofits all working toward the goal of increased productivity in the manufacturing arena.

## Capabilities and Application of B2MML Represented as MTConnect Assets

B2MML provides standard data models for creating XML documents that
represent information from manufacturing management systems (MES, ERP,
etc.) to shop floor execution systems. It also provides a standard
structure for creating XML documents that encapsulate information
collected during the production process and presents that information in
a standard document format to higher level software systems.

B2MML and MTConnect complement each other. B2MML provides a mechanism
for structuring manufacturing information in a standardize document
format. MTConnect provides a mechanism for managing those documents
and/or moving them between software systems where the information is
required.

B2MML provides a wide variety of data models to support different types
of information. As part of this cooperative effort between MESA and
MTConnect, the following information models were initially of specific
interest:

* Product Definition

* Production Schedule

MTConnect provides specific functionalities that support the
inter-operability with other standards like B2MML. These are:

* Semantic data models for representing information relating to the shop floor that is expressed in document form.

* Definition and Reference Implementation software for an MTConnect Agent which stores and organizes information collected from a manufacturing operation. This Agent also produces structured documents containing the information collected for consumption by client software applications.

* Extensibility that allows an implementer to expand the functionality of an MTConnect implementation to include additional content.

MTConnect Assets provides a mechanism for representing complex information associated with manufacturing operations in electronic document form. Examples of types of information that could be modeled as MTConnect Assets include:

* Description of Cutting Tools

* Electronic Documents such as Maintenance Manuals, Test Results, Operator Instructions, etc.

* Description of Manufacturing Processes

* Routings to define the movement of parts through the manufacturing process

* Part Genealogy

* Quality Inspection Data

The electronic documents representing MTConnect Assets are encoded using XML. As such, this provides a straightforward mechanism for integrating with B2MML documents that are also encoded using XML.

Since B2MML addresses a wide variety of data models to support different types of information within the manufacturing environment and MTConnect represents a wide variety of data types collected from the manufacturing environment, the integration between these two standards can be an ever-growing effort with a variety of solutions. As such, the information provided here is intended to lay the groundwork for ongoing integration between these standards.

An initial set of sample implementations are provided below to the open source community with the expectation that the community will continue to grow and expand the implementation examples and contribute their work back to the community through this MTCUP website.

The initial sample implementations were developed as part of the DMDII sponsored SPEC-OPS project.

## Integration of B2MML Documents into an MTConnect Asset

The main reason for integrating B2MML documents with MTConnect Assets is to provide a common mechanism for identifying, managing, storing, and transporting both B2MML documents and other MTConnect Assets (Parts, Processes, Cutting Tools, Files, etc.) in a common format and utilizing a common set of software tools and technologies.

Since MTConnect is an extensible standard, an integrator can add additional content to an MTConnect implementation by extending the schema associated with any of the MTConnect Information Models. In this case, the MTConnect Assets Information Model would be extended to support additional Asset types – one for each of the B2MML document types that the integrator wishes to support in a system.

The integration of B2MML documents as a type of MTConnect Asset is a relatively straightforward process. B2MML and MTConnect encode documents using XML. XML documents are validated based upon a schema that is designed for each document type. XML has a useful feature that allows different parts of a document to be defined using different schemas.
This feature allows an implementer to extend the MTConnect Assets schema for a new type of MTConnect Asset (B2MML) by defining a relatively small extension to the MTConnect Assets schema. This additional content provides identity and some basic information about the B2MML document. It is then possible to embed the entire B2MML document into the MTConnect Asset by merely identifying it and associating the B2MML schema and namespace for that document. By doing this, a software application can request and interpret MTConnect Assets representing B2MML information and then further interpret the B2MML content since the reference to the schema for that content is embedded into the MTConnect Asset document.

To accomplish this integration, the MTConnect Schema and namespace must be extended. The attached file represents a method for extending the MTConnect Schema and namespace to support two types of B2MML documents - Product Definition and Production Schedule.

## B2MML Product Definition Document as an MTConnect Asset

The following figure demonstrates how a B2MML Product Definition
document can be embedded into an MTConnect Asset to create a B2MML
Product Definition Asset type.

A new MTConnect Asset type called B2MMLProductDefinition is created.
MTConnect provides a basic structure (“wrapper”) which defines critical
information for identifying and managing documents based upon this new
Asset type.

The XML element called b2mml:ProductDefinition represents the content of
the original B2MML file. This content is defined by a specific B2MML
schema. An example of that schema would be
<http://www.mesa.org/xml/B2MML-V0600> .

<File:B2MML> Product Definition Tree.png|B2MML Product Definition File
as an Asset

The resulting MTConnect Assets document including a B2MML Product
Definition type Asset is as follows:

```XML
<?xml version="1.0" encoding="UTF-8"?>

<MTConnectAssets xsi:schemaLocation="urn:mtconnect.org:B2MML:1.3 /schemas/B2MML_1.3.xsd" xmlns:b="urn:mtconnect.org:B2MML:1.3" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ns="urn:mtconnect.org:MTConnectAssets:1.3" xmlns:m="urn:mtconnect.org:MTConnectAssets:1.3">
    <Header assetCount="132" assetBufferSize="10240" version="1.4.0.3" instanceId="1505504796" sender="ubuntu" creationTime="2017-10-12T13:50:35Z"/>
    <Assets>
        <B2mmlProductDefinition assetId="9ede2e6c-f2f2-55fb-ab60-f7ae2a21ad17" deviceUuid="itamco_Haas_1c2bc0" timestamp="2017-09-15T19:46:49.679251Z">
            <ProductDefinition ns="http://www.mesa.org/xml/B2MML-V0600">
                <ID>TEST-00003 </ID>
                <Description>Base, Part 1a, Rev. - Base, Part
                1a</Description>
                <ProductSegment>
                    <ID>TEST-00003 </ID>
                </ProductSegment>
            </ProductDefinition>
        </B2mmlProductDefinition>
        <B2mmlProductDefinition assetId="42078e08-169a-5183-ad38-78f2daa0593c" deviceUuid="itamco_Haas_1c2bc0" timestamp="2017-09-15T19:46:49.242110Z">
            <ProductDefinition ns="http://www.mesa.org/xml/B2MML-V0600">
                <ID>TEST-00004 </ID>
                <Description>Rear Mount, HV, Left, Rev. - Rear, HV,
                Mount, Left</Description>
                <ProductSegment>
                    <ID>TEST-00004/ID\>
                </ProductSegment>
            </ProductDefinition>
        </B2mmlProductDefinition>
    </Assets>
</MTConnectAssets>
```

## B2MML Production Schedule Document as an MTConnect Asset

The following figure demonstrates how a B2MML Production Schedule document can be embedded into an MTConnect Asset to create a B2MML Production Schedule Asset type.

A new MTConnect Asset type called B2MMLProductionSchedule is created.   MTConnect provides the same basic “wrapper” for all B2MML documents represented as MTConnect Assets.

<gallery>
[File:B2MML-Production-Schedule-File.png|B2MML](File:B2MML-Production-Schedule-File.png%7CB2MML)
Production Schedule as an Asset

</gallery>

The resulting MTConnect Assets document containing a B2MML Production
Schedule would be:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<MTConnectAssets xsi:schemaLocation="urn:mtconnect.org:B2MML:1.3 /schemas/B2MML_1.3.xsd" xmlns:b="urn:mtconnect.org:B2MML:1.3" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ns="urn:mtconnect.org:MTConnectAssets:1.3" xmlns:m="urn:mtconnect.org:MTConnectAssets:1.3">
    <Header assetCount="132" assetBufferSize="10240" version="1.4.0.3" instanceId="1505504796" sender="ubuntu" creationTime="2017-10-12T15:00:06Z"/>
    <Assets>
        <B2mmlProductionSchedule assetId="49d681d4-a790-5a71-91ec-6fe760e8a297" deviceUuid="6ee5c9" timestamp="2017-09-15T19:46:49.746808Z">
            <AssetArchetypeRef assetId="9ede2e6c-f2f2-55fb-ab60-f7ae2a21ad17" assetType="b:B2mmlProductDefinition"/>
            <Targets>
                <Target type="DEVICE" targetId="WC81-SS">
                    <TargetDevice>WC81-SS</TargetDevice>
                </Target>
                <Target type="DEVICE" targetId="WC41-HTC">
                    <TargetDevice>WC41-HTC</TargetDevice>
                </Target>
                <Target type="DEVICE" targetId="WC89-QC">
                    <TargetDevice>WC89-QC</TargetDevice>
                </Target>
            </Targets>
            <ProductionSchedule ns="http://www.mesa.org/xml/B2MML-V0600">
                <ID>M17-10450</ID>
                <ProductionRequest>
                    <ID>M17-10450</ID>
                    <Description>Base, Part 1a, Rev. - Base, Part
                    1a</Description>
                    <StartTime>2017-09-11T00:00:00-04:00</StartTime>
                    <EndTime>2017-09-15T17:00:00-04:00</EndTime>
                    <SegmentRequirement>
                        <ProductSegmentID>TEST-00003 </ProductSegmentID>
                        <LatestEndTime>2017-09-15T17:00:00-04:00</LatestEndTime>
                        <Duration>PT15H30M</Duration>
                        <MaterialProducedRequirement>
                            <Quantity> <QuantityString>0.1E2</QuantityString>
                                <DataType>Amount</DataType>
                            </Quantity>
                        </MaterialProducedRequirement>
                        <MaterialConsumedRequirement> <MaterialConsumedRequirementProperty>
                                <ID>Availability</ID>
                                <Value>                                    <ValueString>Available</ValueString> <DataType>string</DataType>
                                </Value> </MaterialConsumedRequirementProperty>
                        </MaterialConsumedRequirement>
                    <SegmentRequirement>
                            <ID>005 </ID>
                            <Duration>PT10M</Duration>
                            <ProductionParameter>
                                <Parameter>
                                    <ID>SetupTime</ID>
                                    <Value> <ValueString>PT0S</ValueString>   <DataType>duration</DataType>
                                    </Value>
                                </Parameter>
                            </ProductionParameter>
                            <EquipmentRequirement>
                                <EquipmentID>WC81-SS </EquipmentID>
                                <Description>Shipping Support
                                </Description>
                                <EquipmentRequirementProperty>
                                    <ID>Cost</ID>
                                    <Value> <ValueString>6.918049999999999</ValueString> <DataType>Amount</DataType>
                                        <UnitOfMeasure>US
                                        Dollar</UnitOfMeasure>
                                    </Value> </EquipmentRequirementProperty>
                            </EquipmentRequirement>
                    </SegmentRequirement>
                    <RequestState>Released</RequestState>
                </ProductionRequest>
                <ScheduleState>Released</ScheduleState>
            </ProductionSchedule>
        </B2mmlProductionSchedule>
    </Assets>
</MTConnectAssets>
```

## Mapping of Terms Between the Standards

An addition effort to support the integration between B2MML and
MTConnect involves mapping of terms that are used in both standards and
represent common meaning and usage . The actual name applied to any term
is likely not to be the same in both standards. However, as long as the
terms represent common meaning and functionality in both standards, a
straightforward mapping process can be used to transfer the information
associated with these terms between the data models supported by each
standard.

An initial set of terms and definitions were developed in support of the
SPEC-OPS project. This list of terms primarily focuses on data types
associated with Shop Orders, Product Definitions, and Product
Scheduling. This initial list of terms can be used as the basic
groundwork for ongoing integration between the standards with the
expectation that that community will continue to grow and expand the
implementation examples and contribute their work back to the community.

The initial list of terms include:

| ISA-95/B2MML                                   | MTConnect                            | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ---------------------------------------------- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Equipment                                      | Device                               | The concept of Equipment is a higher-level construct that can represent a cell or a line. In MTConnect, a Device is a singular piece of equipment.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Product                                        | Part                                 | There is no analog to a complete product in MTConnect as a finished assembly.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Physical Asset                                 | Asset                                | The asset in MTConnect is a similar concept, but can also represent a logical asset as well. The models are more open information models that represent specific items vs. a general construct for representing the physical part of the manufacturing process.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Material                                       | Material                             | The concept of material is similar and is provided in the MTConnect process and Part models. Material in MTConnect is mainly concerned with metals and has specific properties that are relevant to machining.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Personnel                                      | User                                 | MTConnect is concerned with the operator and user of a machine or system. It does not have a skills model like Personnel and may need to associate with one later. The Personnel model also be used as a set of requirements for a process.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Production Schedule                            | Process Archetype                    | Production schedule is an ERP construct and MTConnect does not have a direct analogy for this. The best proxy would be the Process Archetype, but the fit is not exact. MTConnect is post-scheduling where this model represents something to be scheduled.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Work Definition or Work Master                 | Process Archetype                    | MTConnect does not have a model that represents an abstract set of work that needs to be completed. The model in ISA-95 is related to the Job Order for the production process. The creation of the master document has some relationship to the MTConnect Process Archetype. The Workflow Specification provides a flow-chart capability to represent process flows. This is included as part of the Work Master.                                                                                                                                                                                                                                                                                                                                                  |
| Work Schedule                                  | Process Archetype                    | The set of work required to be done as part of a job dispatching system. Has a list set of job orders to be run on the shop floor. This is part of the execution and dispatching part of the ISA-95 standard.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Job Schedule & Job Order                       | Process                              | A job schedule represents a set of operations that will be run on a machine. The Process instance model in MTConnect provides a similar set of process steps and activities that will be run at a specific machine. The Job Schedule and Job Order are closely tied to the start and end time of the process execution. MTConnect will be providing guidance schedule information into the model as well. The models will ostensibly provide a similar function, but at differing levels of abstraction.                                                                                                                                                                                                                                                            |
| Work Performance                               | Part                                 | The collection of data for Part genealogy is given in the MTConnect Part model. This is the record of the manufacturing process and any significant events or data requests that have been associated with a part of batch of parts. Work Performance has some similar concepts, but are more abstract.                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Process Segment                                | Process Step                         | A process segment provides a single operation and the required resources as well as the dependencies on other process steps. The Process Step in MTConnect also provides similar information, but are organized in a more specific model that is routing centric since MTConnect is not concerned with continuous process semantics.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Workflow Specification                         | Routing                              | There is no direct analogy to a specific routing in ISA-95. The Workflow Specification provides a set of transitions over a graph of nodes that are associated with the process or product segments. This is a flowchart abstraction that follows the abstract transitions between states and actions. The model is very general, but can encode things like go/no-go options along the connection edges. The MTConnect routing is a group of process steps that in turn have activities. The MTConnect model closely models the discrete manufacturing process flow and could be represented in a workflow specification, but would lose a lot of the detail that is associated with the domain. This is a good candidate for collaboration between the standards. |
| Equipment Capability                           | Device                               | The capability of a piece of equipment is given by the specification model in the device. This allows the device to be very specific about the machine tool’s capabilities. The ISA-95 model is a more general model that specifies the capabilities as an abstraction. For an equipment capability, it specifies the confidence and quantity of the equipment. This is addressing a different problem than MTConnect that is providing the machines specifications. One could provide the capabilities to make something or perform some work, but to reason on the data, one would need to be more specific and have a normalized set of capabilities across all equipment and locations.                                                                         |
| ID                                             | UUID                                 | MTConnect is moving towards unique identifiers for all asset and device identifiers.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| None                                           | Cutting Tool, Axes, Controller, etc… | There is no equivalency for the device and discrete specific vocabulary in ISA-95.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Product, Scheduling, Recipe, Performance, etc… | None                                 | The product and continuous process related information as well as the KPIs are not provided in MTConnect. MTConnect did not concern itself with the ERP and MES scheduling data that did not involve the equipment on the shop floor.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |