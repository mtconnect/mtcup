---
title: Extending the Schema
permalink: /Extending_the_Schema/
---

MTConnect was designed with various extension points that are created as
substitution groups. These extension points allow for new elements to be
created that have certain properties that must be inherited from the
parent to provide consistency for an application. There are three places
where the MTConnect schema can be extended using an additional
namespace: 1. New components can be added, 2. new data item types can be
added, and 3. new asset types or measurements can be added. Each
requires extending one of the three main XSD schema file. The XSD can be
extended using a separate namespace in a separate XSD file and importing
the base MTConnect XSD or you can use the schema generator to create the
file for you.

## Adding a new Component

## Adding a new Data Item Type

The header must look something like the following:

``` XML numberLines
<xs:schema xmlns:xs='http://www.w3.org/2001/XMLSchema' ns='urn:example.com:ExampleStreams:1.2'
   xmlns:e='urn:example.com:ExampleStreams:1.2'
  targetNamespace='urn:example.com:ExampleStreams:1.2' elementFormDefault='qualified'
  attributeFormDefault='unqualified' xmlns:mt='urn:mtconnect.org:MTConnectStreams:1.2'>
  <xs:import namespace='urn:mtconnect.org:MTConnectStreams:1.2'
                    schemaLocation='/schema/MTConnectStreams_1.2.xsd'/>
```

The header needs to be modified slightly to include the new namespace
`e` and MTConnect must remain the default namespace. The statements on
lines 2 and 3 show how this is done. The MTConnect namespace `m` should
always be assigned to MTConnect, this will make sure the agent operates
correctly when generating XML.

The schema locations should also point to the local storage for these
XSD files. It is best to have the agent serve the files up directly so
there are no version issues when you deploy the files. I will cover
agent configuration later in the document.

Line \#5 contains the import statement for the MTConnect schema. This
will include the base schema into the extended schema and resolve all
the references. Specifying it as in the header will work with all
validation tools with the C++ Reference MTConnect agent.

Next we are going to add a new data item called CommonVariable. The
first thing we need to add is a type signature for the common variable.
This is going to be a floating point value or UNAVAILABLE. The value can
be anything you like, the only caveat is that Samples MUST be floating
point numbers and Events are just text.

``` XML numberLines
  <xs:complexType name='CommonVariableType'>
    <xs:annotation>
      <xs:documentation>
        A variable value
      </xs:documentation>
    </xs:annotation>
    <xs:simpleContent>
      <xs:restriction base='mt:EventType'>
        <xs:pattern value='[+-]?\d+(\.\d+)?(E[+-]?\d+)?|UNAVAILABLE'/>
      </xs:restriction>
    </xs:simpleContent>
  </xs:complexType>
  <xs:element name='CommonVariable' type='CommonVariableType' substitutionGroup='mt:Event'>
    <xs:annotation>
      <xs:documentation>
        A variable value
      </xs:documentation>
    </xs:annotation>
  </xs:element>
```

We need to first create the CommonVariableType to show how to and we
specify the content as simpleContent meaning there is no structure, just
some data and we need to extend the mt:EventType which is the type for
the substitutionGroup mt:Event. mt is the namespace for the MTConnect
schema. The pattern we are going to use is a floating point number of
UNAVAILABLE, this is a restriction on the base xs:string type in Event.

The next piece declares the element with the name "CommonVariable" and
make it a member of the substitution group mt:Event. This allows us to
use this within the <Events>...</Events> area of the component stream.
The new entity can now be used by placing the following line in the
Devices.xml:

``` XML
   <DataItem type="x:COMMON_VARIABLE" category="EVENT" id="cv1" name="reg_1" />
```

There is no need to extend the Devices schema since the pattern for the
type includes the ability to use a namespace prefix as a way to indicate
that we are now in an extended space.

Once you have your XSD file completed, you will need to configure the
MTConnect agent to include this schema as the default when it serves the
XML documents. The following lines should be added to the agent
configuration:

``` numberLines
StreamsNamespaces {
  x {
    Urn = urn:example.com:ExampleStreams:1.2
    Location = /schemas/ExampleStreams_1.2.xsd
    Path = ./Schemas/ExampleStreams_1.2.xsd
  }
}

Files {
  schemas {
    Location = /schemas/
    Path = ./Schemas/
  }
}
```

