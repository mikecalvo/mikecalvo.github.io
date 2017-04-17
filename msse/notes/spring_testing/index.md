---
title: Web Application Development
layout: default
---

## Functional Testing

### Marc Kapke

#### kapkema@gmail.com

---

# Functional Tests
- Black box testing
  - Server is running
  - Test issues an HTTP request
- Tests run at build time
- Used to validate code changes
- Not environment specific

---

# Why Functional Tests?
- Fully automated, repeatable
- Great for detecting unexpected regressions
- Run reasonably fast, but slower then unit tests
  - Server startup and shutdown overhead
- Run under the same harness as unit tests
- Makes functional validation first class part of project

---

# Functional Test Pattern
- Similar to a unit test
- Normal and Negative test cases
- Inputs determine expected result
 - Select inputs based on boundaries of REST api vs. function parameters
- Validate ```actual == expected```

---

# So what's a smoke test?
  - Environment specific type of functional test
    - i.e. tests http://myserver.com instead of http://localhost:8080
  - Usually run on some schedule
  - Validate the health/behavior of specific environment

---

# Spring Boot REST Functional Tests
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
