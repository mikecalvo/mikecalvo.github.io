footer: © Citronella Software Ltd 2015
slidenumbers: true

# Controllers & Views
## Mike Calvo
### mike@citronellasoftware.com

---
# Introduction to MVC Architecture
- Model: data specific to your system
- View: how your system is presented to the outside (UI or API)
- Controller: handles user interaction and external requests

---
# Servlets: The Original Controller
- Java specification for HTTP request processing (1997)
- Defined a web server component model for Java
- Servlet: component responsible for receiving an HTTP request and generating an HTTP response
- Lifecycle managed by the Java web server
- Tomcat was one of the first complete implementations

---
# Java Web History Since
- Abstraction layers on top of Servlets
- JavaServerPages (JSP) - servlets in the form of HTML templates
- Struts, JSF, SpringMVC
- People couldn't find the silver bullet

---
# Ruby on Rails
- In December 2005 version 1.0 released
- Based on a dynamic programming language: Ruby
- Favored convention over configuration
- Built around the DRY principle
- Huge productivity gains were observed

---
# Why Was Rails Faster?
- Project structure standard, generated, and consistent
- Dynamic language better at dealing with string-based HTML
- Less verbose configuration and code
  - Pick reasonable defaults
- Solved a specific problem that was very common

---
# Some in Java Took Note
- Grails 1.0 released in February 2008
- Based on JVM language Groovy
- Based on Spring and Hibernate
- Followed patterns popularized by Ruby on Rails

---
# Controllers
- Control the flow of a web application
- Handle inbound HTTP requests
- Decides what happens next
  - Execute another action
  - Renders a view page
  - Directly rener a response

---
# Controller Autowiring
- Like domain classes, controllers are provided many things automatically
- request: The inbound HTTP request being handled
- response: The outbound HTTP response
- session: An HTTP session for the current user
- services: Transactional logic for your application

---
# Controllers Done Easy: Scaffolding
- Automatically create CRUD UI for a domain class
- Not designed to be used as a production UI
- Good way to get started
- Dynamic and static options

---
# Dynamic Scaffolding
- Pages and action methods created at runtime
- No actual source exists
- Simply add the following line to a controller:
`static scaffold = true`

---
# Dynamic Scaffolding Provides
- List Page: paged, sortable table showing all instances
- Detail Page: create, edit, detail and update page
- Dynamically updates UI based on domain changes

---
# Static Scaffolding
- Generate the views and controller code for CRUD functionality
- Good way to learn how Grails works and a starting point for your own UI
- Produce static Scaffolding:
`./grailsw generate-all <domain-class>`

---
# Static Scaffolding: What's Generated?
- A controller with all crud action methods
  - show, create, save, edit, update and delete
- A collection of view files for create, edit, list and show

---
# Scaffolding: Not For Production!
- Scaffolding is a great way to get something up and running quickly
- Scaffolding is a great way to learn about Grails
- Scaffolding can help you get motivated by quick progress
- Scaffolding is an OK option for admin screens
- Once you become comfortable with Grails you'll develop better practices than those shown in scaffolding

---
# Real Programmers: Hand Code Controllers

---
# Controller Conventions
- All controllers live in grails-app/controllers
- Controllers are named consistently: <ControllerName>Controller
  `eg: ArtistController`
- Default convention for URL-to-controller mapping is
  `/controller/action/id`
  - See UrlMappings.groovy for this configuration

---
# Crazy Simple Controller
```
class ArtistController {

  // Maps to /artist/get/id
  def get() {
    def artist = Artist.get(params.id)
    if (!artist) {
      response.sendError(404)
    } else {
      [ artist: artist ]
    }
  }
}
```

---
# Controller Unit Tests
- Let's look at a unit test for this simple controller
- Use @TestFor to tell Grails this is testing a controller
- Use @Mock to mock out domain interactions

