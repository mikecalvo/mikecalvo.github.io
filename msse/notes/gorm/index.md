---
title: Web Application Development
layout: default
---

# GORM Basics
## Mike Calvo
### mike@citronellasoftware.com

---

# GORM
- Grails Object Relational Mapping
- Simplifies use of Hibernate
- Convention-based
- Best suited for green field data models
- Based on JPA and Hibernate

---

# GORM Values
- No SQL
- No XML
- No Annotations
- Reasonable defaults
- DRY

---

# GORM Basics
- Class maps to a table
- Annotate GORM classes with `@Entity`
- Properties map to columns
- Long type auto-generated property id added to all classes
- All properties are mapped by default

---

# Property Mappings
- GORM provides reasonable type-column mappings for common types
- Integer, String, Float, Date, URL, byte[]
- Custom mappings can be defined if required

---

# Auditing Properties
- GORM treats dateCreated and lastUpdated properties specially
- If defined, these properties are automatically set on save with the current timestamp

---

# Example Domain Class
``` groovy
@Entity
@EqualsAndHashCode(includes="id")
class User {
  String email
  String password
  Date dateCreated
}
```

---

# GORM Provides
- Automatic mapping of domain class to table
- All properties are automatically mapped to columns
- A Long value 'id' property added to all domain classes
  - id property is auto generated on create operation

---

# CRUD Operations
- GORM automatically adds static and instance methods for:
  - Create: instance method save()
  - Read: static methods get(id) and read(id)
  - Update: instance method save()
  - Delete: instance method delete()

---

# Domain Integration Tests
- Integration tests create a GORM environment complete with in memory database
- Testing GORM functionality is not interesting
- Verifying the configuration of GORM is
  - It can become very complex with relationships, constraints, and cascades
- Domain classes should be tested with integration tests

---

# Create Domain Integration Test
- Prove that the value is saved and an id is generated

``` groovy
  def 'saving user persists new user in database'() {
    setup:
    def user = new User(email: 'mike@calvo.com', password: '123456')

    when:
    user.save()

    then:
    user.errors.errorCount == 0
    user.id
    user.dateCreated
    User.get(user.id).email == 'mike@calvo.com'
  }
```

---

# 'Silent Failure'
- No exception is thrown if the save fails by default
- failOnError: true can be passed to save() to cause it
- After failure the errors property is populated on the instance with information regarding failure
- This can be the source of frustration for those new to GORM

---

# Update Domain Integration Test

``` groovy
def 'updating a user changes data'() {
  setup:
  def user = new User(email: 'joe@smith.com', password: '7654321').save(failOnError: true)
  user.save(failOnError: true)

  when:
  def foundUser = User.get(user.id)
  foundUser.password = 'secure'
  foundUser.save(failOnError: true)

  then:
  User.get(user.id).password == 'secure'
}
```

---

# Delete Domain Integration Test

``` groovy
def 'deleting an existing user removes it from the database'() {
  setup:
  def user = new User(email: 'pat@ska.com', password: 'skillz')
  user.save(failOnError: true)

  when:
  user.delete()

  then:
  !User.exists(user.id)
}
```

# Create/Update
- The behavior of save() depends on the id property
  - null id results in a create
  - non-null id results in an update
- After successful save
  - id property is guaranteed to have a non-null value
  - if they exist dateCreated and lastUpdated will have values

---

# Constraints
- Place constraints on the properties in domain class
- Conceptually similar to database constraints
- Constraints are automatically checked by GORM before saves
- GORM adds a validate() instance on domain classes which can be called without saving

---

# Example Constraints
  - Nullable
  - Min and max sizes
  - Unique
  - Regular expression patterns (url, email)

---

# GORM Constraints
- Declare a static constraints closure in your domain class
- Each property can be specified with a list of constraints
- By default properties are not nullable - must be declared as such

---

# Domain Constraints Example

``` groovy
class User {

  String email
  String password
  String status
  Date dateCreated

  static constraints = {
    email nullable: false, email: true, unique: true
    password nullable: false, size: 6..12
    status nullable: true, size: 0..64
  }
}
```

---

# Domain Constraints Example Test

``` groovy
def 'saving a user with invalid properties fails to validate'() {
  setup:
  def user = new User(email: 'not', password: '123', status: 'a'*200)

  when:
  def result = user.save()

  then:
  !result
  user.hasErrors()
  user.errors.errorCount == 3
  user.errors.getFieldError('email').rejectedValue == 'not'
}
```
---

