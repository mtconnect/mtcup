---
title: C++ Agent Usage and Configuration
description: 
published: true
date: 2023-01-03T23:19:21.251Z
tags: 
editor: markdown
dateCreated: 2022-06-01T14:19:42.467Z
---

## Usage

```
    agent [help|install|debug|run] [configuration_file]
       help           Prints this message
       install        Installs the service
       remove         Remove the service
       debug          Runs the agent on the command line with verbose logging
       run            Runs the agent on the command line
       config_file    The configuration file to load
                      Default: agent.cfg in current directory
```

For Microsoft Windows® Services, when the agent is started without any arguments it is assumed it will be running as a service and will begin the service initialization sequence. The full path to the configuration file is stored in the registry in the following location:

```
    \\HKEY_LOCAL_MACHINE\SOFTWARE\MTConnect\MTConnect Agent\ConfigurationFile
```

### Directories

* `agent/`        - This contains the application source files and CMake file.

* `assets/`       - Some example Cutting Tool asset files.

* `lib/`          - Third party library source. Contains source for cppunit, dlib, and libxml++.

* `samples/`      - Sample XML configuration files mainly used for tests.

* `simulator/`    - Ruby scripts to execute an adapter simulator, both command-line and log-replay.

* `test/`         - Various unit tests.

* `tools/`        - Ruby scripts to dump the agent and the adapter in SHDR format. Includes a sequence 
                    test script.

* `unix/`         - Unix init.d script

* `win32/`        - Libraries required only for the win32 build and the win32 solution.

## Configuration

The configuration file is using the standard Boost C++ file format. The configuration file format is flexible and allows for both many adapters to be served from one agent and an adapter to feed multiple agents. The format is:

```
    Key = Value
```

The key can only occur once within a block and the value can be any sequence of characters followed by a newline. There no significance to the order of the keys, so the file can be specified in free form. We will go over some configurations from a minimal configuration to more complex multi-adapter configurations.

A block that contains and contextualizes a set of properties is specfied as follows:

```
  Block { 
    Key = Value
  }
```

A comment begins with a `#` until the end of the line. 

```
  # This is a comment
  Key = Value # This is also a comment
```

### Basic Configuration

For the simplest configuration, the MTConnect Agent will look for a Devices.xml file in the location of the executable application and will attempt to connect to the localhost port 7878 for an adapter. 

This is equivilent to the following cofiguration: 

```
    Devices = Devices.xml
    Adapters {
      * { 
        Host = localhost
        Port = 7878
      }
    }
```

### General Configuration

### Monitoring Configuration Files

### Configuring REST Protocol

#### Allowing HTTP PUT/POST

* `AllowPutFrom`	- Allow HTTP PUT or POST from a specific host or 
  list of hosts. Lists are comma (,) separated and the host names will
  be validated by translating them into IP addresses.

    *Default*: none


#### Configuring TLS (HTTPS)

Transport Layer Security is enabled by adding certificates to the agent. The minimum configuration options required are the following configuration items:

* `TlsCertificateChain`: Specifies a file with a certificatate in `PEM` format for the REST server. If any intermediate certificates are required, they should be provide with the primary certificate first followed by the intermediate certificates in order. 
* `TlsPrivateKey`: The file containing the secret key in `PEM` format.
* `TlsDHKey`: The file containing the Diffie–Hellman (D-H) parameters for the key exchange.

The fllowing parameters are optional when configuring TLS:

* `TlsOnly`: Reject any request that does not use TLS (https) to access the server.
* `TlsCertificatePassword`: The password for the private key if required.
* `TlsVerifyClientCertificate`: Require the client certificate to be validated against root certs.
  * `TlsClientCAs`: A set of certificate authorities for client certiticate authentication. This is used when a self-signing client certificate is used.
  
#### Example Configuration

Given there are files in a directory relative to the `agent` directory:

```
Devices = Device.xml
Port = 5000

# TLS
TlsCertificateChain = ./CertificateChain.crt
TlsPrivateKey = ./PrivateKey.key
TlsDHKey = ./dh2048.pem
TlsCertificatePassword = secret
```

If one wants to disable all non-TLS connections:

```
TlsOnly = yes
```

When a client certification chain is used, specifiy it as follows:

