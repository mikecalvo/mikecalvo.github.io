---
title: Web Application Development
layout: default
---
slidenumbers: true

# NoSQL

## Mike Calvo

### mike@citronellasoftware.com

---

# Case Against Relational Data
- Transactional behavior may be too slow for extreme volumes
- Data models a document more than relationships
- Dynamic, frequently changing data structures
- Tree and graph structures require too much joining to be feasible

---

# Types of NoSQL Data Stores
- Key-value store
- Document store
- Graph database
- Column database

---

# Key-value Store
- Examples: Redis, Memcached, Couchbase
- Good for:
  - Persistent hash tables
  - Session tokens
  - Global state
  - Counters

---

# Document-oriented Store
- Examples: MongoDB, CouchDB, Couchbase
- Good for:
  - User profile data
  - Questionnaire data
  - Retail product data

---

# Graph Database
- Examples: Neo4J, OrientDB
- Good for:
  - Social network graphs
  - Directory and tree structures

---

# Column Database
- Examples: HBase, Cassandra, Bigtable
- Good for:
  - Random realtime read/write access to large data sets
  - Time series data

---

# Closer Look: MongoDB
- Database is a set of collections
  - Conceptually similar to tables
- Each collection holds a set of documents
  - Conceptually similar to rows
- Documents are stored as binary JSON (BSON)
- Each document has an Id property for fast lookup

---
# Example MongoDB Document

```
{
  "_id": ObjectId("5248d92ae102251e9e94eb4b"),
  "title": "With or Without You",
  "releaseYear": 1987,
  "artist": {
    "name": "U2"
  }
}
```

---

# MongoDB Features
- Scalability
  - Clustering
  - Sharding
- Files and binary data support
- Extensive cloud support (hosting)
- Command-line interactive shell

---

# MongoDB Cloud Support
- Many options exist for Mongo SAAS providers
- Reduce the need to fully understand complex administration
- Popular options:
  - MongoLab (free developer instance)
  - MongoHQ
  - ObjectRocket

---

# Install MongoDB Locally
- http://www.mongodb.org/downloads
  - Unzip
  - Add the mongo/bin directory to your path
- Mac: `brew install mongodb`
- Interactive shell:
  `mongo <instance> -u user -p password`

---

# Simple Mongo Commands
- Insert a record: `db.artists.insert({name: "U2"})`
- Get the size of a collection: `db.artists.count()`
- Query the collection: `db.artist.find({name: "U2"})`

---

# Setup MongoLab Instance
- Let's setup a new developer instance at MongoLab and use the interactive console to connect to it

---

# Enabling Mongo in Spring Boot
[https://spring.io/guides/gs/accessing-data-mongodb/](https://spring.io/guides/gs/accessing-data-mongodb/)

- Gradle Dependency:
`compile("org.springframework.boot:spring-boot-starter-data-mongodb")`

---

# Mongo Repositories
- MongoRepository

``` groovy
import org.bson.types.ObjectId
import org.springframework.data.mongodb.repository.MongoRepository

interface ReviewNoteRepository extends MongoRepository<ReviewNote, ObjectId> {
  List<ReviewNote> findByRelease(Release release)
}
```

---

# Mapping Classes to Mongo Collections
- Use `@Document` to mark a class as persist-able to Mongo
- Use `@Id` to mark a field as the id property
- Classes can contain both `@Entity` and `@Document`

---

# MongoDb IDs
- Does not support long integers
- Supported Types:
  - java.lang.String
  - java.math.BigInteger
  - org.bson.types.ObjectId

---

# Mixed JPA/Mongo
- Annotate your Config class:
``` groovy  
@EnableJpaRepositories(basePackages = "com.acme.repositories.jpa")
@EnableMongoRepositories(basePackages = "com.acme.repositories.mongo")
```

---

# Configuring MongoDB
- application.properties:

``` groovy
# MONGODB (MongoProperties)
spring.data.mongodb.authentication-database= # Authentication database name.
spring.data.mongodb.database=test # Database name.
spring.data.mongodb.field-naming-strategy= # Fully qualified name of the FieldNamingStrategy to use.
spring.data.mongodb.grid-fs-database= # GridFS database name.
spring.data.mongodb.host=localhost # Mongo server host.
spring.data.mongodb.password= # Login password of the mongo server.
spring.data.mongodb.port=27017 # Mongo server port.
spring.data.mongodb.repositories.enabled=true # Enable Mongo repositories.
spring.data.mongodb.uri=mongodb://localhost/test # Mongo database URI. When set, host and port are ignored.
spring.data.mongodb.username= # Login user of the mongo server.
```

---

# Mongo Resources
- [http://mongodb.org](http://mongodb.org)
- GORM for Mongo:
  - [http://grails.github.io/grails-data-mapping/mongodb/manual/index.html](http://grails.github.io/grails-data-mapping/mongodb/manual/index.html)
- **MongoDB: The Definitive Guide** by Chodorow and Dirolf
