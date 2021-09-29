---
title: HTTP Error
description: 
published: true
date: 2021-09-24T00:31:36.768Z
tags: 
editor: markdown
dateCreated: 2021-09-24T00:31:34.779Z
---

Here is an example of an HTTP error:

``` xml numberLines
HTTP/1.1 200 Success
Content-Type: text/xml; charset=UTF-8
Server: Agent
Date: Sun, 23 Dec 2007 21:10:19 GMT

<?xml version="1.0" encoding="UTF-8"?>
<MTConnectError ns="urn:mtconnect.org:MTConnectError:1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:mtconnect.org:MTConnectError:1.1 http://www.mtconnect.org/schemas/MTConnectError_1.1.xsd">
  <Header creationTime="2010-03-12T12:33:01" sender="localhost" version="1.1" bufferSize="131000" instanceId="1" />
    <Errors>
      <Error errorCode="OUT_OF_RANGE" >Argument was out of range</Error>
      <Error errorCode="INVALID_XPATH" >Bad path</Error>
    </Errors>
</MTConnectError>
```