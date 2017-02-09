---
title: Web Application Development
layout: default
---

# Spring Web

## Mike Calvo

### mike@citronellasoftware.com

---

# Spring Web Support
- Spring MVC
- Spring Data REST
- Spring Web Services
- Spring Web Flows

---

# Installing Spring Boot Web
Add dependency to build.gradle

`spring-boot-starter-web`

---
# Model View Controller
- UI Architectural Pattern
- Model: data about the application
- View: visual presentation (HTML/Javascript)
- Controller: handles interactions

---
# Spring Controllers
- Handle incoming HTTP requests
- Process parameters
- Fetch data
- Persist data

---
# Java Servlets
- Original Java web server component
- Class implements HttpServlet interface
- Configured into web.xml file
- One servlet handles each URL path

---
# Servlet Problems
- Too much configuration
- Not easily tested

---
# Spring DispatcherServlet
- Single Servlet that routes requests to Controllers
- Manages taking responses from Controllers and sending them to views

---
# Creating a Controller
- Defined using `@Controller` above class
- Each method can respond to a different request
  - `@RequestMapping` above each method
- Url path elements and parameters can be mapped to arguments
  - `@PathVariable`

---
# Example Controller

``` groovy
@Controller
class HelloWorldController {

    @RequestMapping("/hello/{name}")
    String helloWorld(@PathVariable String name) {
        return "hello $name";
    }
}
```

---
# Request Mappings
- Specify path (including variables)
  - Base path for all requests may be set at class level with `@Controller`
- Optionally specify method
  - RequestMethod.GET|POST|PUT|DELETE

---
# Composed RequestMappings
-  Allow for simpler specifications
- `@GetMapping(path)`
- `@PostMapping(path)`
- `@PutMapping(path)`
- `@DeleteMapping(path)`

---
# URI Template Patterns
- Define patterns for URI mappings
- Define variables to be used as parameters
  - `@GetMapping('/owners/{ownerId}')`
  - `@GetMapping('/search?q={queryString}')`

---
# Regular Expression Patterns

``` groovy
@RequestMapping("/spring-web/{symbolicName:[a-z-]+}-{version:\\d\\.\\d\\.\\d}{extension:\\.[a-z]+}")
void handle(@PathVariable String version, @PathVariable String extension) {
    // ...
}
```

---
# Mapping Path Variables
- Variables are mapped to parameters using `@PathVariable` annotation
  - Names don't necessarily have to matches
  - `@PathVariable('otherName')`

---
# Spring Web Services
- Support for SOAP/XML-based web services
- Contract-first approach
  - Define a WSDL first
- Data Contracts
  - DTD and Schemas

---
# Web Service Endpoints
- `@Endpoint`
