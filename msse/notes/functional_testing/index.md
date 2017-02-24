---
title: Web Application Development
layout: default
---

## Integration & Functional Testing

---

# Integration testing
- Unit tests validate a single component in isolation
- Integration tests validate how components interact when grouped into 'subsystems'.
- Building block approach
  - verified 'subsystems' are again grouped and integration tested
  - repeat until the last grouping tested represents the whole system

---

# Integration testing - Pros
- Pros
  - Edge cases can be tested easily
  - Less overhead then a functional test
  - Can leverage mocking
  - Can validate specific internal behaviors and state

---

# Integration testing - Cons  
  - Cons
    - Complex setup to initialize 'subsystem' with correct components and mocks
    - Dependent on defining mock behavior to match actual

---

# Functional Tests
- Black box testing
  - Server is running
  - No mocks, no internals
  - Validate the response
- End to end tests from boundaries of system
- Tests issue an HTTP request

---
# Functional Tests
- Pros
  - Testing real interaction
  - Can exercise and validate a lot of code in a single test
- Cons
  - Slowest tests to run
  - Hard to infer what codes being tested
  - Edge cases are hard to provoke without access to internals

---

# Why Integration & Functional Tests?
- Fully automated, repeatable
- Great for detecting regressions in unexpected parts of system
- Run reasonably fast, but slower then unit tests
  - Server startup and shutdown overhead
- Run under the same harness as unit tests

---

# Integration/Functional Test Design
- Similar to a unit test
  - Consider Positive and Negative test cases
  - Inputs determine expected result
    - Choose input values based on types (String, Integer, List, etc..)
  - Validate ```actual == expected```

---

# Spring Integration testing
- Test Spring infrastructure interaction with your system's components
- Use actual Spring configuration
- Validate specific internal state and behavior
  - Data model
  - Error Handlers
  - etc...

---

# MockMVC
- Provided by Spring MVC Test
- Isolates the web layer
  - allows you to easily test interaction between DispatcherServlet and Controllers  
- Several Annotations and builders included

---

# MockMVC - Autowired
- Use `@WebMVCTest` annotation to specify controller under test
- Spring Starts container, limits class scanning to:
  - @Controller, @ControllerAdvice, @JsonComponent
- Convenient and no setup
- Mocking with Spock is a bit more cumbersome
- Might load unused components found during class scanning

---

# MockMVC - Builders
- Only loads minimum required infrastructure for DispatcherServlet to serve requests
- No additional class scanning
- Full control over the instantiation and initialization of controllers
- Isolate and test one controller at a time.
- Can use Spock Mocks directly

---
# Example MockMVC Builder test

``` groovy
def "add valid user"() {
   setup:
   def mockUserService = Mock(UserService)
   UserController userController = new UserController(userService: mockUserService)

   def user = new User(email: "foo", handle: "bar")
   def userBody = ObjectMapper.newInstance().writeValueAsString(user)

   def mockMvc = MockMvcBuilders.standaloneSetup(userController).build()

   when:
   mockMvc.perform(post("/users").content(userBody).contentType(MediaType.APPLICATION_JSON))
          .andExpect(status().isOk()).andDo(print())
          .andExpect(content().contentType(MediaType.APPLICATION_JSON_UTF8))
          .andExpect(content().json(userBody, false))

   then:
   1 * mockUserService.addUser(user) >> user
 }
```

---

# Spring Boot Functional Tests
- SpringBootTest annotation  makes functional testing easy
- Spring Boot includes TestRestTemplate: simplifies making HTTP requests

---

# SpringBootTest
- All required test harness wiring is handled by single annotation - ```@SpringBootTest```
- Determines what server to run for test by scanning for ```@SpringBootApplication``` annotation
- Provides support for starting a fully running server on a random port.
- Registers a TestRestTemplate bean for use in functional tests.

---

# TestRestTemplate
- Autowired service available in all ```@SpringBootTest``` annotated tests
- Provides simple API for executing REST requests against running server
 - Easily automate GET, POST, PUT, DELETE requests
- Map responses to model objects for easy verification

---

# TestRestTemplate method types
- ```getForObject()``` : `*forObject` methods return the body only.
- ```postForEntity()``` :  `*forEntity` methods return response object with body
  - allows you to validate response headers, and status codes.

---

# Basic POST for entity Example
```java
@ContextConfiguration
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class UserFunctionalTests extends Specification {

  @Autowired
  private TestRestTemplate restTemplate

  def "/users - POST adds user"() {
      when:
      ResponseEntity<User> response = this.restTemplate.postForEntity("/users", new User(email: "foo@gmail.com", handle: "bar"), User.class)

      then:
      response.statusCodeValue == 200
      response.headers.getContentType() == MediaType.APPLICATION_JSON_UTF8
      User actual = response.body
      actual.email == "foo@gmail.com"
      actual.handle == "bar"
    }
}
```

---

# Bootstrapping data
- Some tests require pre-existing data
- use test setup to add data to get to your expected starting state
- Interact directly with the repository when adding setup data, don't rely on service code
- If your data source is persisted, be sure to use test cleanup to remove all data added

---

# Example Boostrapped test
```java
@ContextConfiguration
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class PostFunctionalTests extends Specification {
  @Autowired
  TestRestTemplate testRestTemplate

  @Autowired
  PostRespository postRespository

  @Autowired
  UserRepository userRepository

  def "get posts"() {
    setup:
    def user = new User(email: "foo@gmail.com", handle: "foo")
    userRepository.save(user)
    postRespository.save(new Post(user: user, message: "hello!"))

    when:
    ResponseEntity<Map> responseEntity = testRestTemplate.getForEntity("/posts", Map)

    then:
    responseEntity.statusCode == HttpStatus.OK
    Map actual = responseEntity.body

    actual.totalElements == 1

    def posts = actual.content as List
    posts.size() == 1

    def post = posts.get(0)
    post.message == "hello!"
    post.id == 1
  }
}
```

---