```
TlsVerifyClientCertificate = yes
TlsClientCAs = ./ClientCA.crt
```
  
#### Exmple creating self-signing certificates for the agent

##### Creating Test Certifications (see resources gen_certs shell script)

This section assumes you have installed openssl and can use the command line. The subject of the certificate is only for testing and should not be used in production. This section is provided to support testing and verification of the functionality. A certificate provided by a real certificate authority should be used in a production process.

NOTE: The certificates must be generated with OpenSSL version 1.1.1 or later. LibreSSL 2.8.3 is not compatible with 
      more recent version of SSL on raspian (Debian).

###### Server Creating self-signed certificate chain

###### Create Signing authority key and certificate

	openssl req -x509 -nodes -sha256 -days 3650 -newkey rsa:3072 -keyout rootca.key -out rootca.crt -subj "/C=US/ST=State/L=City/O=Your Company, Inc./OU=IT/CN=serverca.org"

###### User Key

    openssl genrsa -out user.key 3072

###### Signing Request

    openssl req -new -sha256 -key user.key -out user.csr -subj "/C=US/ST=State/L=City/O=Your Company, Inc./OU=IT/CN=user.org"

###### User Certificate using root signing certificate

    openssl x509 -req -in user.csr -CA rootca.crt -CAkey rootca.key -CAcreateserial -out user.crt -days 3650

###### Create DH Parameters

    openssl dhparam -out dh2048.pem 3072

###### Verify

    openssl verify -CAfile rootca.crt rootca.crt
    openssl verify -CAfile rootca.crt user.crt

###### Client Certificate

###### Create Signing authority key

    openssl req -x509 -nodes -sha256 -days 3650 -newkey rsa:3072 -keyout clientca.key -out clientca.crt -subj "/C=US/ST=State/L=City/O=Your Company, Inc./OU=IT/CN=clientca.org"

###### Create client key

    openssl genrsa -out client.key 3072

###### Create client signing request

    openssl req -new -key client.key -out client.csr -subj "/C=US/ST=State/L=City/O=Your Company, Inc./OU=IT/CN=client.org"

###### Create Client Certificate

For client.cnf

    basicConstraints = CA:FALSE
    nsCertType = client, email
    nsComment = "OpenSSL Generated Client Certificate"
    subjectKeyIdentifier = hash
    authorityKeyIdentifier = keyid,issuer
    keyUsage = critical, nonRepudiation, digitalSignature, keyEncipherment
    extendedKeyUsage = clientAuth, emailProtection

##### Create the cert

    openssl x509 -req -in client.csr -CA clientca.crt -CAkey clientca.key -out client.crt -CAcreateserial -days 3650 -sha256 -extfile client.cnf

###### Verify

    openssl verify -CAfile clientca.crt clientca.crt
    openssl verify -CAfile clientca.crt client.crt


### Configuring Adapters

#### SHDR Adapters

#### Agent Adapter

#### MQTT Adapters

### Configuring Sinks

#### MQTT Sink

### Adding Plugins

### Ruby Transformations

### Serving Static Content

Using a Files Configuration section, individual files or directories can be inserted into the request file space from the agent. To do this, we use the Files top level  configuration declaration as follows:

```
    Files {
        schemas {
            Path = ../schemas
            Location = /schemas/
        }
        styles {
            Path = ../styles
            Location = /styles/
        }
    }
```

Each set of files must be declared using a named file description, like schema or styles and the local `Path` and the `Location` the files will be mapped to in the HTTP server namespace. For example:

```
    http://example.com:5000/schemas/MTConnectStreams_2.0.xsd will map to ../schemas/MTConnectStreams_2.0.xsd
```

All files will be mapped and the directory names do not need to be the same. These files can be either served directly or can be used to extend the schema or add XSLT stylesheets for formatting the XML in browsers.

### Specifying the Extended Schemas

To specify the new schema for the documents, use the following declaration:

```
    StreamsNamespaces {
      e {
        Urn = urn:example.com:ExampleStreams:2.0
        Location = /schemas/ExampleStreams_2.0.xsd
      }
    }
```

This will use the ExampleStreams_2.0.xsd schema in the document. The `e` is the alias that will be used to reference the extended schema. The `Location` is the location of the xsd file relative in  the agent namespace. The `Location` must be mapped in the `Files` section.

