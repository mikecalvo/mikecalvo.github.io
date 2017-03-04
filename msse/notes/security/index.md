---
title: Web Application Development
layout: default
---

## REST Api Security

### Marc Kapke

#### kapkema@gmail.com

---

# Disclaimers
- I am not an expert
- There's an entire course on this topic
- People dedicate entire careers to this topic
- It's super important
- No such thing as completely secure/safe

---

# Forms of Vulnerability
- There are many forms of vulnerability
- Always evolving
- Know your attack surfaces

---

# Gaining unauthorized access to sensitive data
- Physical access to network, DB, or server
- Unsecured endpoints

---

# Denial of service
  - Overload the system to bring it down
  - Distributed Denial of Service (DDOS)

---

# Cross Site Request Forgery (CSRF)
- A malicious site returns a page which includes code and markup you don't see
- The code and markup submit a form request to a real site
- Because the request is coming from your browser, it includes a cookie for the real site
- That request can look valid to the real site

---

# Protecting Data In Transit
- Data travelling via normal HTTP traffic is visible to anyone
- SSL (Secure Sockets Layer) is the standard approach to encrypt HTTP traffic via the HTTPS protocol
- Apps with sensitive data should be deployed using HTTPS
- This requires generating an SSL cert which needs to be installed in your web server
- Until you do this, browsers will warn visitors

---

# Code injection
- Submit data to server in a way that it will unknowingly be run
- SQL injection, XSS, CSRF, invalid redirects and forwards

---

# Spring Boot Provides Help
- Access to Database through Repositories and JPA
 - Prevent SQL injection
 - Controls what DB internals are exposed
- Spring Security provides necessary interfaces to handle Authentication and Authorization
- Validators provides input validation
- Controllers standardize server access (GET/POST/etc..)

---

# Prevent SQL Injection - JPA Data access
- Built-in repository methods escape inputs correctly
     - `findAll()`, `findOne()`, etc...
- Custom JPA queries (JPQL/SQL) need to use EntityManager and Parameter binding

---

# Constructing JPQL/SQL via String concatenation
### **DON'T do this**
- Technically still valid
- Vulnerable to injection issues
- Use EntityManager instead

``` groovy
List results = entityManager.createQuery("Select user from Users user where user.id = " + userId).getResultList();
List results = entityManager.createNativeQuery("Select * from Users where email_address = " + email).getResultList();
int resultCode = entityManager.createNativeQuery("Delete from Users where id = " + userId).executeUpdate();
```

---

# Entity Manager query building
- Build queries using EntityManager
    - supports JPQL queries,
    - loading JPQL stored procedures
    - native SQL queries

---

# Entity Manager examples
- JPQL query

``` groovy
Query jpqlQuery = entityManager.createQuery("Select user from Users user where user.emailAddress='foo@gmail.com'")
```

- JPQL stored procedure

``` groovy
Query jpqlQuery = entityManager.createNamedQuery("allUsers");
```

- Native SQL

``` groovy
Query sqlQuery = entityManager.createNativeQuery("Select * from Users where email_address='foo@gmail.com", User.class)
```

---

# Parameter Binding
- via `setParameter` pattern
    - set via positional parameters
    - or via named parameters    
- JDBC Driver will escape parameters automatically prior to executing query
- Even unvalidated user input will only ever end up as data since its being escaped and not code

---

# Parameter Binding examples
- By position

``` groovy
Query jpqlQuery = entityManager.createQuery("Select user from Users user where user.emailAddress=?1")
List results = jpqlQuery.setParameter(1, "foo@gmail.com").getResultList();
```

- By name

``` groovy
Query jpqlQuery = entityManager.createQuery("Select user from Users user where user.emailAddress=:email")
List results = jpqlQuery.setParameter("email, "foo@gmail.com").getResultList();
```

---

# Spring Controllers
- Prevents unintended binding of request parameters to database fields
- Support validation
- Testable
- Decouples the input from the data

---

# Validate User Input
- Don't trust the user/request
- Always place limits on queries
- Scrub inputs from things that might be code

---

# Server side validation
- Server *must* validate inputs
- Nothing prevents requests from outside Client

