---
title: Installing C++ Agent on Fedora Alpine
description: 
published: true
date: 2022-06-01T14:01:54.372Z
tags: 
editor: markdown
dateCreated: 2022-06-01T14:01:54.372Z
---

## Overview

Installation procedure for running MTConnect Agent on Fedora Alpine. 

## Building on Fedora Alpine

### As Root

```
	apk add g++ python3 cmake git linux-headers make perl ruby
	gem install rake
	
	python3 -m ensurepip
	python3 -m pip install --upgrade pip
```

### As the user

```
	export PATH=~/.local/bin:$PATH
	pip3 install conan	
	git clone https://github.com/mtconnect/cppagent.git
```

### Export mqtt_cpp package

```
	cd cppagent
	conan export conan/mqtt_cpp/
	conan export conan/mruby/
```

### Install packages

```
conan install . -if build -pr conan/profiles/gcc --build=missing
```

### Build the agent

```
	conan build . -bf build
```