---
# Adding a View
- The controller is simply performing a lookup and returning a model object
- The view is how the end customer will see the response
- If no view is rendered by action, Grails will look for a view by convention
  `grails-app/views/controller-name/action-name.gsp`

---
# GSP - Groovy Server Page
- View templates which combine code with Markup
- Rendered into final markup *on the server*
- Model objects returned by controller are available to code in the GSP

---
# Crazy Simple GSP
```html
<html>
<body>
Details for <span id="name">${artist.name}</span>
<div id="id">Id: ${artist.id}</div>
</body>
</html>
```

---
# Default Controller Conventions
- Responds to URL ending with controller name
- Actions:
  - If the controller defines one action it is the default
  - If the controller defines `index` action it is the default
  - If a property called `defaultAction` exists it is the default

---
# Controller Logging
- All controllers get a `log` member injected into them
- This allows for logging at various levels:
  - debug, error, fatal, info, trace

  `log.info "User ${userid} requested report ${reportId}"`

---
# Request Processing
- All controllers have several request related members injected into them
- Action info: actionName, actionUri
- Controller info: controllerName, controllerUri
- Request: request
- Request Parameters: params

---
# Request Parameters
- In addition to params they are available on request as properties
  - controller/action?userId=1 => request.userId == 1
- The part of the url after the action is the id parameter
  - controller/action/1 => request.id == 1

---
# Redirecting a Request
- A request can be redirected from an action elsewhere
- Another action: redirect(action: 'process')
- Another controller: redirect(controller: 'tax', action: 'calculate')
- Relative URI: redirect(uri: '../checkout/complete')
- Absolute URI: redirect(url: 'http://google.com')

---
# Redirect New Request
- When redirecting to another controller in app keep in mind that request will be new
- Parameters need to be included in the redirect if they are important:
  `redirect(action: 'success', params: params)`
- Alternatively, use flash scope to make things available in the redirected request

---
# Why Redirect?
- Good pattern for providing reliable reload in browser
- Form submits via HTTP PUT or POST behave poorly when browser refreshed
  - Are you sure you want to resubmit this POST?
- Best practice is to redirect back to a display action after processing the form to avoid this


---
# Controller Scopes
- Values can be stored in several contexts with varying lifetimes
- request - current request
- flash - current and next request
- session - until current user closes browser
- servletContext - entire Grails application
- page - available within a GSP

---
# Returning a Model
- When rendering a GSP view it is common for an action to return a model
- The model is commonly domain instances which are needed for the view
- A model is returned by returning a map from the action
  - Keys are the names of the variables in the model
  - Values are the data

---
# Example Model
```
class SongController {
  def show() {
    def song = Song.get(params.id)
    [song: song]
  }
}
```

---
# Rendering a View
- By default Grails will look for the matching GSP view for your action
  `grails-app/views/controller/action.gsp`
- Alternatively you can render a different view
  `render(view: 'alternative', model: [song: song])`
  `render(controller:'artist', view: 'details')`

---
# HTTP Method Restrictions
- Actions can check the requested method
  `if (request.method == 'GET') ...`
- Controller property `allowedMethods` can declaratively restrict access to actions by method
  - Grails will return a 405 error code if accessed with improper method

---
# Example HTTP Method Restriction
```
class SomeController {
  def allowedMethods = [modify: 'POST', delete['POST', 'DELETE']]

  def modify() {}
  def retrieve {}
  def delete {}
}
```

---
# File Uploads
- Uploading a file requires a special HTML 'file' input:
  `<input type='file' name='upload'`
- Controllers can access submitted files via the request:
  `request.getFile('upload')`
- Multipart file data can be mapped into domain class
  - byte[] or String

---
# File Upload Example
```
class FileController {
  def upload() {
    def file = request.getFile('upload')
    if (file && !file.empty) {
      file.transferTo(new File(pathToFile))
    }
  }
}
```

