footer: © Citronella Software Ltd 2015
slidenumbers: true

# REST
## Mike Calvo
### mike@citronellasoftware.com

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
  - In comparison to CORBA
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
## The dynamic duo to the rescue!

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

```
{
  'name': 'Mike',
  'age': 43,
  'nerd': true
  'address': {
    'country': 'USA'
  }
}
```

---
# JSON List format
- Comma-separated values inside square brackets

```
[ 1, 2, 3, 'a', false]

[{'name': 'Mike'}, {'name': 'Lars'}]
```
---
# XML versus JSON
- World is moving away from XML
- Reduced complexity and smaller payloads is driving this
- XML is a pain to process in just about every language
- XML still exists in the wild but primarily in legacy systems
- Bad smell if an external system is only XML-based

---
# Grails and REST
- Grails has several features to simplify creating RESTful APIs
- @Resource Annotation
- Controller actions
- Custom Url Mappings
- JSON and XML rendering

---
# @Resource
- Annotation for domain classes
- Creates implicit controller responding to REST requests
- Actions mapped to the standard REST HTTP methods

```
@Resource(uri='/posts')
class Post {

}
```

---
# Muzic Song REST Resource
- Make the Song domain class available for REST calls

---
# UrlMappings/RestController Approach
- grails-app/conf/UrlMappings.groovy contains definitions for how URLs are mapped to controllers
- URLs can be mapped as 'resources' or 'resource' in this file:
  `"/api/artists"(resources: 'artistRest')` for resources with multiple values
  `"/api/config"(resource: 'configRest')` for resources with a single value
- Specify the controller name for the resource
- Create this contorller and make it a subclass of RestfulController

---
# Muzic Artist REST Resource
- Add an Artist resource via the UrlMappings/RestController approach
- Add a unit test for the controller verifying the REST methods work

---
# Custom Grails REST Services
- Like scaffolding - simple Grails REST support is great for getting things up and running
- Like scaffolding it lacks the flexibility to be useful in most real-world situations

---
# Limitations of @Resource and RestfulController Approaches
- Response payloads too closely tied to domain model
  - Updates to domain model may unintentionally break clients
  - Timeline for API changes may be different than timeline for business or domain  changes
  - May not want to expose all fields of domain model to clients
  - Includes implementation-specific fields like the Groovy Type
- Error handling/reporting a bit generic

---
# Approaches to Detaching API Payload
- Programmatically convert domain into payload
  - Builder patterns can be useful here
- Create seperate model for API payload (DTO approach)
- Modify the marshalling of the domain into destination format

---
# Registering a JSON Marshaller
- Code can be tied in via Bootstrap or a custom Spring bean

```
JSON.registerObjectMarshaller(Song) { Song s ->
  return [ id: s.id, title: s.title, artist: s.artist.name ]
}
```

---
# Renders versus Converters
- Both are responsible for serializing onjects to JSON or XML
- Converters are the built-in Grails mechanism
- Renderers are external libraries or custom approaches
  - Common library for JSON serialization in Java is Jackson

---
# Custom REST Controller
- Most of the time, real-world situations require implementing your own controller to handle REST requests
- Good news: only 4 actions required
- Handy method: respond
  - Renders the argument to the response
  - Allows for a HTTP response status to be specified

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
# Protect Access to Proper HTTP Method
- In custom controller:

```
static allowedMethods = [save: "POST", update: "PUT", patch: "PATCH", delete: "DELETE"]
```

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
# Custom Marshalling
- Grails allows for multiple marshallers to be registered per format type
- Perform this registration at startup (prior to any requests being processed)
- The configuration to use can specified at response render time

---
# Register Custom Marshalling Example

```
JSON.createNamedConfig('v1') { cfg ->
  cfg.registerObjectMarshaller(Post) { Post p ->
    return { message: p.content, user: p.user.loginId }
  }
}
JSON.createNamedConfig('v2') { cfg ->
  cfg.returnObjectMarsaller(Post) { Post p ->
    return [
      message: p.content,
      user: [id: p.user.loginId, name: p.user.name]
    ]
  }
}
```

---
# Choose Marshaller Example

```
class PostController {
  def index(String v) {
    assert !v || v == '1' || v == '2'
    def config = 'v' + (v ?: 1) // v1, v2, etc..
    JSON.use(config) {
      response Post.list()
    }
  }
}
```

---
# REST Not Just for APIs
- Single Page commonly use REST within an app
- UIs are plain HTML and JavaScript (no server rendering)
- Server sends/receives data from browser via REST
- Mobile applications also communicate with cloud components

---
# Final Notes on REST
- Use the proper HTTP methods and response status code
- Use the header for client identification
- All parameters (other than id) should be specified as query parameters
- Use Postman (Chrome plugin) to quick test/debug non GET calls
