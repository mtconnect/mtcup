---
title: Adapter Agent Protocol
description: 
published: true
date: 2023-05-27T04:21:49.521Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:32:21.529Z
---

## Overview

The MTConnect Standard defines the normative information model and protocol for retrieving information from manufacturing equipment. It also specifies the agent behavior and protocol. See the model here: [MTConnect Protocol](https://model.mtconnect.org/#Package__9eede38c-aa3e-4786-ba3a-416344b1d745).

To use OPC UA as a protocol, see
[MTConnect OPCUA CS](https://www.mtconnect.org/opc-ua-companion-specification).

Support for MQTT message brokers is expected in a future release.
Additional protocols may be supported by third parties writing their own
Agents.

## Adapter Agent Protocol Version 2.0

The principle adapter data format is a simple plain text stream separated by the pipe character `|`. Every line except for commands starts with an optional timestamp in UTC. If the timestamp is not supplied the agent will supply a timestamp of its own taken at the arrival time of the data to the agent. The remainder of the line is a key followed by data – depending on the type of data item is being written to.

A simple set of events and samples will look something like this:

```
	2009-06-15T00:00:00.000000|power|ON|execution|ACTIVE|line|412|Xact|-1.1761875153|Yact|1766618937
```

A line is a sequence of fields separated by `|`. The simplest requires one key/value pair. The agent will discard any lines where the data is malformed. The end must end with a LF (ASCII 10) or CR-LF (ASCII 15 followed by ASCII 10) (UNIX or Windows conventions respectively). The key will map to the data item using the following items: the `id` attribute, the `name` attribute, and the `CDATA` of the `Source` element. If the key does not match it will be rejected and the agent will log the first time it fails. Different data items categories and types require a different number of tokens. The following rules specify the requirements for the adapter tokens:

If the value itself contains a pipe character `|` the pipe must be escaped using a leading backslash `\`. In addition the entire value has to be wrapped in quotes:

```
        2009-06-15T00:00:00.000000|description|"Text with \| (pipe) character."
```

Conditions require six (6) fields as follows:

```
	<timestamp>|<data_item_name>|<level>|<native_code>|<native_severity>|<qualifier>|<message>
```
	
For a complete description of these fields, see the standard. An example line will look like this:

```
	2014-09-29T23:59:33.460470Z|htemp|WARNING|HTEMP|1|HIGH|Oil Temperature High
```
	
The next special format is the Message. There is one additional field, native_code, which needs to be included:

```
	2014-09-29T23:59:33.460470Z|message|CHG_INSRT|Change Inserts
```
	
Time series data also gets special treatment, the count and optional frequency are specified. In the following example we have 10 items at a frequency of 100hz:

```
	2014-09-29T23:59:33.460470Z|current|10|100|1 2 3 4 5 6 7 8 9 10
```

The data item name can also be prefixed with the device name if this adapter is supplying data to multiple devices. The following is an example of a power meter for three devices named `device1`, `device2`, and `device3`:

```
	2014-09-29T23:59:33.460470Z|device1:current|12|device2:current|11|device3:current|10
```

All data items follow the formatting requirements in the MTConnect standard for the vocabulary and coordinates like PathPosition.

A new feature introduced in version 1.4 is the ability to announce a reset has been triggered. If we have a part count named `pcount` that gets reset daily, the new protocol is as follows:

```
	2014-09-29T23:59:33.460470Z|pcount|0:DAY
```
	
To specify the duration of the static, indicate it with an `@` sign after the timestamp as follows:

```
	2014-09-29T23:59:33.460470Z@100.0|pcount|0:DAY
```
	
### `DATA_SET` Representation

A new feature in version 1.5 is the `DATA_SET` representation which allows for key value pairs to be given. The protocol is similar to time series where each pair is space delimited. The agent automatically removes duplicate values from the stream and allows for addition, deletion and resetting of the values. The format is as follows:

```
	2014-09-29T23:59:33.460470Z|vars|v1=10 v2=20 v3=30
```

This will create a set of three values. To remove a value

```
	2014-09-29T23:59:33.460470Z|vars|v2 v3=
```

This will remove v2 and v3. If text after the equal `=` is empty or the `=` is not given, the value is deleted. To clear the set, specify a `resetTriggered` value such as `MANUAL` or `DAY` by preceeding it with a colon `:` at the beginning of the line.

```
	2014-09-29T23:59:33.460470Z|vars|:MANUAL
```

This will remove all the values from the current set. The set can also be reset to a specific set of values:

```
	2014-09-29T23:59:33.460470Z|vars|:MANUAL v5=1 v6=2
```

This will remove all the old values from the set and set the current set. Values will accumulate when addition pairs are given as in:

```
	2014-09-29T23:59:33.460470Z|vars|v8=1 v9=2 v5=10
```

This will add values for v8 and v9 and update the value for v5 to 10. If the values are duplcated they will be removed from the stream unless a `resetTriggered` value is given.

```
	2014-09-29T23:59:33.460470Z|vars|v8=1 v9=2 v5=0
```

Will be detected as a duplicate with respect to the previous values and will be removed. If a partial update is given and the other values are duplicates, then will be stripped:

```
	2014-09-29T23:59:33.460470Z|vars|v8=1 v9=3 v5=10
```

Will be effectively the same as specifying:

```
	2014-09-29T23:59:33.460470Z|vars|v9=2
```

And the streams will only have the one value when a sample is request is made at that point in the stream.

When the `discrete` flag is set to `true` in the data item, all change tracking is ignored and each set is treated as if it is new.

One can quote values using the following methods: `"<text>"`, `'<text>'`, and `{<text>}`. Spaces and other characters can be included in the text and the matching character can be escaped with a `\` if it is required as follows: `"hello \"there\""` will yield the value: `hellow "there"`.

### `TABLE` Representation

A `TABLE` representation is similar to the `DATA_SET` that has a key value pair as the value. Using the encoding mentioned above, the following representations are allowed for tables:

```
    <timestamp>|wpo|G53.1={X=1.0 Y=2.0 Z=3.0 s='string with space'} G53.2={X=4.0 Y=5.0 Z=6.0} G53.3={X=7.0 Y=8.0 Z=9 U=10.0}
```

Using the quoting conventions explained in the previous section, the inner content can contain quoted text and exscaped values. The value is interpreted as a key/value pair in the same way as the `DATA_SET`. A table can be thought of as a data set of data sets.

All the reset rules of data set apply to tables and the values are treated as a unit.

### Assets

Assets are associated with a device but do not have a data item they are mapping to. They therefore get the special data item name `@ASSET@`. Assets can be sent either on one line or multiple lines depending on the adapter requirements. The single line form is as follows:

```
	2012-02-21T23:59:33.460470Z|@ASSET@|KSSP300R.1|CuttingTool|<CuttingTool>...
```

This form updates the asset id KSSP300R.1 for a cutting tool with the text at the end. For multiline assets, use the keyword `--multiline--` with a following unique string as follows:

```
		2012-02-21T23:59:33.460470Z|@ASSET@|KSSP300R.1|CuttingTool|--multiline--0FED07ACED
		<CuttingTool>
		...
		</CuttingTool>
		--multiline--0FED07ACED
```
		
The terminal text must appear on the first position after the last line of text. The adapter can also remove assets (1.3) by sending a @REMOVE_ASSET@ with an asset id:

```
	2012-02-21T23:59:33.460470Z|@REMOVE_ASSET@|KSSP300R.1
```

Or all assets can be removed in one shot for a certain asset type:

```
	2012-02-21T23:59:33.460470Z|@REMOVE_ALL_ASSETS@|CuttingTool
```

Partial updates to assets is also possible by using the @UPDATE_ASSET@ key, but this will only work for cutting tools. The asset id needs to be given and then one of the properties or measurements with the new value for that entity. For example to update the overall tool length and the overall diameter max, you would provide the following:

```
	2012-02-21T23:59:33.460470Z|@UPDATE_ASSET@|KSSP300R.1|OverallToolLength|323.64|CuttingDiameterMax|76.211
```

### Commands

There are a number of commands that can be sent as part of the adapter stream. These change some dynamic elements of the device information, the interpretation of the data, or the associated default device. Commands are given on a single line starting with an asterisk `* ` as the first character of the line and followed by a <key>: <value>. They are as follows:

* Specify the Adapter Software Version the adapter supports:

	`* adapterVersion: <version>`

* Set the calibration in the device component of the associated device:
 
	`* calibration: XXX`

* Tell the agent that the data coming from this adapter requires conversion:
 
	`* conversionRequired: <yes|no>`

* Specify the default device for this adapter. The device can be specified as either the device name or UUID:

	`* device: <uuid|name>`

* Set the description in the device header of the associated device:
 
	`* description: XXX`

* Set the manufacturer in the device header of the associated device:
 
	`* manufacturer: XXX`

* Specify the MTConnect Version the adapter supports:

	`* mtconnectVersion: <version>`

* Set the nativeName in the device component of the associated device:
 
	`* nativeName: XXX`

* Tell the agent that the data coming from this adapter would like real-time priority:
 
	`* realTime: <yes|no>`

* Tell the agent that the data coming from this adapter is specified in relative time:
 
	`* relativeTime: <yes|no>`

* Set the serialNumber in the device header of the associated device:
 
	`* serialNumber: XXX`

* Specify the version of the SHDR protocol delivered by the adapter. See `ShdrVersion` above:

	`* shdrVersion: <version>`

* Set the station in the device header of the associated device:
 
	`* station: XXX`

* Set the device model of the associated device:
	`* deviceModel: --multiline--AAAAA\n<XML Fragment>\n--multiline--AAAAA`
  * Note: The XML is expected to be an XML fragment of the `<Device />` intended to be injected into the local device configuration.
  * *WARNING*: If the device configuration is empty, this SHDR command **MUST** be issued before other commands such as `* mtconnectVersion` or `* device`. The only exception is if a device is appropriately defined in the local device configuration *before* the Agent connectes to an adapter.

Any other command will be logged as a warning.

### Protocol

The agent and the adapter have a heartbeat that makes sure each is responsive to properly handle disconnects in a timely manner. The Heartbeat frequency is set by the adapter and honored by the agent. When the agent connects to the adapter, it first sends a `* PING` and then expects the response `* PONG <timeout>` where `<timeout>` is specified in milliseconds. So if the following communications are given:

Agent:

```
	* PING
```

Adapter:

```
	* PONG 10000
```

This indicates that the adapter is expecting a `PING` every 10 seconds and if there is no `PING`, in 2x the frequency, then the adapter should close the connection. At the same time, if the agent does not receive a `PONG` within 2x frequency, then it will close the connection. If no `PONG` response is received, the agent assumes the adapter is incapable of participating in heartbeat protocol and uses the legacy time specified above.
	
Just as with the SHDR protocol, these messages must end with an LF (ASCII 10) or CR-LF (ASCII 15 followed by ASCII 10).

#### HTTP PUT/POST Method of Uploading Data

There are two configuration settings mentioned above: `AllowPut` and `AllowPutFrom`. `AllowPut` alone will allow any process to use `HTTP` `POST` or `PUT` to send data to the agent and modify values. To restrict this to a limited number of machines, you can list the IP Addresses that are allowed to `POST` data to the agent. 

An example would be:

```
    AllowPut = yes
    AllowPutFrom = 192.168.1.72, 192.168.1.73
```
  
This will allow the two machines to post data to the MTConnect agent. The data can be either data item values or assets. The primary use of this capability is uploading assets from a process or even the command line using utilities like curl. I'll be using curl for these examples.

For example, with curl you can use the -d option to send data to the server. The data will be in standard form data format, so all you need to do is to pass the `<data_item_name>=<data_item_value>` to set the values, as follows:

```
    curl -d 'avail=AVAILABLE&program_1=XXX' 'http://localhost:5000/ExampleDevice'
```
    
By specifying the device at the end of the URL, you tell the agent which device to use for the POST. This will set the availability tag to AVAILABLE and the program to XXX:

```XML
    <Availability dataItemId="dtop_3" timestamp="2015-05-18T18:20:12.278236Z" name="avail" sequence="65">AVAILABLE</Availability>
    ...
    <Program dataItemId="path_51" timestamp="2015-05-18T18:20:12.278236Z" name="program_1" sequence="66">XXX</Program>
```
    
The full raw data being passed over looks like this:

```
    => Send header, 161 bytes (0xa1)
    0000: POST /ExampleDevice HTTP/1.1
    001e: User-Agent: curl/7.37.1
    0037: Host: localhost:5000
    004d: Accept: */*
    005a: Content-Length: 29
    006e: Content-Type: application/x-www-form-urlencoded
    009f: 
    => Send data, 29 bytes (0x1d)
    0000: avail=AVAILABLE&program_1=XXX
    == Info: upload completely sent off: 29 out of 29 bytes
    == Info: HTTP 1.0, assume close after body
    <= Recv header, 17 bytes (0x11)
    0000: HTTP/1.0 200 OK
    <= Recv header, 20 bytes (0x14)
    0000: Content-Length: 10
    <= Recv header, 24 bytes (0x18)
    0000: Content-Type: text/xml
    <= Recv header, 2 bytes (0x2)
    0000: 
    <= Recv data, 10 bytes (0xa)
    0000: <success/>
    >== Info: Closing connection 0
```
    
This is using the --trace - to dump the internal data. The response will be a simple `<success/>` or `<fail/>`.
    
Any data item can be set in this fashion. Similarly conditions are set using the following syntax:

```
    curl -d 'system=fault|XXX|1|LOW|Feeling%20low' 'http://localhost:5000/ExampleDevice'
```
    
One thing to note, the data and values are URL encoded, so the space needs to be encoded as a %20 to appear correctly. 

```XML
    <Fault dataItemId="controller_46" timestamp="2015-05-18T18:24:48.407898Z" name="system" sequence="67" nativeCode="XXX" nativeSeverity="1" qualifier="LOW" type="SYSTEM">Feeling Low</Fault>
```

Assets are posted in a similar fashion. The data will be taken from a file containing the XML for the content. The syntax is very similar to the other requests:

```
    curl -d @B732A08500HP.xml 'http://localhost:5000/asset/B732A08500HP.1?device=ExampleDevice&type=CuttingTool'
```

The @... uses the named file to pass the data and the URL must contain the asset id and the device name as well as the asset type. If the type is CuttingTool or CuttingToolArchetype, the data will be parsed and corrected if properties are out of order as with the adapter. If the device is not specified and there are more than one device in this adapter, it will cause an error to be returned.

Programmatically, send the data as the body of the POST or PUT request as follows. If we look at the raw data, you will see the data is sent over verbatim as follows: 

```
    => Send header, 230 bytes (0xe6)
    0000: POST /asset/B732A08500HP.1?device=ExampleDevice&type=CuttingTool
    0040:  HTTP/1.1
    004b: User-Agent: curl/7.37.1
    0064: Host: localhost:5000
    007a: Accept: */*
    0087: Content-Length: 2057
    009d: Content-Type: application/x-www-form-urlencoded
    00ce: Expect: 100-continue
    00e4: 
    == Info: Done waiting for 100-continue
    => Send data, 2057 bytes (0x809)
    0000: (file data sent here, see below...)
    == Info: HTTP 1.0, assume close after body
    <= Recv header, 17 bytes (0x11)
    0000: HTTP/1.0 200 OK
    <= Recv header, 20 bytes (0x14)
    0000: Content-Length: 10
    <= Recv header, 24 bytes (0x18)
    0000: Content-Type: text/xml
    <= Recv header, 2 bytes (0x2)
    0000: 
    <= Recv data, 10 bytes (0xa)
    0000: <success/>
    <success/>== Info: Closing connection 0
```

The file that was included looks like this:

```XML
    <CuttingTool serialNumber="1 " toolId="B732A08500HP" timestamp="2011-05-11T13:55:22" assetId="B732A08500HP.1" manufacturers="KMT">
    	<Description>
    		Step Drill KMT, B732A08500HP Grade KC7315
    		Adapter KMT CV50BHPVTT12M375
    	</Description>
    	<CuttingToolLifeCycle>
    		<CutterStatus><Status>NEW</Status></CutterStatus>
    		<ProcessSpindleSpeed nominal="5893">5893</ProcessSpindleSpeed>
    		<ProcessFeedRate nominal="2.5">2.5</ProcessFeedRate>
    		<ConnectionCodeMachineSide>CV50 Taper</ConnectionCodeMachineSide>
    		<Measurements>
    			<BodyDiameterMax code="BDX">31.8</BodyDiameterMax>
    			<BodyLengthMax code="LBX" nominal="120.825" maximum="126.325" minimum="115.325">120.825</BodyLengthMax>
    			<ProtrudingLength code="LPR" nominal="155.75" maximum="161.25" minimum="150.26">158.965</ProtrudingLength>
    			<FlangeDiameterMax code="DF" nominal="98.425">98.425</FlangeDiameterMax>
    			<OverallToolLength nominal="257.35" minimum="251.85" maximum="262.85" code="OAL">257.35</OverallToolLength>
    		</Measurements>
    		<CuttingItems count="2">
    			<CuttingItem indices="1" manufacturers="KMT" grade="KC7315">
    				<Measurements>
    					<CuttingDiameter code="DC1" nominal="8.5" maximum="8.521" minimum="8.506">8.513</CuttingDiameter>
    					<StepIncludedAngle code="STA1" nominal="90" maximum="91" minimum="89">89.8551</StepIncludedAngle>
    					<FunctionalLength code="LF1" nominal="154.286" minimum="148.786" maximum="159.786">157.259</FunctionalLength>
    					<StepDiameterLength code="SDL1" nominal="9">9</StepDiameterLength>
    					<PointAngle code="SIG" nominal="135" minimum="133" maximum="137">135.1540</PointAngle>
    				</Measurements>
    			</CuttingItem>
    			<CuttingItem indices="2" manufacturers="KMT" grade="KC7315">
    				<Measurements>
    					<CuttingDiameter code="DC2" nominal="12" maximum="12.011" minimum="12">11.999</CuttingDiameter>
    					<FunctionalLength code="LF2" nominal="122.493" maximum="127.993" minimum="116.993">125.500</FunctionalLength>
    					<StepDiameterLength code="SDL2" nominal="9">9</StepDiameterLength>
    				</Measurements>
    			</CuttingItem>
    		</CuttingItems>
    	</CuttingToolLifeCycle>
    </CuttingTool>
```

An example in ruby is as follows:

```
    > require 'net/http'
    => true
    > h = Net::HTTP.new('localhost', 5000)
    => #<Net::HTTP localhost:5000 open=false>
    > r = h.post('/asset/B732A08500HP.1?type=CuttingTool&device=ExampleDevice', File.read('B732A08500HP.xml'))
    => #<Net::HTTPOK 200 OK readbody=true>
    > r.body
    => "<success/>"
```
  
## Creating Test Certifications (see resources gen_certs shell script)

This section assumes you have installed openssl and can use the command line. The subject of the certificate is only for testing and should not be used in production. This section is provided to support testing and verification of the functionality. A certificate provided by a real certificate authority should be used in a production process.

> NOTE: The certificates must be generated with OpenSSL version 1.1.1 or later. LibreSSL 2.8.3 is not compatible with 
      more recent version of SSL on raspian (Debian).

### Server Creating self-signed certificate chain

#### Create Signing authority key and certificate

```
	openssl req -x509 -nodes -sha256 -days 3650 -newkey rsa:3072 -keyout rootca.key -out rootca.crt -subj "/C=US/ST=State/L=City/O=Your Company, Inc./OU=IT/CN=serverca.org"
```

#### User Key

```
    openssl genrsa -out user.key 3072
```

##### Signing Request

```
    openssl req -new -sha256 -key user.key -out user.csr -subj "/C=US/ST=State/L=City/O=Your Company, Inc./OU=IT/CN=user.org"
```

##### User Certificate using root signing certificate

```
    openssl x509 -req -in user.csr -CA rootca.crt -CAkey rootca.key -CAcreateserial -out user.crt -days 3650
```

#### Create DH Parameters

```
    openssl dhparam -out dh2048.pem 3072
```

#### Verify

```
    openssl verify -CAfile rootca.crt rootca.crt
    openssl verify -CAfile rootca.crt user.crt
```

### Client Certificate

#### Create Signing authority key

```
    openssl req -x509 -nodes -sha256 -days 3650 -newkey rsa:3072 -keyout clientca.key -out clientca.crt -subj "/C=US/ST=State/L=City/O=Your Company, Inc./OU=IT/CN=clientca.org"
```

#### Create client key

```
    openssl genrsa -out client.key 3072
```

#### Create client signing request

```
    openssl req -new -key client.key -out client.csr -subj "/C=US/ST=State/L=City/O=Your Company, Inc./OU=IT/CN=client.org"
```

#### Create Client Certificate

For client.cnf

```
    basicConstraints = CA:FALSE
    nsCertType = client, email
    nsComment = "OpenSSL Generated Client Certificate"
    subjectKeyIdentifier = hash
    authorityKeyIdentifier = keyid,issuer
    keyUsage = critical, nonRepudiation, digitalSignature, keyEncipherment
    extendedKeyUsage = clientAuth, emailProtection
```

#### Create the cert

```
    openssl x509 -req -in client.csr -CA clientca.crt -CAkey clientca.key -out client.crt -CAcreateserial -days 3650 -sha256 -extfile client.cnf
```

#### Verify

```
    openssl verify -CAfile clientca.crt clientca.crt
    openssl verify -CAfile clientca.crt client.crt
```