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

---

# Create Users and Roles

Spring Security will force HTTP calls into your Grails app to be authenticated.  This means you will need some sample users and roles from which you can authenticate as.  Don't forget to also assign users to roles.  A convenient place for this is in the `grails-app/init/Boostrap.groovy` file:

``` Groovy
class BootStrap {

  def init = { servletContext ->
    def admin = new User(username: 'admin', password: 'r00t!').save(flush: true, failOnError: true)
    def role = new Role(authority: 'ROLE_READ').save(flush: true, failOnError: true)
    new UserRole(user: admin, role: role).save(flush: true, failOnError: true)
  }

  def destroy = {
  }
}
```
---

# Configure the Plugins

Both Spring Security and the REST Security Plugin have a dizzying amount of configuration.  The following file should be added (or appended to an exisiting) `grails-app/conf/application.groovy` file.

This configuration is doing several things:
1. Enabling the stateless security chain that supports the token-based model
2. Forces all calls under the path of '/api' to be authenticated
2. Enables using GORM to store generated auth tokens
3. Sets the HTTP header to be used for sending an auth token on subsequent calls
4. Defines the domain classes to be used for User, Role, UserRole and AuthenticationToken concepts
5. Defines a map listing the required access permissions for protected endpoints

``` Groovy
grails.plugin.springsecurity.filterChain.chainMap = [

    //Stateless chain
    [
        pattern: '/api/**',
        filters: 'JOINED_FILTERS,-anonymousAuthenticationFilter,-exceptionTranslationFilter,-authenticationProcessingFilter,-securityContextPersistenceFilter,-rememberMeAuthenticationFilter'
    ]

]

grails.plugin.springsecurity.rest.token.storage.useGorm = true
grails.plugin.springsecurity.rest.token.storage.gorm.tokenDomainClassName = 'grails.security.AuthenticationToken'
grails.plugin.springsecurity.rest.token.validation.useBearerToken = false
grails.plugin.springsecurity.rest.token.validation.headerName = 'X-Auth-Token'

grails.plugin.springsecurity.userLookup.userDomainClassName = 'grails.security.User'
grails.plugin.springsecurity.userLookup.authorityJoinClassName = 'grails.security.UserRole'
grails.plugin.springsecurity.authority.className = 'grails.security.Role'

grails.plugin.springsecurity.securityConfigType = 'InterceptUrlMap'
grails.plugin.springsecurity.interceptUrlMap = [
    [
        [pattern: '/api/restaurants/**', access: ['ROLE_READ']]
    ]
]```

---

# Improve Error Responses

By default Grails will use views to show error responses such as 500, 401, 403, 404.  These can be overridden.  Below is an example ErrorController that retsponds with JSON responses for these HTTP error statuses.

``` Groovy
class ErrorController {

  def internalServerError() {
    response.status = 500
    render(contentType: 'application/json') {
      error = response.status
      message = 'Internal server error'
    }
  }

  def notFound() {
    response.status = 404
    render(contentType: 'application/json') {
      error = response.status
      message = 'Not found'
    }
  }

  def unauthorized() {
    response.status = 401
    render(contentType: 'application/json') {
      error = response.status
      message = 'Unauthorized'
    }
  }

  def forbidden() {
    response.status = 403
    render(contentType: 'application/json') {
      error = response.status
      message = 'Forbidden'
    }
  }
}
```

The ErrorController is mapped to error codes in grails-app/controllers/UrlMappings.groovy:

``` Groovy
class UrlMappings {

  static mappings = {
    "/$controller/$action?/$id?(.$format)?" {
      constraints {
        // apply constraints here
      }
    }

    "/"(view: 'index')
    "500"(controller: 'Error', action: 'internalServerError')
    "404"(controller: 'Error', action: 'notFound')
    "401"(controller: 'Error', action: 'unauthorized')
    "403"(controller: 'Error', action: 'forbidden')
  }
}```

---

# Functional Test

At last, a test should be written to verify that we've correctly wired up the security plugins, configured them correctly, added the user and roles correctly, and configured the auth header names.

This test relies on the http-builder project for the REST client.  Add the dependency `testCompile 'org.codehaus.groovy.modules.http-builder:http-builder:0.7'` to build.gradle to use this.

``` Groovy
import geb.spock.GebSpec
import grails.converters.JSON
import grails.test.mixin.integration.Integration
import groovyx.net.http.HttpResponseException
import groovyx.net.http.RESTClient
import spock.lang.*

@Integration
@Stepwise
class RestaurantResourceFunctionalSpec extends GebSpec {

  RESTClient restClient

  @Shared
  def token

  def setup() {
    restClient = new RESTClient(baseUrl)
  }

  def 'calling restaurants endpoint without token is forbidden'() {
    when:
    restClient.get(path: '/api/restaurants')

    then:
    HttpResponseException problem = thrown(HttpResponseException)
    problem.statusCode == 403
    problem.message.contains('Forbidden')
  }

  def 'passing a valid username and passowrd generates a token'() {
    setup:
    def authentication = ([username: 'admin', password: 'r00t!'] as JSON) as String

    when:
    def response = restClient.post(path: '/api/login', body: authentication, requestContentType: 'application/json')

    then:
    response.status == 200
    response.data.username == 'admin'
    response.data.roles == ['ROLE_READ']
    //noinspection GroovyDoubleNegation
    !!(token = response.data.access_token)
  }

  def 'using token access to restaurants endpoint allowed'() {
    when:
    def response = restClient.get(path: '/api/restaurants', headers: ['X-Auth-Token': token])

    then:
    response.status == 200
    response.data.size() == 0
  }
}
```