An optional `Path` can be added to the `...Namespaces` declaration instead of declaring the  `Files` section. The `Files` makes it easier to include multiple files from a directory and will automatically include all the default MTConnect schema files for the correct version. (See `SchemaVersion` option below)

You can do this for any one of the other documents: 

```
    StreamsNamespaces
    DevicesNamespaces
    AssetsNamespaces
    ErrorNamespaces
```

### Specifying the XML Document Style

The same can be done with style sheets, but only the Location is required.

```
    StreamsStyle {
      Location = /styles/Streams.xsl
    }
```
    
An optional `Path` can also be used to reference the xsl file directly. This will not include other files in the path like css or included xsl transforms. It is advised to use the `Files` declaration.
    
The following can also be declared:

```
    DevicesStyle
    StreamsStyle
    AssetsStyle
    ErrorStyle
```

### Examples

#### Example 1:

Here’s an example configuration file. The `#` character can be used to comment sections of the file. Everything on the line after the `#` is ignored. We will start with an extremely simple configuration file.

```
    # A very simple file...
    Devices = VMC-3Axis.xml
```

This is a one line configuration that specifies the XML file to load for the devices. Since all the values are defaulted, an empty configuration file can be specified. This configuration file will load the `VMC-3Axis.xml` file and try to connect to an adapter located on the localhost at port 7878. `VMC-3Axis.xml` must only contain the definition for one device; if more devices exist, an error will be raised and the process will exit.

#### Example 2:

Most configuration files will specify at least one adapter. The adapters are contained within a block. The Boost configuration file format allows for nested configurations and block associations. There are a number of configurations that can be given for each adapter. Multiple adapters can be specified for one device as well.

```
    Devices = VMC-3Axis.xml

    Adapters
    {
        VMC-3Axis
        {
            Host = 192.168.10.22
            Port = 7878 # *Default* value...
        }
    }
```

This example loads the devices file as before, but specifies the list of adapters. The device is taken from the name `VMC-3Axis` that starts the nested block and connects to the adapter on `192.168.10.22` and port `7878`. Another way of specifying the equivalent configuration is:

```
    Devices = VMC-3Axis.xml

    Adapters
    {
        Adapter_1
        {
            Device = VMC-3Axis
            Host = 192.168.10.22
            Port = 7878 # *Default* value...
        }
    }
```

Line 7 specifies the Device name associated with this adapter explicitly. We will show how this is used in the next example.

#### Example 3:

Multiple adapters can supply data to the same device. This is done by creating multiple adapter entries and specifying the same Device for each.

```
    Devices = VMC-3Axis.xml

    Adapters
    {
        Adapter_1
        {
            Device = VMC-3Axis
            Host = 192.168.10.22
            Port = 7878 # *Default* value...
        }
    
        # Energy sensor
        Adapter_2
        {
            Device = VMC-3Axis
            Host = 192.168.10.2
            Port = 7878 # *Default* value...
        }
    }
```

Both `Adapter_1` and `Adapter_2` will feed the `VMC-3Axis` device with different data items.  The `Adapter_1` name is arbitrary and could just as well be named `EnergySensor` if desired as illustrated below.

```
    Devices = VMC-3Axis.xml

    Adapters
    {
        Controller
        {
            Device = VMC-3Axis
            Host = 192.168.10.22
            Port = 7878 # *Default* value...
        }
    
        EnergySensor
        {
            Device = VMC-3Axis
            Host = 192.168.10.2
            Port = 7878 # *Default* value...
        }
    }
```

#### Example 4:

In this example we change the port to 80 which is the default http port.  This also allows HTTP PUT from the local machine and 10.211.55.2.  

```
    Devices = MyDevices.xml
    Port = 80
    AllowPutFrom = localhost, 10.211.55.2

    Adapters
    {
        ...
```

For browsers you will no longer need to specify the port to connect to.

#### Example 5:

If multiple devices are specified in the XML file, there must be an adapter feeding each device.

```
    Devices = MyDevices.xml

    Adapters
    {
        VMC-3Axis
        {
            Host = 192.168.10.22
        }
    
        HMC-5Axis
        {
            Host = 192.168.10.24
        }
    }
```

This will associate the adapters for these two machines to the VMC and HMC devices in `MyDevices.xml` file. The ports are defaulted to 7878, so we are not required to specify them.