# Other Validators
- blank: for strings prevents nulls or empty string
- inList: is one of a range of values
- validator: custom validation with a closure
- matches: regular expression

---

# Sharing Constraints
- importFrom constraint takes a domain type and a set of constraints to include or exclude
- Example:

``` groovy
static constraints {
  importFrom User, include ['password']
}
```

---

# GORM Relationships
- Domain classes can be related to each other
- Cardinality models relationship
  - Instance -> Collection: one-to-many
  - Instance -> Instance: one-to-one
  - Collection -> Collection: many-to-many
- Directionality: one-way vs bi-directional

---

# One-to-one Relationships
- hasOne declares that one instance belongs to this domain class
- Owned side simply has a reference to an instance of an owner
- Relationships can have constraints

---

# Example One-to-one

``` groovy
class User {
  String username
  String password

  static hasOne = [profile: Profile]
  static constraints = {
    profile nullable:true
  }
}

class Profile {
  User user
  String fullName
}
```

---

# One-to-many Relationships
- Model relationships of 1*n cardinality
- For example: Order has Line Items
- The one side defines hasMany map listing the relationship name and type
- The many side defines belongsTo map listing the relationship name and type of one side

---

# Example One-to-many

``` groovy
class Order {
  Date orderDate
  static hasMany = [lines: OrderLine]
}

class OrderLine {
  Integer number
  Integer quantity
  String sku
  static belongsTo = [order: Order]
}
```

---

# Lazy and Eager Fetching
- By default GORM uses a lazy fetch pattern for collection relationships
- The collection is not retrieved as part of retrieving the owning instance
- It is retrieved when the relationship is first accessed on the owning instance
  - This happens when someone references the property

---

# Eager Fetching
- The related items are fetched with the owning instance
- Eager fetching can be forced on collections
- By default one-to-one relationships are eager fetched

---

# Cascading
- Cascading affects how and when related objects are saved or deleted
- A cascade only occurs on objects with belongsTo defined
- For example, when an Order is deleted, all of its corresponding OrderLines are also deleted

---

# Adding and Removing
- GORM automatically adds methods for adding and removing related objects when a hasMany is defined
  - addTo<PropertyName>s
  - removeFrom<PropertyName>s
- For example
  - Order.addToLines()
  - Order.removeFromLines()

---

# Sorting Many Side
- By default, 'hasMany' stores related items in a set
- To keep items in order, GORM provides the mapping closure
- The many side can define the property and direction for how it should be sorted
- The one side can define a mapping closure with sort the sort property

---

# Example Sorted One-to-many

```  groovy
class Order {
  Date orderDate
  static hasMany = [lines: OrderLine]
  static mapping {
    lines sort:'number' // this affects Order.lines.each calls
  }
}

class OrderLine {
  Integer number
  Integer quantity
  String sku
  static belongsTo = [order: Order]
  static mapping {
    sort number:'asc'  // this affects OrderLine queries
  }
}
```

---

# Many-to-many Relationships
- Use hasMany on both sides of a many-to-many relationship
- The owned side of the relationship should also have a belongsTo defined
- Many-to-many can also be self-referring
- Cascades only happen when a belongsTo exists

---

# Self-referring Many-to-many Example

```  groovy
class User {
  static hasMany = [followers : User, following : User]
  // This will have addToFollowers, removeFromFollowers,
  //   addToFollowing, removeFromFollowing
  // Updates will not cascade to followers or following
}
```

---

---

# Where Queries
- GORM adds static where methods to all Domain classes
- The where method takes a closure that allows for powerful query expressions based on one or more properties
- The result of a where query provides a series of execution operations
`<domainClass>.where { <criteria> }.<execution>()`
---

# Example WhereQuery

``` groovy
// find all users whose email is mike@*
User.where { email =~ "mike@%"}.list()
```

---

# Query Syntax
- The left side of any comparison is ALWAYS a property on the domain class
- Comparison operators are common Groovy comparisons: ==, <, > ~=, !=, etc
- Logical OR and AND operators are also available for complex, multi-value queries
- Properties can be nested within related properties

---

# Examples Queries

