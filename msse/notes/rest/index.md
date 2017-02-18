---
title: Web Application Development
layout: default
---
autoscale: true
theme: Next, 3

# REST

---

# Defining Web Services
- System-to-system integration using HTTP
- Goal: cross-platform API consumption
- Originally defined using XML-based protocols
- Language and system neutral

---

# Old School Web Services
- XML-Based
- Began to emerge at the dawn of the century
- Developed by the W3C Working Group and industry consortiums

---

# Elements of XML Web Services
- Define standard request and response XML structures
- SOAP - Simple Object Access Protocol
- WSDL - Web Services Description Language
- UDDI - Universal Description, Discovery and Integration

---

# Benefits of XML Web Services
- Good tooling support due to strict XML-schema-based protocol
- Well-structured payloads could be validated minimizing errors
- First viable attempt at cross platform interoperability
- Still used in many companies today

---

# Problems with XML Web Services
- XML payloads proved to be too large:
  - Excessive network load
  - Difficult to read and debug issues
- Large standards bodies moved to slow
  - Required too much buy in from too many parties

---

# REST and JSON

---

# REST
- Representational State Transfer
- Architectural style based strictly on HTTP and a common pattern
- Convention for defining URI mappings to resources
- HTTP method (verb) defines the action being requested by the client

---

# Resources
- Key concept in REST
- Data of value at the end of a URL
- Individual item
- Collections
- Think Domain classes

---

# REST Constraints
- Client-server
- Stateless
- Cacheable
- Uniform interface
- Layered System
- Code-On-Demand

---

# Uniform Interface
- Self-descriptive messages (MIME types)
- Resource identifiers as an endpoint
- HTTP methods are used to signify intent

---

# HTTP Methods
- GET - retrieve resource
- PUT - replace (update) resource
- POST - create new single resource
- DELETE - delete resource

---

# Method Characteristics
- GET is safe
  - Calling it produces no side-effects
- PUT and DELETE are idempotent
  - Calling it repeatedly will produce the same result

---

# URI Conventions
- Collection: /resources/
  - GET: return a list
  - POST: create a new item
- Items: /resources/id
  - GET: return specific resource
  - DELETE: delete specific resource
  - PUT: update specific resource

---

# Good URI Rules
- Unique to a resource
- Long-lived
  - Always point to the same resource

---



# REST Data Formats
- REST only addressed part of the problem with XML Web Services
  - Complexity
- The rest of the problem lies in the complexity of the request and response payloads
- REST can be implemented with XML
- To complete the move to simpler APIs implement REST with JSON

---

# JSON
- JavaScript Object Notation
- Lightweight data format
  - Syntax, data types, format
- Only two structures:
  - Objects and Lists
- Maps nicely to REST

---

# JSON Value Types
- string
- number
- object
- array
- null
- true
- false

---

# JSON Object Format
- String value mapping inside curly braces

``` javascript
{
  "name": "Bill",
  "age": 66,
  "golfer": true,
  "address": {
    "country": "USA"
  }
}
```

---

# JSON List format
- Comma-separated values inside square brackets

``` javascript
[ 1, 2, 3, 'a', false]

[{"name": "Mike"}, {"name": "Lars"}]
```

---

# REST Considerations
- Updates to domain model may unintentionally break clients
- Timeline for API changes may be different than timeline for business or domain  changes
- May not want to expose all fields of domain model to clients
- Error handling/reporting a bit generic
- Parent child representation can be challenging

---

# Approaches to Detaching API Payload
- Programmatically convert domain into payload
  - Builder patterns can be useful here
- Create separate model for API payload (DTO approach)
- Modify the marshalling of the domain into destination format

---

# Return Proper Status
- 200 : OK - response handled properly
- 201 : Created - return after a successful POST
- 400 : Bad Request - invalid parameters
- 403 : Unauthorized - failure to authenticate, not logged in
- 403 : Forbidden - missing required permission
- 404 : Not Found - item requested is missing
- 415 : Unsupported Media Type - data not in correct format
- 422 : Unprocessable Entity - failed validation

---

# REST API Security
- Protect access
  - Suggested: Basic HTTP Authentication
- Know who is making calls
  - Reporting
  - Throttling
  - Suggested: API Key

---

# Basic HTTP Authentication
- Client includes credentials in the header of every request
- Must use HTTPS to prevent sending of plain text credentials
- Spring Security provides an implementation of Basic HTTP Authentication

---

# API Versioning
- New data formats may break existing clients
- Several options for solving
  - Create new URL with a version URL path (/v2/songs)
  - Rely on a request header from client to format the proper response
  - Create a custom MIME type and respond with different renderings for each
    - Honor the Accept HTTP header

---

# REST Not Just for APIs
- Single Page commonly use REST within an app
- UIs are plain HTML and JavaScript (no server rendering)
- Server sends/receives data from browser via REST

---

# Final Notes on REST
- Use the proper HTTP methods and response status code
- Use the header for client identification
- All parameters (other than id) should be specified as query parameters
- Use Postman (Chrome plugin) to quick test/debug non GET calls
- REST/JSON competitors exist and are becoming popular

---

# Alternatives to REST/JSON

- Alternatives exist to go back to the RPC style of things
- Most offer backwards versioning and similar/better compression over HTTP
- Examples
  - Protocol Buffers (Google)
  - Thrift (Facebook)
  - Avro (Apache)

- [gRPC](https://github.com/grpc/grpc) Google's RPC exchange over HTTP2 (using protobufs)
