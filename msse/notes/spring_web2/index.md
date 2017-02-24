---
title: Web Application Development
layout: default
---
autoscale: true
theme: Next, 3

# Spring Web

---

# Installing Spring Boot Web
Add dependency to `build.gradle`

``` groovy
dependencies {
  ...
  compile('org.springframework.boot:spring-boot-starter-web')
  ...
}
```

---

# Model View Controller
- UI Architectural Pattern
- Model: data about the application
- View: visual presentation (HTML/Javascript)
- Controller: mediates between view and model

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
- Lots of repetition
- Hand editing XML

---

# Spring Dispatcher Servlet
- Removes xml requirement - configuration moved to annotations
- Single servlet that routes requests to Controllers
- Manages taking responses from controllers and sending them to views

---

# Spring Controllers
- Handle incoming HTTP requests
- Process parameters
- Fetch data
- Persist data

---

# Spring MVC - Server side rendering

- Used in conjuntion with server-side templates
  - Thymeleaf, Velocity, Freemarker, Html
- Controllers can return a view that maps by default to:
  - resources/static
  - resources/templates
- Models are Maps
- Views are strings that resolve to templates

---

# Route Method Return Types - Controllers
- ModelAndView, Model, Map, View, String
  - These are specific to SpringMVC view approaches
  - Example: String is name of the view to show
- `@ResponseBody`: Method Annotation: Return value instead of model/view

---

# Example model/view

``` groovy

@Controller
class PostViewController {

  @RequestMapping("/postView")
  public String postView(Model model, Pageable request) {
    Page page = postService.listPosts(new PageRequest(request.pageNumber, request.pageSize, new Sort(Sort.Direction.DESC, "createdDate")))

    //Will be available to template as Object -> page
    model.addAttribute("page", page)
    //String ties to /resources/templates/postView.html
    return "postView"
  }
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
  - Base path for all requests may be set at class level with `@RequestMapping`
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

# Advanced Route Mapping
- Headers
  - `@GetMapping(path = "/pets", headers = "myHeader=myValue")`
- Parameters
  - `@GetMapping(path = "/pets/{petId}", params = "myParam=myValue")`
- Response Media Type
  - `@GetMapping(path = "/pets/{petId}", produces = MediaType.APPLICATION_JSON_UTF8_VALUE)`

---

# Route Method Arguments
- Certain values can be injected as method arguments by Spring
- HttpServletResponse
  - Modify headers
  - Customize response code
- Pageable
  - Repositories must extend PagingAndSortingRepository (JpaRepository does this)
  - Can pass PageRequest to repositories for easy sorting and paging
  `/posts?page=0&size=2&sort=createdDate,desc`

---

# Route Method Arguments Cont.
- WebRequest, HttpServletRequest, HttpMethod
- Locale, TimeZone
- Principal
- Create your own - `HandlerMethodArgumentResolver`

---

# Mapping Path and Query Variables
- Variables are mapped to parameters using `@PathVariable` annotation
  - Names don't necessarily have to match
  - `@PathVariable('otherName')`
- Request Parameters -> `@RequestParam('myParam')`

```groovy
//http://localhost:8080/pathParam?myQueryParam=foo
@GetMapping(/{pathParam})
public Object getObject(@PathParam String pathParm, @RequestParam('myQueryParam') String myQueryParam) {

}
```
---

# Serializing to/from Models

- Add Data - `@RequestBody`
- Return Data - `@ResponseBody`
- Spring MVC registers a list of HttpMessageConverters
- On request they inspect the mime type to determine if they can map to an Object
- Can add your own or modify existing ones

---
# RequestBody Example

```groovy
@PostMapping
User addUser(@RequestBody User user) {
  return userService.addUser(user)
}
```
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
- Controllers specifically for RESTful data exchange over http
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
  public User getUser(@PathVariable Long id) {
      return userRepository.findOne(id)
  }
}
```

---

## Spring Data Rest

## The Final Level Of Rest?

---

# Richardson Maturity Model
- Popular talk delivered at QCon
- Described how an organization should mature their rest model

---

![inline](images/rmm.png)

---

# RMM Explained
- Level 0 - POX (Plain old xml)
  - Using HTTP without any meaningful use of the web (RPC)
- Level 1 - Resources/Multiple endpoints
- Level 2 - HTTP Verbs / GET/POST etc.
- Level 3 - Resouces describe actions you can take on a resource instead of just the resource

[https://martinfowler.com/articles/richardsonMaturityModel.html](https://martinfowler.com/articles/richardsonMaturityModel.html)

---

## HATEOS (Hypertext As The Engine Of Application State)

```
{
  createdDate: "2017-02-16T12:46:48.769",
  modifiedDate: "2017-02-16T12:46:48.769",
  email: "franklin@franksinternet.com",
  handle: "franklin",
    _links: {
        self: {
          href: "http://localhost:8080/users/1"
        },
        user: {
          href: "http://localhost:8080/users/1"
        },
        posts: {
          href: "http://localhost:8080/users/1/posts"
        }
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
Add dependency to build.gradle (This can have side effects)

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
