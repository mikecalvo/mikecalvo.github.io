---
title: Web Application Development
layout: default
---

# Web and Grails Security

## Mike Calvo

### mike@citronellasoftware.com

---

# Disclaimers
- I am not an expert
- There's an entire course on this topic
- It's super important
- No such thing as completely secure/safe

---

# Price of Success

## If your web app is successful - it will be attacked

---

# Forms of Attack
- Gaining unauthorized access to sensitive data
- Denial of service: overload the system to bring it down
- Code injection
  - Submit data to server in a way that it will unknowingly be run
  - SQL injection, XSS, CSRF, invalid redirects and forwards

---

# Grails Provides Help
- Access to database is scraped to prevent SQL injection attacks
- Default scaffolding HTML escapes all data fields before display
- Tags producing links escapes markup to prevent code injection
- Standard codecs for rendering of escaped data directly from controllers
- Command objects allow constrained form submission of data

---

# Grails Safer Data Access
- Dynamic finders
- Where queries
- Criteria API
- Static HQL with parameters
- Less Safe:
  - Constructing HQL/SQL via concatenation

---

# Command Objects
- Reusable components used for passing parameters to controllers
- Support validation
- Testable
- Decouples the input from the data
  - Prevents unintended binding of request parameters to database fields

---

# Validate User Input
- Don't trust the user/request
- Always place limits on queries
- Scrub inputs from things that might be code

---

# Client Versus Server Validation
- Developers balk at doing same work in two places
- Server must validate
  - Nothing prevents a hacker from sending a raw request to app
- Client validation optional
  - Provides a better/crisper user experience

---

# Escaping Output
- Any data specified by users must be considered inherently unsafe to display directly
- Example:
  Hotel review includes some javascript code which launches a popup to a porn website displaying when any user of the travel review site visits that hotel
- Solution: HTML escape values before displaying them

---

# HTML Escaping
- Code in HTML must exist within a `<script>` element
- Replace every `<`, `>` character with the HTML escaped equivalent
  - `&lt;`
  - `&gt;`
- Code will now safely show up as a strange comment
- Grails escapes all dynamic content in GSP pages

---

# Cross Site Request Forgery (CSRF)
- A malicious site returns a page which includes code and markup you don't see
- The code and markup submit a form request to a real site
- Because the request is coming from your browser, it includes a cookie for the real site
- That request can look valid to the real site

---

# Preventing CSRF
- Include a unique token with the form for your application
- When a form request is processed, the token is compared against the expected token
- Grails CSRF support
  - `useToken` attribute on the `<g:form>` tag adds the token
  - withForm controller block includes an invalidToken method for placing CSRF token mismatch logic

---

# Protecting Data In Transit
- Data travelling via normal HTTP traffic is visible to anyone
- SSL (Secure Sockets Layer) is the standard approach to encrypt HTTP traffic via the HTTPS protocol
- Apps with sensitive data should be deployed using HTTPS
- This requires generating an SSL cert which needs to be installed in your web server
- Until you do this, browsers will warn visitors

---

# Access Control
- Two key parts
- Is the user the person they say they are?
  - Authentication
- Does the user have rights to perform the action they are requesting
  - Authorization

---

# Grails Access Control
- Interceptors
- Security Plugins

---

# Grails Interceptors
- Intercept all HTTP requests into Ser er
- Logic can be run before or after the request is handled
- Live in `grails-app/controllers`
- Name ends with `Interceptor`
- `grails create-interceptor`

---

# Example Interceptor

``` groovy
class SecurityInterceptor {
    SecurityInterceptor() {
        matchAll()
        .except(controller:'user', action:'login')
    }

    boolean before() {
        if (!session.user && actionName != "login") {
            redirect(controller: "user", action: "login")
            return false
        }
        return true
    }

}
```

---

# Security Plugins
- Spring Security
  - De-facto standard for many years
  - Not smoothly integrated with 3.x
  - Security REST Plugin
- Shiro
  - Adds declarative access control to Grails Controllers

---

# Spring Security Rest Plugin
- [https://grails.org/plugin/spring-security-rest](https://grails.org/plugin/spring-security-rest)

``` groovy
compile 'org.grails.plugins:spring-security-rest:2.0.0.M2'
```
---

# Security Recap
- Security is important and complex
- Leverage frameworks (like Grails and Spring Security)
- Code reviews help
