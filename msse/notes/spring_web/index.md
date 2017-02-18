---
title: Web Application Development
layout: default
---
autoscale: true
theme: Next, 3

# Spring Web + Spring Data Rest

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

![inline](images/mvc.png)

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

# Spring Controllers
- Handle incoming HTTP requests
- Process parameters
- Fetch data
- Persist data

---

# Spring Controllers (MVC)

- Used in conjuntion with server-side templates
  - Thymeleaf, Velocity, Freemarker
- Controllers can return a view that maps by default to:
  - resources/static
  - resources/templates
- Models are Maps
- Views are strings that resolve to templates

---

# Example model/view

``` groovy
@RequestMapping("/postView")
public String postView(Model model, Pageable request) {
  Page page = postService.listPosts(new PageRequest(request.pageNumber, request.pageSize, new Sort(Sort.Direction.DESC, "createdDate")))
  page.content.each {Post post ->
    post.user
  }
  //Will be available to template as Object -> page
  model.addAttribute("page", page)
  //String ties to /resources/templates/postView.html
  return "postView"
}
```

---
# Creating a Controller
- Defined using `@Controller` above class
- Each method can respond to a different request
  - `@RequestMapping` above each method
- Url path elements and parameters can be mapped to arguments
  - `@PathVariable`
- @ResponseBody can be defined to get back an object instead of a view

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

---
# Regular Expression Patterns

``` groovy
@RequestMapping("/spring-web/{symbolicName:[a-z-]+}-{version:\\d\\.\\d\\.\\d}{extension:\\.[a-z]+}")
void handle(@PathVariable String version, @PathVariable String extension) {
    // ...
}
```

---
# Mapping Path and Query Variables
- Variables are mapped to parameters using `@PathVariable` annotation
  - Names don't necessarily have to matches
  - `@PathVariable('otherName')`
- Request Parameters -> @RequestParam

```groovy
//http://localhost:8080/pathParam?myQueryParam=foo
@GetMapping(/{pathParam})
public Object getObject(@PathParam String pathParm, @RequestParam String myQueryParam) {

}
```

---
# Advanced Route Mapping
- Headers
  - `@GetMapping(path = "/pets", headers = "myHeader=myValue")`
- Parameters
  - `@GetMapping(path = "/pets/{petId}", params = "myParam=myValue")`
- Response Media Type
  - `@GetMapping(path = "/pets/{petId}", produces = MediaType.APPLICATION_JSON_UTF8_VALUE)`

---
# Route Method Arguments
- Certain values can be injected by Spring allow fine grained control
- HttpServletResponse
  - Modify headers
  - Customize response code
- Pageable
  - Repositories must extend PagingAndSortingRepository (JpaRepository does this)
  - Can pass PageRequest to repositories for easy sorting and paging
  `/posts?page=0&size=2&sort=createdDate,desc`

---
# Route Method Arguments Cont.
- WebRequest, HttpServletRequest, HttpServletResponse, HttpMethod
- Locale, TimeZone, ZoneId
- InputStream, Reader, OutputStream, Writer
- Principal
- `@RequestParam`, `@RequestHeader`, `@RequestBody`

---
# Route Method Return Types
- ModelAndView, Model, Map, View, String
  - These are specific to SpringMVC view approaches
  - Example: String is name of the view to show
- `@ResponseBody`: Returned string is the response
- `@ModelAttribute`: Returned value is available in model

---

# ResponseEntity
- Can wrap all responses in `ResponseEntity<Entity>`
- Can return error messages in Status
- `HttpEntityMethodProcessor` delegates to appropriate HttpMessageConverter

```groovy
@GetMapping('/percentage/{percent}')
ResponseEntity<Percentage> getPosts(@PathVariable int percent) {
  if (percent > 100) {
    Status error = new Status( "failed", "invalid parameter" )
    return new ResponseEntity<Status>(error, HttpStatus.BAD_REQUEST)
  }
  ...
}
```

---

# RestControllers
- `@RestController` Class annotation
- Provides basic Rest behavior for persisting and retrieving domain instances
- Combines `@Controller` with `@ResponseBody`
- Not to be used for templated views

---
# Example RestController

``` groovy
@RestController
public class UserController {

  @Autowired
  UserRepository userRepository

  @GetMapping("/user/{id}")
  public User getUser(@PathVariable String id) {
      return userRepository.findOne(Integer.parseInt(id))
  }
}
```

---

# Spring Data Rest

- Spring MVC Application
- Turns Repositories into Restful Controllers
- Creates endpoints using standard naming conventions
- Gives HATEOS, Pagination, Sorting Support
- Exposes custom finders in the Repository as search urls

---

## Spring Data Rest and @RestController

- Routes can conflict with your @RestControllers
- Can change the default URL of Spring Data Rest to continue using RestControllers
  - (application.properties) spring.data.rest.base-uri=/new/base/uri

---

# Installing Spring Boot Data Rest
Add dependency to build.gradle

`compile('org.springframework.boot:spring-boot-starter-data-rest')`

---

## Differences between RestController and RestRepository Approaches
- POSTing with Parent Child Relationships requires the link instead of id/path/data
  - POST /posts `{"user": "http://localhost:8080/users/21", "message": "hello"}`
- View manipulation handled by Projections and Excerpts

---

# Projections
- Interfaces that can provide a altered or combination of and number of resources
- Placed at same level or nested as Entities
- Accessed via query params `http://localhost:8080/users/1?projection=emailOnly

```groovy
@Projection(name= "emailOnly", types = { User.class})
interface EmailOnly {

  String getEmail()
}
```

---

# Excerpts

- Projections applied on a Resource level
- Only applied to Collections
- @RepositoryRestResource(excerptProjection = EmailOnly.class)
- Can be useful to include embedded Resources automatically
