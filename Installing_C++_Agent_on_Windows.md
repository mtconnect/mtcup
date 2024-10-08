---
title: Installing C++ Agent on Windows
description: Build, install, configure and run MTConnect C++ Agent on Windows.
published: true
date: 2024-09-09T14:14:39.124Z
tags: 
editor: markdown
dateCreated: 2022-06-01T12:39:42.594Z
---

## Overview

Installation procedure for running MTConnect Agent on Windows. 

## Windows Binary Release

The windows binary releases come with a prebuilt exe that is statically linked with the Microsoft Runtime libraries. 

Aside from the standard system libraries, the agent only requires winsock libraries. The agent has been test with version of Windows 2000 and later.

You can download the C++ Agent toolkit - Pre-built binaries here: [C++ Agent Windows Binaries](https://github.com/mtconnect/cppagent/releases)

## Building on Windows

If instead of using the pre built binaries you would like to build the Agent yourself on Windows then follow this section.

The MTConnect Agent uses the conan package manager to install dependencies:

[python 3](https://www.python.org/downloads/) and [Conan Package Manager Downloads](https://conan.io/downloads.html)

Download the Windows installer for your platform and run the installer.

You also need [git](https://git-scm.com/download/win) and [ruby](https://rubyinstaller.org/) if you want to embed mruby.

CMake is installed as part of Visual Studio. If you are using Visual Studio for the build, use the bundled version. Otherwise download from [CMake](https://cmake.org/download/).

### Setting up build

Install dependencies from the downloads above. Make sure python, ruby, and cmake are in your path.

```
pip install --upgrade pip
pip install conan
```

Clone the agent to another directory:

```
git clone https://github.com/mtconnect/cppagent.git
```

Make a build subdirectory of `cppagent`

```
cd cppagent
conan export conan\mqtt_cpp
conan export conan\mruby
```

####  To build for 64 bit Windows
	
Make sure to setup the environment For VS 2022:

```
"C:\Program Files\Microsoft Visual Studio\2022\Professional\VC\Auxiliary\Build\vcvars64.bat"
conan build . --build=missing -pr conan/profiles/vs64 
OR
conan build . --build=missing -pr conan/profiles/vs64debug

```
or

```
"C:\Program Files (x86)\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvars64.bat"	
conan build . --build=missing -pr conan/profiles/vs64
OR
conan build . --build=missing -pr conan/profiles/vs64debug

```
#### To build for 32 bit Windows

```
C:\Program Files\Microsoft Visual Studio\2022\Professional\VC\Auxiliary\Build\vcvars32.bat"
conan build . --build=missing -pr conan/profiles/vs32 
```

#### To build for Windows XP

The windows XP 140 XP toolchain needs to be installed under individual component in the Visual Studio 2022 installer. 

```
conan install . --build=missing -pr conan/profiles/vsxp
```

### Package the release
```
cpack -G ZIP
```