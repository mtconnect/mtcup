---
title: Installing C++ Agent on Ubuntu
description: 
published: true
date: 2024-03-18T02:10:29.454Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:31:47.231Z
---

## Overview

Installation procedure for running MTConnect Agent on Ubuntu. This is a general guide and covers the basic configuration of the MTConnect simulator.

## Setup the build

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential libxml2 libxml2-dev python3.9 python3-pip git cmake libcppunit-dev build-essential screen ruby curl rake
```

## Download the Agent source

```
git clone https://github.com/mtconnect/cppagent.git
```

## Install dependencies using conan

Install conan.

```
python3.9 -m pip install conan
source ~/.profile
```

Install dependencies.

```
cd cppagent
conan export conan/mqtt_cpp
conan export conan/mruby
conan install . -if build --build=missing -pr conan/profiles/gcc
```

## Build the agent

```
conan build . -bf build
echo 'export PATH=$PATH:~/cppagent/build/bin' >> ~/.bashrc
```

### If the build fails because of `out of memory` error

In case of a Raspberry Pi with a RAM size of 4 GB or less, the build might crash because of `out of memory` error. This can be addressed in two ways.

#### Reduce the number of available cores to build processes.

Conan uses all available cores on your Pi. More build processes require more memory. We can force the available cores to 1 which should address the issue for Pi with RAM size of 3GB and above. However, it will take longer to build. For a quad-core processor, you may try reducing it to 2 and it should work.

```
export CONAN_CPU_COUNT=1
```

Now try building the agent again.

#### Increase swap memory

The swap file is used to increase the system’s total accessible memory beyond its hardware capabilities. This means that when all of the Raspberry Pi’s RAM is exhausted, it can start using the swap file as memory instead.

One caveat of a large swap file is that you need that space to be free on your SD Card. You can’t set a swap file on your Raspberry Pi larger than your available free space.

Following are the steps to increase the swap file.

Turn off all swap processes.

```
sudo swapoff -a
```

Resize the swap as needed. Make sure RAM + Swap size is at least 6 GB. The change in size of the swap file **MUST** be available on your SD card. The example below increases swap file by 2 GBs.

```
sudo dd if=/dev/zero of=/swapfile bs=1G count=2
```

Make the file usable as swap.

```
sudo mkswap /swapfile
```

Activate the swap file.

```
sudo swapon /swapfile
```

Check the amount of swap available. It should have increased by 2 GBs.

```
grep SwapTotal /proc/meminfo
```

Now try building the agent again.

## Run the Agent

The following command will run the agent with a sample config file located in `~/cppagent/simulator`.

> Make sure the `Devices` file path is correct in the agent config file: `~/cppagent/simulator/agent.cfg`. Learn more about agent device files and how to create them for your own device here [].

> Make sure the `SchemaVersion` in the agent config file is the same as the agent version which is `2.0` as of `31st May 2022`.

```
cd ~/cppagent/simulator
agent debug agent.cfg
```

If it works, you should see something like:

```
  MTConnect Agent Version 2.0.0.5 - built on Tue May 31 15:59:57 2022
  [2022-05-31 19:49:01.990469] [0xb6f079c0] [info]    Loading configuration from: "/home/pi/cppagent/simulator/agent.cfg"
  Loading configuration from:"/home/pi/cppagent/simulator/agent.cfg"2022-05-31T14:19:01.992895Z (0xb6f079c0) [info] MAIN->AgentConfiguration::initialize->AgentConfiguration::loadConfig: Starting agent on port 5000
  2022-05-31T14:19:02.011530Z (0xb6f079c0) [warning] MAIN->AgentConfiguration::initialize->AgentConfiguration::loadConfig->AgentConfiguration::loadSinks: The following path "../styles/old" is not a regular file: "/home/pi/cppagent/simulator/../styles/old"
  2022-05-31T14:19:02.066829Z (0xb6f079c0) [info] MAIN->AgentConfiguration::initialize->AgentConfiguration::loadConfig->AgentConfiguration::loadAdapters: shdr: Adding adapter for VMC-3Axis: VMC-3Axis
  2022-05-31T14:19:02.075907Z (0xb6f079c0) [debug] MAIN->Agent::start->Connector::connect: Connecting to data source: 127.0.0.1 on port: 7878
  2022-05-31T14:19:02.076503Z (0xb69f7400) [error] Connector::connected: Connection refused: Connection refused
```

You will notice that connection to data source at 127.0.0.1 on port 7878 has been refused. This is where the agent is listening for an adapter which has not been configured yet. See [Develop an Adapter](/MTConnect_Adapter "wikilink") for how to develop and setup your own adapter.

You can run a simulated adapter to see how agent processes adapter data.

```
ruby -l run_scenario.rb VMC-3Axis-Log.txt
```

## Test the Agent output

To see the output in XML:

```
curl -H "Accept: application/xml" -H "Content-Type: application/xml" -X GET http://localhost:5000/current
```

To see the output in JSON:

```
curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X GET http://localhost:5000/current
```

To see the output in the browser, open the browser and go to `http://localhost:5000/current`

---

Learn how to configure the Agent: [Usage and Configuration](/Agent-Usage-and-Configuration "wikilink")

Learn how to write your own Device file for the Agent: [Model Devices](/MTConnect_Device_File "wikilink")

---

## Setup the systemd scripts

Just like configuring the agent above, install the following text files.

The systemd script to start the agent:

```
sudo sh -c 'cat <<EOT > /etc/systemd/system/mtc_agent.service
[Unit]
Description=MTConnect Agent

[Service]
WorkingDirectory=/home/pi/cppagent/simulator
ExecStart=agent run agent.cfg
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
WorkingDirectory=/home/pi/cppagent/simulator
ExecStart=/usr/bin/ruby run_scenario.rb -l VMC-3Axis-Log.txt
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

```
sudo systemctl start mtc_agent
sudo systemctl start mtc_adapter
```

Look for the processes

```
pgrep -a ruby
pgrep -a agent
```

Test the services

```
curl http://localhost:5000/current
```

Use the enable command to ensure that the service starts whenever the system boots

```
sudo systemctl enable mtc_agent
sudo systemctl enable mtc_adapter
```

Test