---
# Writing Binary Responses
- Images or other binary data may need to be returned from app
- Response supports writing directly to the output stream
- Content type should be supplied

---
# Example Binary Response
```
class ProfileController {

  def image() {
    def profile = Profile.get(id)
    byte[] img = profile.image

    response.contentType = 'application/octect-stream'
    response.outputStream << img
    response.outputStream.flush()
  }
}
```

---
# Controller Interceptors
```
def beforeInterceptor = [action: this.&auth, except: 'login']
// defined with private scope, so it's not considered an action
private auth() {
  if (!session.user) {
    redirect(action: 'login')
    return false
  }
}

def login() {
  // display login page
}
```

---
# GSP Code
- Grails injects all pertinent values:
  - request, response, session, params, model objects
- Code can exist
  - In page (scriptlet): <% def date = new Date() %>
  - In page (expression): ${model.property}

---
# GSP Tags
- Reusable code for GSP
- Code runs and typically injects markup in place of tag
- Syntax: <g:tagName attributes />
- Large set included with Grails
  - <g:if /> <g:each /> <g: datePicker />

---
# Essential Tags
```
<g:if test="${session.user != null}">
  Welcome ${session.user.firstName}
</g:if>
<g:elseif test="${session.user.name == 'Mike'}">
  Go away
<g:/elseif>

<g:each var="n" in="${names}">
<li>${n}</li>
</g>
```

---
# Link Tags
```
<g:link controller="artist" action="create">
  New Artist
</g:link>

<img src="<g:createLink controller='image' action='render'/>" />

<link rel="stylesheet"
  href="${resource(dir:'css',file:'foo.css')}" />

<g:external dir="css" file="foo.css" />
```

---
# Form Tags
```
<g:form controller="user" action="register"> ... </g:form>

<g:textField name="firstName" value="${user?.firstName}" />
<g:textArea name="bio" value="${user?.bio}" rows="10" cols="60" />

<g:select name="color" from="${Color.list()}"
    optionKey="id"
    optionValue="displayName"
    noSelection="${'null': 'Please Chose a Color'}" />

<g:hiddenField name="rendered" value="${new Date()}" />
```

---
# Many More Tags
- Advanced controls
  - g:datePicker, g:currencySelect, g:paginate, g:countrySelect
- Displaying resources
  - g:message
- Displaying errors
  - g:hasErrors, g:renderErrors

---

# Custom Tags
- Write your own tags
- Conventions:
  - Live in grails-app/taglib
  - NameTagLib is class name

---
# Example TagLib
```
class DateTagLib {
  def dateFromNow { attrs->
    def niceDate = formatNiceDate(attrs.date)
    out << niceDate
  }
}

In page:
<g:dateFromNow date="${post.created}" />
```

---
# TagLib Namespace
- g: is the default namespace for Grails plugins
- Plugins often use a different prefix for tags part of plugin
- Define a static namespace value in your TagLib to override default
  `static namespace = 'mz'`

---
# Templates
- Reusable chunks of HTML markup
- Defined common layouts
- Can refer to all scoped variables GSPs can
- GSP pages starting with _ are templates
- Including a template: <g:render template="X" />

---
# Layouts
- Grails uses Sitemesh to provide view layouts
- They live in grails-app/views/layouts
- A Layout is a normal GSP file
  - Includes special tags for inserting content into the layout
  - g:layoutHead and g:layoutBody

---
# Example Layout
```html
<html>
  <head>
    <g:layoutHead />
  </head>
  <body>
    <!-- some common header or menu -->
    <g:layoutBody>
  </body>
</html>
```

---
# Triggering a Layout
```html
<html>
  <head>
    <title>My Page's Title</title>
    <meta name="layout" content="main" />
  </head>
  <body>
    My page's content here
  </body>
</html>
```

---
# Summary
- Controllers: respond to user interactions
- Views: present a UI or front-end for the system
- What's next: Functionally testing them
