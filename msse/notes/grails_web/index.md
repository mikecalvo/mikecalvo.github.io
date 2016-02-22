---
title: Web Application Development
layout: default
---

# Controllers & Views

## Mike Calvo

### mike@citronellasoftware.com

---

# Controllers

- Grails components that handle HTTP requests
- Requests are routed to methods in Controllers called Actions
- Default routing: controller/action/[id]
- Default action: index

---

# Controller Responses

- Render HTML to the response directly
- Populate a model to be used by a View
- Render JSON to the response directly
- Forward the request to another route for further processing

---

# Controller Conventions

- grails-app/controllers
- Classname ends with "Controller"
- Public methods are available actions/routes

---

# Controller Scope Variables

- Grails injects the following values into every controller
- request: current request
- params: request parameters
- session: cross-request state
- flash: available for this request and the next
- servletContext: web application scope

---

# View

- Groovy Server Pages (GSP)
- Template files combining markup (HTML) and code
- Custom tags are reusable components with GSPs