``` groovy
// Find all users who's email starts with
//    case insensitive joe and were created after a date
User.where { email ==~ 'joe%' && dateCreated > fromDate }.list()


// Only limit by date if it was included
User.where {
  email ==~ 'joe%'
  if (fromDate) {
    dateCreated > fromDate
  }
}.list()

Post.where { post.user.email == userEmail }
```

---

# Controlling Results
- The list() function on the where query allows for filtering of results
- max: The maximum number to return
- offset: The number of elements into the ResultSet to start at
- sort: The field to sort on
- order: 'asc' or 'desc'
- fetch: Eager/lazy fetch strategy as a Map of options

---

# Getting a single result
- The get() function returns a single value (rather than multiple via list())
- get() also takes the result parameters list() does

---

# Enabling SQL Logging
- Sometimes it is necessary to tune queries based on performance
- Handy to view the actual SQL being executed
- DataSource.groovy:
`logSql = true on your dataSource`

---

# Viewing SQL Query Parameters
- To view all values passed to database, including query parameters dial up the logging on these Hibernate classes in your Config.groovy:

``` groovy
log4j = {
  // Other logging configured here...

  debug "org.hibernate.SQL"
  trace "org.hibernate.type.descriptor.sql.BasicBinder"
}
```

---

# Easy Counting and Listing
- GORM provides static count() and list() methods on each domain class
- list() performs a simple query operations with limited control on results
  - max, order, offset, fetch, sort are all supported
- count() returns a count of all items in the corresponding table

---

# Dynamic Find and Count Methods
- GORM provides findBy<Property> and countBy<Property> methods for each property
- They can look for specific values:
  `User.findByEmail('joe@jones.com')`
- The can look by patterns:
  `User.countByEmailILike('%joe%')`
- They can combine properties:
  `User.findByEmailAndPassword(e, p)`

---

# Criteria Queries
- Construct your queries using a builder style pattern
- Combine criteria using and() and or() methods
- Criteria are methods (instead of operators in where queries)
- Useful for constructing dynamic queries

---

# Example Criteria Query

``` groovy
User.withCriteria {
  and {
    ilike 'email', '%mike%'
    if (fromDate) {
      ge 'dateCreated', fromDate
    }
  }
}
```

---

# Referencing Related Properties
- Create a block with the related field

``` groovy
// Find all songs who's artist is U2
Song.withCriteria {
  artist { eq 'name', 'U2' }
}
```

---

# Query Projections
- Projections are aggregations or filtering functions applied to queries
- They provide for grouping by properties
- Commonly used projection functions are count, min, max, avg, and sum
- Results for projections are lists of lists rather than domain instances

---

# Example Projection
- Find a count of songs by artist name

``` groovy
Song.withCriteria {
  createAlias 'artist', 'a'  // this lets me reference artist in projection

  projections {
    groupProperty 'a.name'
    count id
  }
}
```

---

# HQL
- This is most familiar to Hibernate users
- SQL-like language
- Similar to JDBC style queries with parameters
- Not portable non-relational stores
- Supports everything criteria queries do
- Additionally update queries can be performed via HQL

---

# Example HQL Statements

``` groovy
// Find all users with email that contains mike
// Parameters can be positional
User.findAll('from User u where user.email like ?', ['%mike%'])

// Parameters can be named
User.findAll('from User u where user.email like :n', [n: '%mike%'])

// Make all users with email that contains mike disabled
User.executeUpdate('update User u set u.enabled=false '+
  'where u.email like ?', ['%mike%'])
```

---

# Query Recommendations
- Use where queries when possible (easiest to understand)
- Use HQL or Criteria for dynamic queries

---

# Bootstrap Considerations
- Running application should respond gracefully to requests
- Local development servers run with in-memory databases which are wiped clean on each run
- Consider what data needs to be loaded in the local app to make it functional

---

# Suggestions
- Check the size of key domain classes to see if they have no data
  - If not, run a populate sample data method
- When creating sample setup data, always use failOnError: true
  - This will give you errors at startup time that sample data was not generated
- Utilize environment metadata

---

# Environment Metadata
- Describes the current active spring environment (development, test, produnction)
- Bootstrap provides an environment block which will only run when the environment matches the block
- Alternatively check Environment.current to see which is currently running

---

# Environment Examples

``` groovy
class BootStrap {
  def init = { servletContext ->
    environments {
      development {
        createSampleData()
      }
    }

    if (Envrionment.current != Environment.DEVELOPMENT) {
      // do some non development stuff here
    }
  }
}
```
