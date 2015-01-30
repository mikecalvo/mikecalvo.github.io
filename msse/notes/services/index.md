footer: © Citronella Software Ltd 2015
slidenumbers: true

# Grails Services
## Mike Calvo
### mike@citronellasoftware.com

---
# Services with Detours
- Transactions
- Dependency Injection

---
# Services Defined
- Architectural layer of Grails
- Reusable business logic
- Can be shared by controllers
- Transactional

---
# Conventions
- Live in grails-app/services
- End in 'Services'
  - The name of the service is the part before
  - Example: AlbumService
- All methods in services are transactional by default
- Camel-case version can be injected into other components

---
# Common Service Logic
- All Domain interactions
- External system integrations
- Sending email
- Asynchronous message sending
- Any non-UI handling logic

---
# Detour 1:  Transaction Review
- Sequence of operations that should be treated as a single operation
- Only succeeds if every operation succeeds
- Can be nested
- Commit: finalize the work of the transaction
- Rollback: abort the transaction (no changes)

---
# ACID properties
  **Atomic**:
    -All or nothing
  **Consistent**:
    - System makes sense before and after
  **Isolation**:
   - Results of other transactions are not included
  **Durable**:
    -Permanently made (written to storage)

---
# Classic Account Transfer Example
- Both withdraw and deposit must succeed for transfer to succeed (atomic)
- Sum of both accounts must be the same after transfer (consistency)
- No other withdraws during transfer period (isolation)
- Bank systems must have correct amounts (durability)

---
# Service Transactions
- Based on Spring Transactions
- Default behavior:
  1. Before Method: Start new transaction
  1. Invoke Method: Transactional code executes
  1. Successful return: Commit transaction
  1. Runtime Exception: Rollback transaction

---
# Transactional Behavior
- Only available via dependency injection
- Only works when called externally
- Creating a new service instance will not work
- Spring provides transactional behavior
- Checked exceptions are not automatically rolled back

---
# Hibernate Session and Rollback
- Hibernate session is closed on rollback
- Subsequent attempts to reference lazy loaded relationships will fail
- Solutions:
  - Eagerly fetch the relationship before rollback
  - Redirect to new request after rollback

---
# Changing Service Transactions
- To disable automatic transactions on service:
`static transactional = false`
- @Transactional
  - Mark method or class as transactional
  - readOnly attribute improves performance by not requiring commit after method completes
- Good idea to think about read only methods

---
# withTransaction Alternative
- Grails/GORM provides a withTransaction method on all domain classes
- This allows for fine-grained transactions which can be defined anywhere
  - Controllers
  - Scripts
  - Integration Tests

---
# withTransaction Example

```
class AlbumController {

  def addAlbum(String title, String artistName) {
    Album.withTransaction { TransactionStatus status ->
      Artist artist = Artist.findByName(artistName)
      if (!artist) {
        artist = new Artist(name: artistName).save()
      }
      Album album = Album.findByTitle(title)
      if (album && album.artist.name != artistName) {
        status.setRollbackOnly()
      }
      else {
        new Album(title: title, artist: artist).save()
      }
    }
  }
}
```

---
# Notes on withTransaction
- Service-based transactions are preferred
- Doesn't matter which domain class you call it on

---
# Creating a Service
- Run the grails script:
  `grailsw create-service servicename`
  - create the basic service class and test
- Alternatively, simply create a service class in grails-app/services

---
# Example Service
```
class PostService {
    Post createPost(String userId, String comment) {
      def user = User.findByUserId(userId)
      if (user) {
        def post = new Post(comment: comment)
        user.addToPosts(post)

        if (post.validate()) {
          if (user.save()) {
            return post;
          }
          throw new RuntimeException('Error saving post')
        }
        throw new RuntimeException('Invalid post')
      }
      throw new RuntimeException('No user found')
    }
}
```

---
# Service Spec
```
@TestFor(PostService)
@Mock([User,Post])
class PostServiceSpec extends Specification {
  def "Valid posts get saved and added to the user"() {
    given: "A new user in the db"
      new User(loginId: "chuck_norris",
          password: "password").save(failOnError: true)
    when: "a new post is created by the service"
      def newPost = service.createPost("chuck_norris", "First Post!")
    then: "the post returned and added to the user"
      newPost.content == "First Post!"
      User.findByLoginId("chuck_norris").posts.size() == 1
  }
}
```

---
# Detour #2: Dependency Injection
- Based on Inversion of Control pattern
- class A depends on class B
- Don't have A create B - let a container 'inject' it

---
# Example Without IOC
- Class A cannot be tested without testing class B as well

```
class A {

  A() {
    B b = new B()
    b.doSomething()
  }
}
```

---
# Example With IOC

```
class A {

  B b

  A() {
    b.doSomething()
  }
}

```
---
# Dependency Injection Benefits
- Makes code more unit testable
- Allows for more flexible component relationships
- Supports Aspect-Oriented Programming (AOP)
  - Container injects 'wrapped' version
  - This is how Spring provides transactional support

---
# Dependency Autowiring
- Spring wire beans by name or by type
- In Grails the convention is to wire by name

---
# Other Injectable Spring Beans
- grailsApplication
- sessionFactory
- groovyPagesTemplateEngine
- messageSource

---
# Using A Service
- Inject the service into your controller by adding a member matching your service name:

```
class PostController {
  def postService

  def create() {
    postService.createPost(params.user, params.comment)
  }
}
```

---
# Service Names
- Plugins may provide services
- To avoid name collections when injecting, Grails adds a prefix to the service name matching the name of the plugin
- Example: taxer plugin has a taxService
  `def taxerTaxService`

---
# Service State
- Services are singletons by default
- Likely to be called concurrently
- Storing state in Services generally bad idea
  - Can be used for caching

---
# Changing Service Scope
- Default singleton scope can be changed using static scope property
- Supported scopes:
  - prototype: new services each time injected
  - request: new services for each request
  - session: new service for each session
  - singleton: default (does not need to be specified)

---
# Example Service
- Create a service for the muzic app to add a play of a song
- Look up the artist by name and create it if not found
- Look up the song by name and create it if not found
- Add a play

---
# Testing Transactions
- Integration tests are run inside a transaction by default and are automatically rolled back after each test
  - Integration tests for rollbacks must be marked as transactional = false
- Writing functional tests requires actually writing to the data store
  - Be sure to include flush: true on saves for setup data

---
# Final Recommendations
- Use services for business logic
- Leverage Dependency Injection
  - Prefer singleton scope for Spring beans
  - Use Spring DSL (over XML)
- Update the database within transactions