#### Example 6:

In this example we  demonstrate how to change the service name of the agent. This allows a single machine to run multiple agents and/or customize the name of the service. 
Multiple configuration files can be created for each service, each with a different ServiceName. The configuration file must be referenced as follows:

```
    C:> agent install myagent.cfg
```

If myagent.cfg contains the following statements:

```
    Devices = MyDevices.xml
    ServiceName = MTC Agent 1

    Adapters
    {
        ...
```

The service will now be displayed as "MTC Agent 1" as opposed to "MTConnect Agent" and it will automatically load the contents of myagent.cfg with it starts. You can now 
use the following command to start this from a command prompt:

```
    C:> net start "MTC Agent 1"
```

To remove the service, do the following:

```
    C:> agent remove myagent.cfg
```

#### Example 7:

Logging configuration is specified using the `logger_config` block. You can change the `logging_level` to specify the verbosity of the logging as well as the destination of the logging output.

```
    logger_config
    {
        logging_level = debug
        output = file debug.log
    }
```

This will log everything from debug to fatal to the file debug.log. For only fatal errors you can specify the following:

```
    logger_config
    {
        logging_level = fatal
    }
```

The default file is agent.log in the same directory as the agent.exe file resides. The default logging level is `info`. To have the agent log to the command window:

```
    logger_config
    {
        logging_level = debug
        output = cout
    }
```

This will log debug level messages to the current console window. When the agent is run with debug, it sets the logging configuration to debug and outputs to the standard output as specified above.

#### Example 8:

The MTConnect C++ Agent supports extensions by allowing you to specify your own XSD schema files. These files must include the MTConnect schema and the top level node is required to be MTConnect. The "x" in this case is the namespace. You **MUST NOT** use the namespace "m" since it is reserved for MTConnect. See example 9 for an example of changing the MTConnect schema file location.

There are four namespaces in MTConnect: Devices, Streams, Assets, and Error. In this example we will replace the Streams and Devices namespace with our own namespace so we can have validatable XML documents. 

```
	StreamsNamespaces {
	  x {
	    Urn = urn:example.com:ExampleStreams:1.2
	    Location = /schemas/ExampleStreams_1.2.xsd
	    Path = ./ExampleStreams_1.2.xsd
	  }
	}

	DevicesNamespaces {
	  x {
	    Urn = urn:example.com:ExampleDevices:1.2
	    Location = /schemas/ExampleDevices_1.2.xsd
	    Path = ./ExampleDevices_1.2.xsd
	  }
	}
```
	
For each schema file we have three options we need to specify. The Urn is the urn in the schema file that will be used in the header. The Location is the path specified in the URL when requesting the schema file from the HTTP client and the Path is the path on the local file system.

#### Example 9:

If you only want to change the schema location of the MTConnect schema files and serve them from your local agent and not from the default internet location, you can use the namespace "m" and give the new schema file location. This MUST be the MTConnect schema files and the urn will always be the MTConnect urn for the "m" namespace -- you cannot change it.

```
	StreamsNamespaces {
	  m {
	    Location = /schemas/MTConnectStreams_2.0.xsd
	    Path = ./MTConnectStreams_2.0.xsd
	  }
	}

	DevicesNamespaces {
	  m {
	    Location = /schemas/MTConnectDevices_2.0.xsd
	    Path = ./MTConnectDevices_2.0.xsd
	  }
	}
```
    
The MTConnect agent will now serve the standard MTConnect schema files from the local directory using the schema path `/schemas/MTConnectDevices_2.0.xsd`.


#### Example 10:

We can also serve files from the MTConnect Agent as well. In this example we can assume we don't have access to the public internet and we would still like to provide the MTConnect streams and devices files but have the MTConnect Agent serve them up locally. 

```
	DevicesNamespaces {
	  x {
	    Urn = urn:example.com:ExampleDevices:2.0
	    Location = /schemas/ExampleDevices_2.0.xsd
	    Path = ./ExampleDevices_2.0.xsd
	  }

	Files {
	  stream { 
	    Location = /schemas/MTConnectStreams_2.0.xsd
	    Path = ./MTConnectStreams_2.0.xsd
	  }
	  device { 
	    Location = /schemas/MTConnectDevices_2.0.xsd
	    Path = ./MTConnectDevices_2.0.xsd
	  }
	}
```
	
