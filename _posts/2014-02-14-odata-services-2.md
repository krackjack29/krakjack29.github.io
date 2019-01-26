---
title: Introduction to OData
created: 2014/02/14 17:08:00
layout: single
comments: true
categories: 
   - Tech
tags:
   - tools
   - software
   - sdk
---

>"OData is a standardized protocol for creating and consuming data APIs. OData builds on core protocols like HTTP and commonly accepted methodologies like REST. The result is a uniform way to expose full-featured data APIs. – http://www.Odata.org “


That’s the definition available on the official website, it’s a protocol (funded and professed by Microsoft) that allows clients application to read, update or create data through HTTP (a Url) !! Smile really that’s it. The data is transmitted over HTTP in the known format of XML, Json, Atom etc. based on the implementation.

A sample Uri to select entity which is exposed as OData would be

http://{server-endpoint}/{entity-Name}   — This would give the list of all entities (if paginated includes continuation token for the next list).

http://{server-endpoint}/{entity-Name}(key) – Gets a single entity based on the key passed.

http://{server-endpoint}/{entity-Name}?$select={comma separated field names} – Gets single entity with only the fields mentioned in the select query parameter.

There are few more special query parameters like $expand, $skip, $format etc. you can read about them all at [here](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/)

### Why OData
The number of clients accessing the data have varied (smartphones, web, desktop, tablets, other web services) so has the technologies the clients are built in, so we needed a protocol which would send the data on standard format such as Json or Xml, yes we do have SOAP protocol but that involves a considerable overhead of adding the soap headers etc.

The data format is simple Json or XML hence there should no additional serialization protocols needed for cross technology data access. e.g. a Odata service written in .Net can be consumed by a javascript as a json and convert that to a javascript object model.

Most webservices serve one purpose i.e. exposing data to the consumers (some may have high end business rules, which isn’t really recommended) hence this protocol becomes more relevant especially when you are creating a webservice as just data provider.

In my next blog I would cover how to create a OData service and consume it in .Net using WCF services.