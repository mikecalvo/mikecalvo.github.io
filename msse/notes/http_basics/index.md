---
title: Web Application Development
layout: default
---
autoscale: true
theme: Next, 3

# HTTP Basics

---

# HTTP
- Hypertext Transfer Protocol
- Protocol by which web browsers communicate with web servers
- Very old
  - First version (.9) published in 1991
- Very stable
  - Standard is version 1.1 published in 1999

---

# HTTP Basics
- Server listens for requests on a port
  - Default is 80
- Secure version requires encrypted requests and responses (HTTPS)
  - Default port is 443
- Request and Response structure

---

# HTTP 1.1 Characteristics
- Text-based
- Stateless
- Ordered
- Blocking

---

# HTTP Request Format
- Request method
- URI (Uniform Resource Identifier)
- Protocol version (HTTP/1.1)
- Optional Headers
- Optional Body

---

# Http Request Example

GET /v1/pdp/tcin/31173465 HTTP/1.1
Host: someserver.com
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.95 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Encoding: gzip, deflate, sdch, br
Accept-Language: en-US,en;q=0.8
Cookie: adpakalb=p_csp1usw2;

---

# Request Methods
- Most commonly used methods
  - GET: Gets the resource at the URI
  - PUT: Request to store data at the specified URI
  - POST: Request a new resource be stored at the specified URI
- Other less frequently used methods
  -HEAD, DELETE, OPTIONS, TRACE

---

# HTTP Response Format
- Protocol version (HTTP/1.1)
- Status code and reason
- Response headers
  - Meta-information about response
- Response message body

---

# Example Response

HTTP/1.1 200 OK
Accept-Ranges: bytes
Access-Control-Allow-Headers: X-Auth-Token, Content-Type, X-Requested-With
Access-Control-Allow-Methods: GET
Access-Control-Allow-Origin: *
Cache-Control: max-age=0, no-cache, no-store
Content-Encoding: gzip
Vary: Accept-Encoding
X-Response-Time: 6.00000
Content-Length: 6881
Date: Sun, 10 Feb 2017 19:45:11 GMT
Connection: keep-alive
Content-Type: application/json

...Insert Response Body...

---

# Example Response Statuses
- 200 "OK"
- 302 "Found" (Temporary Redirect)
- 303 "See Other"
- 403 "Forbidden"
- 404 "Not Found"
- 500 "Internal Server Error"

---

# Response Headers
- Optional based on request method and status code
- Examples include:
- Response meta-information (age)
- Server meta-information
- Content meta-information
  - encoding, language, length, encryption, type, expiration

---

# Response Body
- Also known as the “Entity”
- What the client is really requesting
- HTML text
- Images
- Multimedia
- Encrypted or encoded data of any kind (could be RMI or IIOP data)

---

# HTML
- Hypertext Markup Language
- A defined DTD for linked documents
- Uses XML markup to define
- Document structure (headings, paragraphs, etc.)
- Formatting (bold, centering, fonts)
- Links (within the document to other resources)

---

# HTML Content
- Originally came from files on the HTTP server
- Placed in a special location visible to server
- Server responds to a request by locating the corresponding file and returning its contents in the response body

---

# CGI
- Common Gateway Interface
- Certain requests cause web server to launch external program
- This program is responsible for
  - Processing incoming HTTP request
  - Producing the correct HTTP response
- Result: Dynamic content

---

# CGI Programs
- Scripting languages commonly used (Perl)
- Written to interact with database
- Read and update
- Could interface with legacy systems to provide a web front end

---

# CGI Issues
- Scalability issues
  - Each request spawns process
- Scripting language limitations
- Resource contention

---

# Web Applications
- CGI was OK for small things
- Big things: Add abstraction layers
- Provide a model for the HTTP world
- Object-oriented
- Provide higher level of server integration for developers
- Simplify and clarify entry points
- Push work to the browser

---

# Web Application Abstractions
- Request
- Response
- Session
- Cookie
- Context scoping

---

# Requests and Responses
- Allow programmers to
  - Access header values
  - Write response values
  - Specify response codes
  - Redirect requests
  - Store values for later processing

---

# Sessions
- Represents a conversation between a client and the web server
- State that is maintained over several request/response sessions
- Usually time limited
- Prevents resource exhaustion on the server
- Typically maintained within one browser instance interaction
- Classic example: shopping cart

---

# Scoping
- Application data can be stored at various levels:
  - Page
  - Request
  - Session
  - Application

---

# Cookies
- Files stored on behalf of the server by the browser on the requesting machine
- Help preserve state between site visits
- Specified using HTTP headers
- Web server program sends cookie information as part of a response
- Browser cooperates by sending cookie information back when user revisits same site

---

# Client-side Behavior
- Beyond HTML formatting browsers can perform advanced application hosting features
- Programs are sent by the server, run by browser
  - JavaScript
  - Java Applets
  - Flash and Silverlight movies
- Provides for rich user experience

---

# JavaScript
- The most common, portable mechanism for client-side behavior is JavaScript
- C-based programming language
- Dynamic Language
- Amongst the most widely used programming languages world-wide

---

# JavaScript Supports
- Respond to UI events
- Inspect HTML DOM (Document Object Model)
- Modify HTML DOM
- Animate the page
- Validate input forms fields
- Make additional HTTP requests (AJAX)

---

# The Future: Http 2
- Binary protocol
- Multiplexed over TCP
- Asynchronous pipelining
- Header compression to reduce overhead
- Supports push responses to clients
- Requires TLS (Transfer Layer Security)

---

# Multiplexing
- Allow multiple request and response messages to be in flight at the same time
- Messages can be intermingled with each other on the wire
- Client shares one connection channel for all messages to the origin

---

# Single connection
- Typical for web pages/applications to open multiple connections to origin (CSS, JavaScript, API calls)
- Multiple connections can result in network congestion and retransmits
- Limiting messages to a single connection reduces load on network resources

---

# Server Push
- Servers can send CSS and JavaScript to browser caches as the HTML loads
- Prevents the round trip of requests for content that is known to be needed
- Proper use of this approach is still experimental

---

# Header Compression
- Typical pages have about 1400 bytes of headers
  - Cookies, Referer, Content-type, etc.
- Header compression reduces the need for round-trips to server for simply retrieving headers
  - Typically required to properly handle the response
- Header-based latencies, especially on mobile networks, can often exceed several hundred milliseconds

---

# HTTP 2 Availability
- Current versions of
  - Edge
  - Safari
  - Chrome
  - Firefox

---

# Summary
- HTTP
  - Stateless response/request protocol
- Programming models for the web involve abstraction layers above HTTP
- JavaScript provides support for dynamic behavior
- HTTP 2 is ~~the future~~ here
- Still needs lots of adoption
  - Lots of server support now (requires extra configuration)
  - Limited client support (Netty, OkHttp, .. ?)