Or use the short form for all files:

```
        Files {
          schemas { 
            Location = /schemas/MTConnectStreams_2.0.xsd
            Path = ./MTConnectStreams_2.0.xsd
          }
        }
```
    
If you have specified in your xs:include schemaLocation inside the ExampleDevices_2.0.xsd file the location "/schemas/MTConnectStreams_2.0.xsd", this will allow it to be served properly. This can also be done using the Devices namespace:

```
	DevicesNamespaces {
	  m {
	    Location = /schemas/MTConnectDevices_2.0.xsd
	    Path = ./MTConnectDevices_2.0.xsd
	  }
	}
```

The MTConnect agent will allow you to serve any other files you wish as well. You can specify a new static file you would like to deliver:

```
	Files {
	  myfile { 
	    Location = /files/xxx.txt
	    Path = ./files/xxx.txt
	  }
```

The agent will not serve all files from a directory and will not provide an index function as this is insecure and not the intended function of the agent.

### Ruby

If the "-o with_ruby=True" build is selected, then use to following configuration:

```
    Ruby {
      module = path/to/module.rb
    }
```

The module specified at the path given will be loaded. There are examples in the test/Resources/ruby directory in  github: [Ruby Tests](https://github.com/mtconnect/cppagent/tree/master/test/resources/ruby).

The current functionality is limited to the pipeline transformations from the adapters. Future changes will include adding sources and sinks.

The following is a complete example for fixing the Execution of a machine:

```ruby
class AlertTransform < MTConnect::RubyTransform
  def initialize(name, filter)
    @cache = Hash.new
    super(name, filter)
  end

  @@count = 0
  def transform(obs)
    @@count += 1
    if @@count % 10000 == 0
      puts "---------------------------"
      puts ">  #{ObjectSpace.count_objects}"
      puts "---------------------------"
    end
    
    dataItemId = obs.properties[:dataItemId]
    if dataItemId == 'servotemp1' or dataItemId == 'Xfrt' or dataItemId == 'Xload'
      @cache[dataItemId] = obs.value
      device = MTConnect.agent.default_device
      
      di = device.data_item('xaxisstate')
      if @cache['servotemp1'].to_f > 10.0 or @cache['Xfrt'].to_f > 10.0 or @cache['Xload'].to_f > 10
        newobs = MTConnect::Observation.new(di, "ERROR")
      else
        newobs = MTConnect::Observation.new(di, "OK")
      end
      forward(newobs)
    end
    forward(obs)
  end
end
    
MTConnect.agent.sources.each do |s|
  pipe = s.pipeline
  puts "Splicing the pipeline"
  trans = AlertTransform.new('AlertTransform', :Sample)
  puts trans
  pipe.splice_before('DeliverObservation', trans)
end
```

## Configuration Parameters

### Top level configuration items

* `BufferSize` - The 2^X number of slots available in the circular
  buffer for samples, events, and conditions.

    *Default*: 17 -> 2^17 = 131,072 slots.

* `MaxAssets` - The maximum number of assets the agent can hold in its buffer. The
  number is the actual count, not an exponent.

    *Default*: 1024

* `CheckpointFrequency` - The frequency checkpoints are created in the
  stream. This is used for current with the at argument. This is an
  advanced configuration item and should not be changed unless you
  understand the internal workings of the agent.

    *Default*: 1000

* `Devices` - The XML file to load that specifies the devices and is
  supplied as the result of a probe request. If the key is not found
  the defaults are tried.

    *Defaults*: probe.xml or Devices.xml 

* `PidFile` - UNIX only. The full path of the file that contains the
  process id of the daemon. This is not supported in Windows.

    *Default*: agent.pid

* `ServiceName` - Changes the service name when installing or removing 
  the service. This allows multiple agents to run as services on the same machine.

    *Default*: MTConnect Agent

* `Port`	- The port number the agent binds to for requests.

    *Default*: 5000
    
* `ServerIp` - The server IP Address to bind to. Can be used to select the interface in IPV4 or IPV6.

    *Default*: 0.0.0.0

* `AllowPut`	- Allow HTTP PUT or POST of data item values or assets.

    *Default*: false

* `AllowPutFrom`	- Allow HTTP PUT or POST from a specific host or 
  list of hosts. Lists are comma (,) separated and the host names will
  be validated by translating them into IP addresses.

    *Default*: none

* `LegacyTimeout`	- The default length of time an adapter can be silent before it
  is disconnected. This is only for legacy adapters that do not support heartbeats.

    *Default*: 600

* `ReconnectInterval` - The amount of time between adapter reconnection attempts. 
  This is useful for implementation of high performance adapters where availability
  needs to be tracked in near-real-time. Time is specified in milliseconds (ms).
      
    *Default*: 10000
      
* `IgnoreTimestamps` - Overwrite timestamps with the agent time. This will correct
  clock drift but will not give as accurate relative time since it will not take into
  consideration network latencies. This can be overridden on a per adapter basis.
  
    *Default*: false

* `PreserveUUID` - Do not overwrite the UUID with the UUID from the adapter, preserve
  the UUID in the Devices.xml file. This can be overridden on a per adapter basis.

    *Default*: true
    
* `SchemaVersion` - Change the schema version to a different version number.

    *Default*: 2.0

* `ConversionRequired` - Global default for data item units conversion in the agent. 
  Assumes the adapter has already done unit conversion.

    *Default*: true
    
* `UpcaseDataItemValue` - Always converts the value of the data items to upper case.

    *Default*: true

* `MonitorConfigFiles` - Monitor agent.cfg and Devices.xml files and restart agent if they change.

    *Default*: false

* `MinimumConfigReloadAge` - The minimum age of a config file before an agent reload is triggered (seconds).

    *Default*: 15

* `Pretty` - Pretty print the output with indententation

    *Default*: false

* `ShdrVersion` - Specifies the SHDR protocol version used by the adapter. When greater than one (1), allows multiple complex observations, like `Condition` and `Message` on the same line. If it equials one (1), then any observation requiring more than a key/value pair need to be on separate lines. This is the default for all adapters.

    *Default*: 1
	
* `SuppressIPAddress` - Suppress the Adapter IP Address and port when creating the Agent Device ids and names. This applies to all adapters.

    *Default*: false

* `WorkerThreads` - The number of operating system threads dedicated to the Agent

    *Default*: 1
	
#### Configuration Pameters for TLS (https) Support

The following parameters must be present to enable https requests. If there is no password on the certificate, `TlsCertificatePassword` may be omitted.
	
* `TlsCertificateChain` - The name of the file containing the certificate chain created from signing authority

    *Default*: *NULL*

* `TlsPrivateKey` -  The name of the file containing the private key for the certificate

    *Default*: *NULL*

* `TlsDHKey` -  The name of the file containing the Diffie–Hellman key

    *Default*: *NULL*

* `TlsCertificatePassword` -  The password used when creating the certificate. If none was supplied, do not use.

    *Default*: *NULL*

* `TlsOnly` -  Only allow secure connections, http requests will be rejected

    *Default*: false

* `TlsVerifyClientCertificate` - Request and verify the client certificate against root authorities

    *Default*: false

* `TlsClientCAs` - For `TlsVerifyClientCertificate`, specifies a file that contains additional certificate authorities for verification

    *Default*: *NULL*


### Adapter configuration items

* `Adapters` - Adapters begins a list of device blocks. If the Adapters
  are not specified and the Devices file only contains one device, a
  default device entry will be created with an adapter located on the
  localhost and port 7878 associated with the device in the devices
  file.

    *Default*: localhost 5000 associated with the default device

    * `Device` - The name of the device that corresponds to the name of the device in the Devices file. Each adapter can map to one device. Specifying a "*" will map to the default device.
    
        *Default*: The name of the block for this adapter or if that is
        not found the default device if only one device is specified
        in the devices file.

    * `Host` - The host the adapter is located on.

        *Default*: localhost

    * `Port` - The port to connect to the adapter.

        *Default*: 7878

    * `Manufacturer` - Replaces the manufacturer attribute in the device XML.

        *Default*: Current value in device XML.

    * `Station` - Replaces the Station attribute in the device XML.

        *Default*: Current value in device XML.

    * `SerialNumber` - Replaces the SerialNumber attribute in the device XML.

        *Default*: Current value in device XML.
        
    * `UUID` - Replaces the UUID attribute in the device XML.

        *Default*: Current value in device XML.

    * `AutoAvailable` - For devices that do not have the ability to provide available events, if `yes`, this sets the `Availability` to AVAILABLE upon connection.

        *Default*: no (new in 1.2, if AVAILABILITY is not provided for device it will be automatically added and this will default to yes)

    * `AdditionalDevices` - Comma separated list of additional devices connected to this adapter. This provides availability support when one adapter feeds multiple devices.

        *Default*: nothing

    * `FilterDuplicates` - If value is `yes`, filters all duplicate values for data items. This is to support adapters that are not doing proper duplicate filtering.

        *Default*: no

    * `LegacyTimeout` - length of time an adapter can be silent before it is disconnected. This is only for legacy adapters that do not support heartbeats. If heartbeats are present, this will be ignored.

        *Default*: 600

    * `ReconnectInterval` - The amount of time between adapter reconnection attempts. This is useful for implementation of high performance adapters where availability needs to be tracked in near-real-time. Time is specified in milliseconds (ms). Defaults to the top level `ReconnectInterval`.
      
        *Default*: 10000
        
    * `IgnoreTimestamps` - Overwrite timestamps with the agent time. This will correct clock drift but will not give as accurate relative time since it will not take into consideration network latencies. This can be overridden on a per adapter basis.

        *Default*: Top Level Setting
        
    * `PreserveUUID` - Do not overwrite the UUID with the UUID from the adapter, preserve the UUID in the Devices.xml file. This can be overridden on a per adapter basis.

		    *Default*: false
		
    * `RealTime` - Boost the thread priority of this adapter so that events are handled faster.

        *Default*: false
    
    * `RelativeTime` - The timestamps will be given as relative offsets represented as a floating point number of milliseconds. The offset will be added to the arrival time of the first recorded event.

        *Default*: false

    * `ConversionRequired` - Adapter setting for data item units conversion in the agent. 
      Assumes the adapter has already done unit conversion. Defaults to global.

        *Default*: Top Level Setting
        
    * `UpcaseDataItemValue` - Always converts the value of the data items to upper case.

        *Default*: Top Level Setting

	* `ShdrVersion` - Specifies the SHDR protocol version used by the adapter. When greater than one (1), allows multiple complex observations, like `Condition` and `Message` on the same line. If it equials one (1), then any observation requiring more than a key/value pair need to be on separate lines. Applies to only this adapter.

	    *Default*: 1

	* `SuppressIPAddress` - Suppress the Adapter IP Address and port when creating the Agent Device ids and names.
		*Default*: false


### Agent Adapter Configuration

* `Url` - The URL of the source agent. `http:` or `https:` are accepted for the protocol.
* `SourceDevice` – The Device name or UUID for the source of the data
* `Count` – the number of items request during a single sample

    *Default*: 1000  
    
* `Polling Interval` – The interval used for streaming or polling in milliseconds

    *Default*: 500ms
    
* `Reconnect Interval` – The interval between reconnection attampts in milliseconds

    *Default*: 10000ms
    
* `Use Polling` – Force the adapter to use polling instead of streaming. Only set to `true` if x-multipart-replace blocked.

    *Default*: false
    
* `Heartbeat` – The heartbeat interval from the server

    *Default*: 10000ms

### logger_config configuration items


* `logger_config` - The logging configuration section.

    * `logging_level` - The logging level: `trace`, `debug`, `info`, `warn`,
        `error`, or `fatal`.

        *Default*: `info`

    * `output` - The output file or stream. If using a file, specify  as: `"file <filename>"`. cout and cerr can be used to specify the standard output and standard error streams. *Default*s to the same directory as the executable.

        *Default*: file `adapter.log`
        
    * `max_size` - The maximum log file size. Suffix can be K for kilobytes, M for megabytes, or G for gigabytes. No suffix will default to bytes (B). Case is ignored.
    
        *Default*: 10M
        
    * `max_index` - The maximum number of log files to keep.

        *Default*: 9
        
    * `schedule` - The scheduled time to start a new file. Can be DAILY, WEEKLY, or NEVER.
            
        *Default*: NEVER
    
## Adapter Agent Protocol Version 2.0

[Adapter Agent Protocol](/Protocol "wikilink")