---

# Server Side Validation - Validators
- Use Validators to validate domain object inputs
- Several built-in annotations for common validations
  - `@NotEmpty`, `@NotNull`, `@Email`, `@Size`, `@Pattern`, etc..
- Build custom validators for others
- Independent, easily testable

---

# Validators - examples
``` groovy
class AccountCredentials {
  @Size(min = 2, max=25)
  String username

  @Password
  String password
}
```
---

# Validator - Custom
``` groovy
class PasswordValidator implements ConstraintValidator<Password, String> {
  private Pattern pattern
  private Matcher matcher
  private static final String PASSWORD_PATTERN = '((?=.*\\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[@#$%]).{6,20})'

  PasswordValidator() {
    pattern = Pattern.compile(PASSWORD_PATTERN)
  }

  //...

  @Override
  boolean isValid(String value, ConstraintValidatorContext context) {
    if(!value){
      return false
    }

    matcher = pattern.matcher(value)
    return matcher.matches()
  }
}
```

---

# Validator - Custom annotation
- Provide annotation to use custom Validator
``` groovy
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = PasswordValidator.class)
@interface Password  {
  String message() default "Password does not meet the minimum requirements"

  Class<?>[] groups() default []

  Class<? extends Payload>[] payload() default []
}
```
---

# Client Side Validation
- Client validation optional
- Provides a better/crisper user experience

---

# Escaping Output
- Any data specified by users must be considered inherently unsafe to display directly
- Example:
  Hotel review includes some javascript code which launches a popup to a phishing website displaying when any user of the travel review site visits that hotel
- Solution: HTML escape values before displaying them

---

# HTML Escaping
- Code in HTML must exist within a `<script>` element
- Replace every `<`, `>` character with the HTML escaped equivalent
  - `&lt;`
  - `&gt;`
- Code will now safely show up as a strange comment
- Spring provides `HtmlUtils` to do this.
- `@SafeHtml` annotation on model properties will do this automatically

---

# Preventing CSRF in Spring
- Include a unique token with the form for your application
- When a form request is processed, the server validates the token is the expected token
- Spring CSRF support
  - Spring MVC with `@EnableWebSecurity` will automatically include CSRF Token in your form
  - Uses the `CsrfRequestDataValueProcessor`

---
# Access Control
- Two key parts
- Is the user the person they say they are?
  - Authentication
- Does the user have rights to perform the action they are requesting
  - Authorization

---

# Authentication
- Mechanism for validating a user is who the say they are
- Usually User/Password validation

---

# Authentication - Common patterns
  - Session
    - Server stores session
    - Cookie in client tracks session
  - Stateless
    - Encrypted Token (Social login, JWT, etc..)
    - Token validated at request time

---

# Authorization
- Assign users to roles (admin,superuser,etc...)
- Use roles to authorize access to certain parts of application

---

# Authentication - Basic Authentication

- Default behavior in Spring Security
- Enforced on every endpoint by default once includes
- Session based Authentication
  - Server managed authentication
  - Doesn't scale - client must communicate with same server going forward

---

# Authentication - Json Web Token (JWT)
- Provides Stateless Authentication
- Encrypted JSON payloads
- Requires client to get JWT first
- Include JWT with each authenticated request
- Not tied to a session
- Scales because the token is valid on any server instance

---

# JWT Token
- Comprised of 3 parts
- Header, Payload and Signature
- Format:
  - [header].[payload].[signature]
- Base64Url encoded, hashed and signed with secret

---

# JWT Token - header
- consists of two parts:  
  - the type of the token, which is JWT,
  - and the hashing algorithm being used, such as HMAC SHA256 or RSA.
  - Gets Base64Url encoded

``` json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

---

# JWT Payload
- Contains claims about entity (User/Account)
- Reserved claims - issuer, expiration, subject, audience
- Public claims - anything custom json about entity
- Private claims - custom claims created to share information between parties that agree on using them
- Base64Url encoded

---

# JWT Payload - Example
- Reserved and Public claims

``` json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

---