The first section declares that there is a new namespace (we're calling
it `x` (it does not have to match the `e` above). The URN needs to match
URN as declared at the top of the previous xsd
(`ns='`<urn:example.com:ExampleStreams:1.2>`'` ) and the location will
tell it where the file lives when referenced in the XML document. The
Actual path of the file is in the Schema subdirectory.

The next Files section tells the agent that it should look for the
standard MTConnect schema files in the Schemas directory and prefix them
with the location /schema/. That allows it to serve these file correctly
if the they are request when referenced by the import above.

### Special DataItem Formats

The data contained in a data can be any format you require. The one
requirement is the special reserved word `UNAVAILABLE` is included. For
example, if you would like the have a data item with a pattern of
<name>:<value> pairs separated by a space, you would specify it as
follows:

``` XML
  <xs:pattern value="(\d+:[+-]?\d+(\.\d+)?(E[+-]?\d+)?)( \d+:[+-]?\d+(\.\d+)?(E[+-]?\d+)?)*|UNAVAILABLE"/>
```

This will allow us to create a document with an event like this:

``` XML
<e:CommonVariable sequence="1234" timestamp="2014-03-05T12:34:22Z" dataItemId="cv1">1:9 2:0 3:0 4:0 5:1.2356 6:0 7:0 8:0 9:0 10:9 11:0 12:0 13:0 14:0 15:0 16:0 17:0 18:0 19:0 20:0 21:0 22:0 23:0 24:0 25:0 26:0 27:0 28:0 29:0 30:0 31:0 32:0 33:0 34:0 35:0 36:0 37:0 38:0 39:0 40:0 41:0 42:0 43:0 44:0 45:0 46:0 47:0 48:0 49:0 50:0 51:191.34 52:0 53:0 54:0 55:0 56:0 57:0 58:0 59:0 60:0 61:0 62:0 63:0 64:0 65:0 66:0 67:0 68:0 69:0 70:0 71:0 72:0 73:0 74:0 75:0 76:0 77:0 78:0 79:0 80:0 81:0 82:0 83:0 84:0 85:0 86:0 87:0 88:0 89:0 90:0 91:0 92:0 93:0 94:0 95:0 96:0 97:0 98:0 99:0 100:0 101:191.34 102:0 103:0 104:0 105:0 106:0 107:0 108:0 109:0 110:0 111:0 112:0 113:0 114:0 115:0 116:0 117:0 118:0 119:0 120:0 121:0 122:0 123:0 124:0 125:0 126:0 127:0 128:0 129:0 130:0 131:0 132:0 133:0 134:0 135:0 136:0 137:0 138:0 139:0 140:0 141:0 142:0 143:0 144:0 145:0 146:0 147:0 148:0 149:0 150:0 151:191.34 152:0 153:0 154:0 155:0 156:0 157:0 158:0 159:0 160:0 161:0 162:0 163:0 164:0 165:0 166:0 167:0 168:0 169:0 170:0 171:0 172:0 173:0 174:0 175:0 176:0 177:0 178:0 179:0 180:0 181:0 182:0 183:0 184:0 185:0 186:0 187:0 188:0 189:0 190:0 191:0 192:0 193:0 194:0 195:0 196:0 197:0 198:0 199:0 200:0</e:CommonVariable>
```

Now to put it all together:

``` XML numberLines
<?xml version="1.0" encoding="UTF-8"?>
<!--
Copyright (c) 2008-2010, AMT – The Association For Manufacturing Technology (“AMT”)
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:
    * Redistributions of source code must retain the above copyright
      notice, this list of conditions and the following disclaimer.
    * Redistributions in binary form must reproduce the above copyright
      notice, this list of conditions and the following disclaimer in the
      documentation and/or other materials provided with the distribution.
    * Neither the name of the AMT nor the
      names of its contributors may be used to endorse or promote products
      derived from this software without specific prior written permission.

DISCLAIMER OF WARRANTY. ALL MTCONNECT MATERIALS AND SPECIFICATIONS PROVIDED
BY AMT, MTCONNECT OR ANY PARTICIPANT TO YOU OR ANY PARTY ARE PROVIDED "AS IS"
AND WITHOUT ANY WARRANTY OF ANY KIND. AMT, MTCONNECT, AND EACH OF THEIR
RESPECTIVE MEMBERS, OFFICERS, DIRECTORS, AFFILIATES, SPONSORS, AND AGENTS
(COLLECTIVELY, THE "AMT PARTIES") AND PARTICIPANTS MAKE NO REPRESENTATION OR
WARRANTY OF ANY KIND WHATSOEVER RELATING TO THESE MATERIALS, INCLUDING, WITHOUT
LIMITATION, ANY EXPRESS OR IMPLIED WARRANTY OF NONINFRINGEMENT,
MERCHANTABILITY, OR FITNESS FOR A PARTICULAR PURPOSE.

LIMITATION OF LIABILITY. IN NO EVENT SHALL AMT, MTCONNECT, ANY OTHER AMT
PARTY, OR ANY PARTICIPANT BE LIABLE FOR THE COST OF PROCURING SUBSTITUTE GOODS
OR SERVICES, LOST PROFITS, LOSS OF USE, LOSS OF DATA OR ANY INCIDENTAL,
CONSEQUENTIAL, INDIRECT, SPECIAL OR PUNITIVE DAMAGES OR OTHER DIRECT DAMAGES,
WHETHER UNDER CONTRACT, TORT, WARRANTY OR OTHERWISE, ARISING IN ANY WAY OUT OF
THIS AGREEMENT, USE OR INABILITY TO USE MTCONNECT MATERIALS, WHETHER OR NOT
SUCH PARTY HAD ADVANCE NOTICE OF THE POSSIBILITY OF SUCH DAMAGES.
-->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:vc="http://www.w3.org/2007/XMLSchema-versioning" ns="urn:example.com:ExampleStreams:1.3" xmlns:e="urn:example.com:ExampleStreams:1.3" xmlns:mt="urn:mtconnect.org:MTConnectStreams:1.3" targetNamespace="urn:example.com:ExampleStreams:1.3" elementFormDefault="qualified" attributeFormDefault="unqualified" vc:minVersion="1.1">
    <xs:import namespace="urn:mtconnect.org:MTConnectStreams:1.3" schemaLocation="/Schemas/MTConnectStreams_1.3.xsd"/>
    <xs:complexType name="CommonVariableType">
        <xs:annotation>
            <xs:documentation>
        A variable value
      </xs:documentation>
        </xs:annotation>
        <xs:simpleContent>
            <xs:restriction base="mt:EventType">
                <xs:pattern value="(\d+:[+-]?\d+(\.\d+)?(E[+-]?\d+)?)( \d+:[+-]?\d+(\.\d+)?(E[+-]?\d+)?)*|UNAVAILABLE"/>
            </xs:restriction>
        </xs:simpleContent>
    </xs:complexType>
    <xs:element name="CommonVariable" type="CommonVariableType" substitutionGroup="mt:Event">
        <xs:annotation>
            <xs:documentation>
        A variable value
      </xs:documentation>
        </xs:annotation>
    </xs:element>
</xs:schema>
```

And the Streams Document will look like this:

``` XML numberLines
<?xml version="1.0" encoding="UTF-8"?>
<!--Sample XML file generated by XMLSpy v2014 sp1 (x64) (http://www.altova.com)-->
<MTConnectStreams xmlns:m="urn:mtconnect.org:MTConnectStreams:1.3" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:example.com:ExampleStreams:1.3 /Streams/ExampleStream_1.3.xsd" ns="urn:mtconnect.org:MTConnectStreams:1.3"  xmlns:e="urn:example.com:ExampleStreams:1.3">
    <Header version="-" creationTime="2001-12-17T09:30:47Z" nextSequence="1" lastSequence="1" firstSequence="1" instanceId="1" sender="String" bufferSize="1">String</Header>
    <Streams>
        <DeviceStream name="String" uuid="String">
            <ComponentStream componentId="c1" component="Controller">
                <Events>
                    <e:CommonVariable sequence="1234" timestamp="2014-03-05T12:34:22Z" dataItemId="cv1">1:9 2:0 3:0 4:0 5:1.2356 6:0 7:0 8:0 9:0 10:9 11:0 12:0 13:0 14:0 15:0 16:0 17:0 18:0 19:0 20:0 21:0 22:0 23:0 24:0 25:0 26:0 27:0 28:0 29:0 30:0 31:0 32:0 33:0 34:0 35:0 36:0 37:0 38:0 39:0 40:0 41:0 42:0 43:0 44:0 45:0 46:0 47:0 48:0 49:0 50:0 51:191.34 52:0 53:0 54:0 55:0 56:0 57:0 58:0 59:0 60:0 61:0 62:0 63:0 64:0 65:0 66:0 67:0 68:0 69:0 70:0 71:0 72:0 73:0 74:0 75:0 76:0 77:0 78:0 79:0 80:0 81:0 82:0 83:0 84:0 85:0 86:0 87:0 88:0 89:0 90:0 91:0 92:0 93:0 94:0 95:0 96:0 97:0 98:0 99:0 100:0 101:191.34 102:0 103:0 104:0 105:0 106:0 107:0 108:0 109:0 110:0 111:0 112:0 113:0 114:0 115:0 116:0 117:0 118:0 119:0 120:0 121:0 122:0 123:0 124:0 125:0 126:0 127:0 128:0 129:0 130:0 131:0 132:0 133:0 134:0 135:0 136:0 137:0 138:0 139:0 140:0 141:0 142:0 143:0 144:0 145:0 146:0 147:0 148:0 149:0 150:0 151:191.34 152:0 153:0 154:0 155:0 156:0 157:0 158:0 159:0 160:0 161:0 162:0 163:0 164:0 165:0 166:0 167:0 168:0 169:0 170:0 171:0 172:0 173:0 174:0 175:0 176:0 177:0 178:0 179:0 180:0 181:0 182:0 183:0 184:0 185:0 186:0 187:0 188:0 189:0 190:0 191:0 192:0 193:0 194:0 195:0 196:0 197:0 198:0 199:0 200:0</e:CommonVariable>
                </Events>
            </ComponentStream>
        </DeviceStream>
    </Streams>
</MTConnectStreams>
```