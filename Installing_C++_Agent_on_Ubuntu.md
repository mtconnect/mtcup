---
title: Installing C++ Agent on Ubuntu
description: 
published: true
date: 2021-09-25T01:41:14.544Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:31:47.231Z
---

## Overview

Installation procedure for running MTConnect Agent on Ubuntu. This is a
general guide and covers the basic configuration of the MTConnect
simulator.

## Install Required Dependencies

The C++ agent depends on libxml2 and for testing libcppunit. The rest
are the standard packages for building software. We also use cmake to
generate the make files.

```
sudo apt-get update
sudo apt-get install libxml2 libxml2-dev cmake git libcppunit-dev build-essential screen ruby curl
```

## Download and build the agent

Make a directory for the agent work and our build directory

```
mkdir -p ~/agent/build
cd ~/agent
```

Use git to clone the agent from the repo, init and update the submodules
and create the makefiles. We're going to be building the Release
version; a debug version can also be built, refer to the cmake docs for
more info.

```
git clone <https://github.com/mtconnect/cppagent.git>
cd build
cmake -D CMAKE_BUILD_TYPE=Release ../cppagent/
```

Now build the app and install it in /usr/local/bin

```
make
sudo cp agent/agent /usr/local/bin
```

There may be a few build warnings, these can be ignored.

## Setup a configuration directory

Setup the agent

```
sudo mkdir -p /etc/mtconnect/agent /etc/mtconnect/adapter
cd ~/agent/cppagent
sudo cp -r styles schemas simulator/VMC-3Axis.xml /etc/mtconnect/agent
```

Setup the simulator

```
sudo cp simulator/VMC-3Axis-Log.txt simulator/run_scenario.rb /etc/mtconnect/adapter
```

## Configure the Agent

Install the text file below for the configuration. To do this copy and
paste the code into a new text document. Line numbers as seen below are
added from the copy and paste process, get rid of them as they are not
part of the actual code. Copy and paste the edited text into the
Terminal and press enter.

``` 
sudo sh -c 'cat <<EOT > /etc/mtconnect/agent/agent.cfg
Devices = /etc/mtconnect/agent/VMC-3Axis.xml
AllowPut = true
ReconnectInterval = 1000
BufferSize = 17
SchemaVersion = 1.5

Adapters {
   VMC-3Axis {
      Host = 127.0.0.1
      Port = 7878
   }
}

Files {
    schemas {
        Path = /etc/mtconnect/agent/schemas
        Location = /schemas/
    }
    styles {
        Path = /etc/mtconnect/agent/styles
        Location = /styles/
    }
    Favicon {
        Path = /etc/mtconnect/agent/styles/favicon.ico
        Location = /favicon.ico
    }
}

StreamsStyle {
  Location = /styles/Streams.xsl
}

# Logger Configuration
logger_config
{
    logging_level = info
    output = file /var/log/mtc_agent.log
}
EOT'
```

Check your work. In the first line of the text file above,
"/etc/mtconnect/agent/agent.cfg" is the location where the file is
saved. Open the file manager and navigate to that location. If you
entered the code right, you should see the "agent.cfg" file. Open the
file and make sure it looks just like the text above and proceed.

Test the agent config from the command line.

` agent debug /etc/mtconnect/agent/agent.cfg`

If it works, you should see something like:

` MTConnect Agent Version 1.3.0.4 - built on Thu Sep  4 00:22:34 2014`
`     0 INFO  [0] init.config: Starting agent on port 5000`
`     5 INFO  [0] init.config: Adding adapter for VMC-3Axis on 127.0.0.1:7878`
`     6 DEBUG [0] agent: registerFile: Path /etc/mtconnect/agent/styles/favicon.ico is not a directory: Unable to find directory /etc/mtconnect/agent/styles/favicon.ico, trying as a file`
`     6 DEBUG [1] input.connector: Connecting to data source: 127.0.0.1 on port: 7878`
`     6 WARN  [1] input.connector: connect: Socket exception: unable to connect to '127.0.0.1:7878'`
`     6 INFO  [1] input.adapter: Will try to reconnect in 1000 milliseconds`
`  1006 DEBUG [1] input.connector: Connecting to data source: 127.0.0.1 on port: 7878`
`  1007 WARN  [1] input.connector: connect: Socket exception: unable to connect to '127.0.0.1:7878'`
`  1007 INFO  [1] input.adapter: Will try to reconnect in 1000 milliseconds`

## Setup the systemd scripts

Just like configuring the agent above, install the following text files.

The systemd script to start the agent:

```
sudo sh -c 'cat <<EOT > /etc/systemd/system/mtc_agent.service
[Unit]
Description=MTConnect Agent

[Service]
WorkingDirectory=/home/pi
ExecStart=/usr/local/bin/agent run /etc/mtconnect/agent/agent.cfg
Restart=always

[Install]
WantedBy=multi-user.target
EOT'
```

The systemd script for the adapter:

```
sudo sh -c 'cat <<EOT > /etc/systemd/system/mtc_adapter.service
[Unit]
Description=MTConnect Adapter

[Service]
WorkingDirectory=/home/pi
ExecStart=/usr/bin/ruby /etc/mtconnect/adapter/run_scenario.rb -l /etc/mtconnect/adapter/VMC-3Axis-Log.txt
Restart=always

[Install]
WantedBy=multi-user.target
EOT'
```

NOTE: Update your \*WorkingDirectory\* in the systemd scripts above if
different. Similarly if the \*ExecStart\* line of code doesn't execute,
make sure the full paths of the files/executables are correct.

Cycle the power (reboot) and proceed.

## Start the services

Start both services

` sudo systemctl start mtc_agent`
` sudo systemctl start mtc_adapter`

Look for the processes

` pgrep -a ruby`
` pgrep -a agent`

Test the services

` curl `<http://localhost:5000/current>

Use the enable command to ensure that the service starts whenever the
system boots

` sudo systemctl enable mtc_agent`
` sudo systemctl enable mtc_adapter`