# JWT Payload Signature
- encoded header + encode payload
- Use secret and algorithm specified in the header to sign

``` groovy
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

---

# Spring Boot Security
- `compile('org.springframework.boot:spring-boot-starter-security')`

---

# JWT with Spring Security
- `compile('io.jsonwebtoken:jjwt:0.6.0')`

---

# Spring Boot - Access Control
- Use Web config to define access control rules by endpoint
  - Explicitly define public endpoints
  - All others default to secure
- `Authentication` implementation defines system specific authentication behavior
- Use Filters for Authentication and Authorization

---

# Example Spring Web Security config

``` groovy
@Override
protected void configure(HttpSecurity http) throws Exception {
  // disable caching
  http.headers().cacheControl()

  http.csrf().disable()
      .authorizeRequests()
      .antMatchers("/").permitAll()
      .antMatchers(HttpMethod.POST, "/login").permitAll()
      .antMatchers("/admin/**").hasRole("ADMIN")
      .anyRequest().authenticated()
      .and()
  .addFilterBefore(new JWTLoginFilter("/login", authenticationManager()), UsernamePasswordAuthenticationFilter.class)
  .addFilterBefore(new JWTAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class)
}
```

---

# Spring Boot Filters
- Intercept all HTTP requests into server
- Logic can be run before or after the request is handled

---

# Example Authentication Filter

``` groovy
class JWTAuthenticationFilter extends GenericFilterBean{

  @Override
  void doFilter(ServletRequest request, ServletResponse response, FilterChain filterChain) throws IOException, ServletException {
    Authentication authentication = new TokenAuthenticationService().parseAndValidateToken((HttpServletRequest)request)

    SecurityContextHolder.getContext().setAuthentication(authentication)
    filterChain.doFilter(request,response)
  }
}
```
---

# Example Login Filter

``` groovy
class JWTLoginFilter extends AbstractAuthenticationProcessingFilter {

  private TokenAuthenticationService tokenAuthenticationService

  JWTLoginFilter(String url, AuthenticationManager authenticationManager) {
    super(new AntPathRequestMatcher(url))
    setAuthenticationManager(authenticationManager)
    tokenAuthenticationService = new TokenAuthenticationService()
  }

  @Override
  Authentication attemptAuthentication(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse)
      throws AuthenticationException, IOException, ServletException {
    AccountCredentials credentials = new ObjectMapper().readValue(httpServletRequest.getInputStream(), AccountCredentials.class)
    UsernamePasswordAuthenticationToken token = new UsernamePasswordAuthenticationToken(credentials.username, credentials.password)
    return getAuthenticationManager().authenticate(token)
  }

  @Override
  protected void successfulAuthentication(HttpServletRequest request, HttpServletResponse response, FilterChain chain, Authentication authentication)
      throws IOException, ServletException {
    String name = authentication.name
    tokenAuthenticationService.createAndAddTokenToResponse(response, name)
  }
}
```

---

# JWT Token Service
- Issues and validates tokens
- Wire up to Spring's Authentication Processing

---

# JWT Token Service - Issue Token
``` groovy
  private long EXPIRATIONTIME = 1000 * 60 * 60 * 24 * 10 // 10 days
  private String secret = "ThisIsASecret"
  private String tokenPrefix = "Bearer"
  private String headerString = "Authorization"

  void createAndAddTokenToResponse(HttpServletResponse response, String username) {
    String JWT = Jwts.builder()
                     .setSubject(username)
                     .setExpiration(new Date(System.currentTimeMillis() + EXPIRATIONTIME))
                     .signWith(SignatureAlgorithm.HS512, secret)
                     .compact()
    response.addHeader(headerString, tokenPrefix + " " + JWT)
  }
```

---

# Json Token Service - Validate token

``` groovy
Authentication parseAndValidateToken(HttpServletRequest request) {
   String token = request.getHeader(headerString)
   if (token != null) {
     // parse the token.
     String username = Jwts.parser()
                        .setSigningKey(secret)
                        .parseClaimsJws(token)
                        .getBody()
                        .getSubject()

     return username ? new AuthenticatedUser(username) : null
   }
   return null
 }
 ```
