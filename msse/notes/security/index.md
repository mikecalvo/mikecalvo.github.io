footer: © Citronella Software Ltd 2015
slidenumbers: true

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
# Grails Safe Data Access
- Safer:
  - Dynamic finders
  - Where queries
  - Criteria API
  - Static HQL with parameters
- Unsafer:
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
- Code in HTML must exist within a <script> element
- Replace every <, > character with the HTML escaped equivalent
  - &lt;
  - &gt;
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
- Until you do this, browsers will warn visitors when sites are accessed

---
# Access Control
- Two key parts
- Is the user the person they say they are?
  - Authentication
- Does the user have rights to perform the action they are requesting
  - Authorization

---
# There's a Plugin For That!
- No need to roll your own access control functionality
- Several Grails plugin implementations exist:
  - Authentication plugin
  - Shiro plugin
  - Spring Security plugin

---
# Spring Security Plugin
- Formerly Acegi Security
- Includes suport for Twitter and Facebook authentication
- Provides integration with required access control data and Grails models
- Outstanding
- BuildConfig.groovy: `compile ":spring-security-core:2.0-RC4"`

---
# Spring Security Concepts
- User - a principal user of your system
- Role - an action that requires access
- RequestMap - a mapping of urls to role (optional)
- Users can have many Roles

---
# How S2 Works
- Pages are assumed to be secure by default
- Servlet filter intercepts inbound HTTP request
- If request is not protected it lets it through
- Checks the Session for an authenticated user
- If there is no authenticated user or the user is missing required role route to login page
- Otherwise let the request through

---
# What is a Role?
- String value
- Describes a type of use of the system
  - Examples: Admin, User, Moderator
- May be more fine-grained or permission based
  - Examples: View Posts, Create Posts, Moderate Posts

---
# Setting up Spring Security
`grails s2-quickstart <domain-package> <user> <role>`
- Generates domain classes for access control
- Generates a login page
- Default configurations in Config.groovy
- It is safe to make changes to the generated code

---
# Protecting Access
- Static config: urls are mapped to Roles in Config.groovy
- Annotations - controllers and services can be annotated with Spring Security
  `@Secured(value=["hasRole('ADMIN')"])`
- Dynamic - request map is stored in database

---
# Example
- Let's look at the muzic project with Spring Security Plugin applied
- Add some sample users and roles in the bootstrap
- Update the default url security configuration to protect the existing controllers
- Update the functional tests to first login

---
# Spring Security Service
- springSecurityService provides access to the following:
- currentUser
- isLoggedIn
- encodePassword
- isAjax

---
# Customizing Login
- grails.plugin.springsecurity.auth.loginFormUrl
- grails.plugin.springsecurity.failureHandler.defaultFailureUrl
- grails.plugin.springsecurity.successHadler.defaultTargetUrl

---
# Protecting By User Id
- Some data can only be seen by the logged in user
  - Example: View/Edit user profile
- @Secured annotation allows a closure to define the security

```
@Secured(closure = {
  authentication.username = params.id
  })
def editProfile() {

}
```

---
# Example: Muzic Edit Profile Page
- Add a simple profile edit page
- Restrict access to the current logged-in user
- Add some functional tests

---
# Security Recap
- Security is important and complex
- Leverage frameworks (like Grails and Spring Security)
- Code reviews help
