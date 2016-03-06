---
title: Security Example
layout: default
---

# Git Repository

All examples found on this page can be seen in a working Grails 3 project and cloned at: [https://github.com/mikecalvo/grails3-security-example](https://github.com/mikecalvo/grails3-security-example)

---

# Security Plugin

The Grails REST Security plugin provides token-based, stateless security for Grails 3 applications leveraging the Spring Security framework.  Details on the plugin can be found here [https://grails.org/plugin/spring-security-rest](https://grails.org/plugin/spring-security-rest)

To enable the Grails REST Security plugin, add this dependency to grails.config:

``` Groovy
compile 'org.grails.plugins:spring-security-rest:2.0.0.M2'
compile 'org.grails.plugins:spring-security-rest-gorm:2.0.0.M2'
```

An overview of how the plugin works is shown below:
![Overview](http://alvarosanchez.github.io/grails-spring-security-rest/latest/docs/images/rest.png)

---

# Security Domain Classes

Spring Security requires a 3 element domain model to support authentication and authorization

* User - contains the username and password as well as other properties that represent the Account
* Role - an authority granted to a User
* UserRole - the many-to-many join table between Users and Roles

The REST Security plugin adds a 4th element required in the domain model: an object that represents the tokens that are generated after a user is authenticated.

# User Example

This example User domain class shows examples of several concepts:

1. Injection of the springSecurityService into the domain class
2. Encoding of password fields prior to insert or update of the User
3. Spring Security requires the ability to query a User for it's granted authorities.  The getAuthorities method uses GORM to retrieve authorities for this User

``` Groovy
class User {

  transient springSecurityService

  String username
  String password
  boolean enabled = true
  boolean accountExpired = false
  boolean accountLocked = false
  boolean passwordExpired = false

  Set<Role> getAuthorities() {
    UserRole.findAllByUser(this)*.role
  }

  def beforeInsert() {
    encodePassword()
  }

  def beforeUpdate() {
    if (isDirty('password')) {
      encodePassword()
    }
  }

  protected void encodePassword() {
    password = springSecurityService?.passwordEncoder ?
        springSecurityService.encodePassword(password) :
        password
  }

  static constraints = {
  }
}
```

# Role and UserRole Examples

Plain-old Gorm classes using relationships between the entities.

``` Groovy
class Role {

  String authority

  static constraints = {
  }
}
```

``` Groovy
class UserRole {

  User user
  Role role

  static constraints = {
  }
}
```

# Authentication Token Example

The last domain class to be added is for an Authentication token.  This simply contains the username and the token granted.

``` Groovy
class AuthenticationToken {

    String tokenValue
    String username

    static mapping = {
        version false
    }
}
```
