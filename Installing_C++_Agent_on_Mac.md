---
title: Installing C++ Agent on Mac
description: 
published: true
date: 2022-06-01T13:57:54.464Z
tags: 
editor: markdown
dateCreated: 2022-06-01T13:57:54.464Z
---

## Overview

Installation procedure for running MTConnect Agent on Mac.

## Building on Mac OS

Install brew and xcode command line tools

```
	/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
	xcode-select —install
```

### Setup the build

```
  brew install git
  brew install cmake

	pip3 install conan
```

### Download the source

```
	git clone https://github.com/mtconnect/cppagent.git
```

### Install the dependencies

```
    cd cppagent
    conan export conan/mqtt_cpp
    conan export conan/mruby
    conan install . -if build  --build=missing
```

### Build the agent

```	
	conan build . -bf build
```

### For XCode

```   
	mkdir build & cd build
  cmake -G Xcode ..
```
