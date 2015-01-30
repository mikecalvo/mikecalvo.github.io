footer: © Citronella Software Ltd 2015
slidenumbers: true

# Web Basics
## Mike Calvo
### mike@citronellasoftware.com

---
# Objective
- Provide high-level overview of technologies involved in delivering UI over the web
  - HTTP
  - HTML
  - CSS
- Web Application Constructs
- JavaScript

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
# HTTP Request Format
- Request method
- URI (Uniform Resource Identifier)
- Protocol version (HTTP/1.1)
- Optional Headers
- Optional Body

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
# HTML Basics
- Basic page structure
  - HTML, HEAD, BODY
- Document structure
  - P, H1, H2, H3

---
# HTML Formatting & Visuals
- Layout
  - TABLE, DIV, BR
- Formatting
  - B, I, U, SPAN, CENTER
- Visual Elements
  - IMG, HR

---
# HTML Forms
- HTML provides a mechanism for submitting (POST method) data to a server
- The FORM element represents a block of information that is entered by the user
- INPUT elements within the form produce input controls in the page such as Checkboxes, Text Fields, Combo boxes, Buttons

---
# Example Form
```html
<form action="process" method="post">
  First Name: <input name="firstName" /><br/>
  Last Name: <input name="lastName" /><br/>
  Gender: <select name="gender">
    <option>Select</option>
    <option>Female</option>
    <option>Male</option>
  </select>
  <input type="submit" />
</form>
```

---
# Form Input Values
- The values collected in the input fields are submitted as part of the request to the server using the input field name
- GET method: Appended to URI after ?
Example http://myserver/process?gender=Male&firstName=Mike
- POST method: Included as request body

---
# Styling HTML
- While HTML supports styling using style attribute it’s use is discouraged
- Recommended approach – CSS
  - Cascading Style Sheets

---
# What's Cascading?
- Order of preference when applying styles:
- Browser default
- External style sheet
- Internal style
- Inline

---
# Applying Styles
- Selectors are use to apply styles
- Selectors define which element(s) should be affected by the style block
- Selectors can be a variety of things including: tag names, ids, and class names

---
# CSS Selector examples:
```
/* Tag name */
h1 {
  color: red;
}  

/* Class (matches class attribute) */
.heavy {
  font-weight: bold
}

/* Id */
#first-column {
  width: 10px
}
```

---
# Style Examples
```html
<html>
<head>
  <link rel="stylesheet" href="css/screen.css" />
  <style type="text/css">
    h1 { color: blue; }
  </style>
</head>
<body>
  <h1 style="font-size: large">Hello World</h1>
</body>
</html>
```

---
# Style Precedence
- The closer a style is to the element it is styling the higher the precedence
- For example: inline style overrides the same property from an internal style

---
# Style Inheritance
- Many styles cascade from parent into child elements
- For example, a span within a paragraph that is styled to be red text will also have red text

---
# CSS Terms
- Rule => `h1 { color: blue; }`
- Selector => `h1`
- Declaration => `color: blue;`
- Property => `color`
- Value => `blue`

---
# Advanced Selectors
- HTML Element + class
`h1.headline { color: black; }`
Matches all h1 elements with a class attribute of “headline”
- May be grouped
`h1, h2, h3 {  font-family: sans-serif }`

---
# Tips on CSS Properties
- Intellisense tooling really helps here
- IntelliJ has it

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
# Adding JavaScript
- Inline: <script> element
- External file: <script src="file_path">
- Typically placed in HEAD of document but can exist within body as well
- Code executed in order as found within the document

---
# Element Events
- Most HTML elements support typical UI event attributes
  - onclick, onmouseout, onmouseover, onkeydown, onkeyup
- Form elements support
  - onblur, onfocus, onchange, onselect, onsubmit
- Documents support
  - onload, onunload

---
# JavaScript Functions
- Declared in SCRIPT element
- Have a name and parameters
- Parameters can be untyped
- Can have a return value

---
# Validation Example
```javascript
def validate = function() {
  var firstName = document.getElementById('first').value
  if (!firstName) {
    alert('First Name Required')
  }

  return !!firstName
}
```
```html
<input type="submit" onclick="validate();" />
```

---
# Summary
- HTTP
  - Stateless response/request protocol
- HTML Basic Structure of document for content and Layout
- CSS is recommended way to style content
- Programming models for the web involve abstraction layers above HTTP
- JavaScript provides support for dynamic behavior
