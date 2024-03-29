---
title: Installing C++ Agent on Mac
description: 
published: true
date: 2024-03-01T16:01:20.479Z
tags: 
editor: markdown
dateCreated: 2022-06-01T13:57:54.464Z
---

## Overview

Installation procedure for running MTConnect Agent on Mac.

## Building on Mac OS

Install brew and xcode command line tools.

```
	/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
	xcode-select —install
```

Run brew help to verify install. You may need to refer to "Next Steps" in the terminal for commands to add Homebrew to your PATH. 

```
brew help
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
```
Test the conan profile. If the default conan profile is not set, use the following command.
```
conan profile detect
```

### Build the agent

```
	conan create cppagent -pr cppagent/conan/profiles/macos --build=missing
```

### Add agent to PATH
Note that the filepath may be specific to your build; in this example the buildid is mtcond5569031024c6. 
```
touch .zshrc
nano .zshrc
```
Add the following to the .zshrc file; if the file does not exist opening in nano will add the file.
```
export PATH=$PATH:~/.conan2/p/b/mtcond5569031024c6/p/bin
```
Save changes to .zshrc and restart terminal. Run the agent.
```
agent
```

### For XCode

```   
	mkdir build & cd build
  cmake -G Xcode ..
```
