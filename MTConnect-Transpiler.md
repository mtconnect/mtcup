---
title: MTConnect Transpiler
description: An overview of the MTConnect Transpiler project
published: true
date: 2023-12-04T19:51:29.082Z
tags: mtconnect, transpiler, c#, open-source
editor: markdown
dateCreated: 2023-12-04T19:37:48.495Z
---

# MTConnect Transpiler
## Overview
The MTConnect Transpiler refers to the open-source, C# library that deserializes the MTConnect SysML model into a strongly-typed model. With the implementation of "Sinks", the Transpiler is capable of transpiling the SysML model of the standard into anything such as code, documentation, and more.

The transpiler represents a leap forward in standards development and implementation. Developers that implement the transpiler in their workflows will find it's easier to maintain parity with the development of the standard.

## Getting Started
As a developer, if you're looking to use implement "model-aware" in your software, it is recommended to look for existing, open-source transpiler projects that may already output the codebase you need. For example, the `MtconnectCore` and `MTConnect.NET` C# libraries already contain transpiled code from the standard.

If you need a more custom codebase, then you will need to utilize sinks to implement your own executable transpiler.

# Sinks
The MTConnect Transpiler implements a sink architecture where-in the `MtconnectTranspiler` library is capable of deserializing the XMI of the MTConnect SysML model into a strongly-typed class model. The `MtconnectTranspiler` then sends this model to sink operator(s) to complete the tranpilation process.

## Developing a Sink
To get started developing a custom sink, determine if there is an existing "abstract" sink that can help you get started. Examples of this include the `ScribanTemplates` or `CSharp` projects which give you a headstart in implementing the `Transpiler`.

Continue with these steps:
 - Create a new console application project in C#
 - **If starting fresh**, reference the `Mtconnect.Transpiler` library in your C#
   - `$ dotnet add package MtconnectTranspiler --version`. View the `MtconnectTranspiler` project on [GitHub (mtconnect/MtconnectTranspiler)](https://github.com/mtconnect/MtconnectTranspiler)
   - Implement the `ITranspilerSink` interface and the `Transpile(XmiDocument model, CancellationToken cancellationToken)` method
 - **If using an abstract sink**, reference the sink and the `Mtconnect.Transpiler` library should be inherited
   - Implement the abstract sink according to its documentation
 - Write your console application to execute the transpiler:
   - Create a `TranspilerDispatcher`
     - `var dispatcher = new TranspilerDispatcher(new FromFileOptions() { Filepath = "...path to XMI" })`
   - Add your sink to the dispatcher
     - `dispatcher.AddSink(new Transpiler(...Options))`
   - Run the transpiler
     - `await dispatcher.TranspilerAsync()`

**Note** the specific construction of the `Transpiler` will differ depending on the options required.

## Development Workflow
Once a transpiler is created, for example to write code, it should only need to be run in case of the following events:

 - The format of the output changes
 - The transpiler has been updated (or its dependencies)
 - The MTConenct Standard is updated (ie. v2.2 to v2.3)
 
These events may be rare and are appropriate candidates for workflow automation.

 - In the event that format changes, developers could create pipelines based on Releases of their git repositories.
 - In the event that the transpiler has been updated (or dependencies), existing pipelines like Dependabot can be utilized
 - In the event that the Standard is updated, workflows can be scheduled to continually check for updates on the standard and compare against your codebase
   - For example, checkout this [GitHub Workflow (TrueAnalyticsSolutions/mtconnect-compare-version-action)](https://github.com/marketplace/actions/tams-mtconnect-version-comparator)
 
If you've implemented your transpiler as an executable (ie. EXE file or cross-platform .NET standard app) then you can easily trigger the execution of the transpiler at the end of your workflow. Then, it is recommended to commit any code changes to a new branch and create a new pull request.



## Sink Examples
Here are a few examples of sink implementations that can be referenced and built upon:

 - `MtconnectTranspiler.Sinks.ScribanTemplates`: An abstract sink that utilizes [Scriban](https://github.com/scriban/scriban) templates to generate static files. View on [GitHub (mtconnect/MtconnectTranspiler.Sinks.ScribanTemplates)](https://github.com/mtconnect/MtconnectTranspiler.Sinks.ScribanTemplates)
   - `MtconnectTranspiler.Sinks.CSharp`: An abstract sink that builds upon the `MtconnectTranspiler.Sinks.ScribanTemplates` project to write `.cs` (C#) files. View on [GitHub (mtconnect/MtconnectTranspiler.Sinks.CSharp)](https://github.com/mtconnect/MtconnectTranspiler.Sinks.CSharp)
     - `MtconnectCore`: An open-source project that uses the `MtconnectTranspiler.Sinks.CSharp` to write custom classes and enums that reflect the standard. View on [GitHub (TrueAnalyticsSolutions/MtconnectCore)](https://github.com/TrueAnalyticsSolutions/MtconnectCore)
     - `AdapterSdk`: An open-source project that uses the `MtconnectTranspiler.Sinks.CSharp` to write custom classes and enums that reflect the standard. View on [GitHub (TrueAnalyticsSolutions/Mtconnect.Adapter)](https://github.com/TrueAnalyticsSolutions/Mtconnect.Adapter)
   - `MtconnectTranspiler.Sinks.JsonSchema`: An abstract sink that builds upon the `MtconnectTranspiler.Sinks.ScribanTemplates` project to write `.json` (JSON) files. View on [GitHub (mtconnect/MtconnectTranspiler.Sinks.JsonSchema)](https://github.com/mtconnect/MtconnectTranspiler.Sinks.JsonSchema)
 - `MTConnect.NET`: A fork of the `MtconnectTranspiler` project used to maintain class models used in `MTConnect.NET` libraries. View on [GitHub (TrakHound/MTConnect.NET)](https://github.com/TrakHound/MTConnect.NET)
