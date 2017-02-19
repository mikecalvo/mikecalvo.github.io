---
title: Web Application Development
layout: default
---
autoscale: true
theme: Next, 3

# Spring Data Rest
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
