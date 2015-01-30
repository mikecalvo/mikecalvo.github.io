footer: © Citronella Software Ltd 2015
slidenumbers: true

# GORM Queries
## Mike Calvo
### mike@citronellasoftware.com

---

# Where Queries
- GORM adds static where methods to all Domain classes
- The where method takes a closure that allows for powerful query expressions based on one or more properties
- The result of a where query provides a series of execution operations
`<domainClass>.where { <criteria> }.<execution>()`
---

# Example WhereQuery
```
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
```
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

```
log4j = {
  // Other logging configured here...

  debug "org.hibernate.SQL"
  trace "org.hibernate.type.descriptor.sql.BasicBinder"
}
```

---

# Easy Counting and Listing
- GORM provides static count() and list() methods on each domain class
- list() performs a simplequery operations with limited control on results
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
```
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

```
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
```
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
- This is most familiar to Hiberate users
- SQL-like language
- Similar to JDBC style queries with parameters
- Not portable non-relational stores
- Supports everything criteria queries do
- Additionally update queries can be performed via HQL

---

# Example HQL Statements
```
// Find all users with email that contains mike
// Parameters can be positional
User.findAll('from User u where user.email like ?', ['%mike%'])

// Parameters can be named
User.findAll('from User u where user.email like :n', [n: '%mike%'])

// Make all users with email that contains mike disabled
User.executeUpdate('update User u set u.enabled=false '+
  'where u.email like ?', ['%mike%'])
```

# Query Recommendations
- Use where queries when possible (easiest to understand)
- Use the Grails console UI to debug queries
- Use HQL or Criteria for dynamic queries
