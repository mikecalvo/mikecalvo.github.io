footer: © Citronella Software Ltd 2015
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
- Examples: Redis, Memcached
- Good for:
  - Persistent hash tables
  - Session tokens
  - Global state
  - Counters

---
# Document-oriented Store
- Examples: MongoDB, CouchDB
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
# Grails with MongoDB
- Install MongoDB plugin via BuildConfig.groovy:
  ` compile ":mongodb:1.3.0"`
- Update Config.groovy with MongoDB instance info:
```
grails{ mongo {
  host = 'host'
  port = 12345
  databaseName = 'datanbase'
  }}
```

---
# Mapping Domain Class to MongoDB
- Add this to your Domain class:
  `static mapWith = 'mongo'`
- Optionally add a static mapping section to specify collection, index, etc
  `static mapping { collection = 'artists' }`
- All normal GORM methods work (save, get, count, etc.)

---
# Example MongoDB Mapped Domain
```
class Audit {

  static mapWith = "mongo"

  ObjectId id
  String user
  String action
  Date dateCreated

  static mapping = {
    collection 'audits'
    database 'muzic'
    version: false
  }
}
```

---
# Special Case: Mongo Ids
- Mongo relies on an id it assigns to improve performance and other sharing functionality
- Allowing GORM to define a standar Long id property would break this
- Most of the time, it is better to define the id property explicitly in a Mongo Domain class as type **org.bson.types.ObjectId**

---
# Embedded Data Types
- Hibernate and GORM support embedded relationships for types
- An embedded relationship means that the nested class is _included_ as part of the data of the parent (rather than a separate entity)
- Example: Customer->Address
  - The Address object data will be stored together with the Customer data

---
# Embedded Example
```
class Customer {
  String name
  String email
  Address address
  List<Address> otherAddresses

  static embedded = ['address', 'otherAddresses']
}

class Address {
  String line1
  String city
  String state
  String zip
}
```

---
# MongoDB Domain Relationships
- Since MongoDB stores the entire document, nested relationships in MongoDB are typically modelled as embedded relationships
- Non-embedded relationships (reference) can be used, but will reduce performance

---
# Auditing Example in Action
- Let's use our MongoLab instance and save auditing information from a GORM-mapped domain class
- We'll automatically log a system audit event when the system boots up

---
# Dynamic Attributes
- MongoDB Domain classes allow dynamic properties to be added
- These properties do not need to be defined in the domain class
- When saved, MongoDB Domain class will also persist these dynamic attributes

---
# Dynamic Attributes Example
```
class Audit {

  static mapWith = "mongo"

  ObjectId id
  String user
  String action
  Date dateCreated

}

def a = new Audit(action: 'save', userId: u)
a['hostname'] = InetAddress.getLocalHost().getHostName()
```

---
# Querying MongoDB Domains
- Other than HQL, all GORM queries work with MongoDB Domain classes
- Even dynamic attributes can be queried

---
# Raw MongoDB Access
- The MongoDB plugin includes a GMongo reference that can be used in any controller or service
- This class allows lower-level, direct MongoDB query and update functionality
- To access, declare the 'mongo' property on your controller or service

---
# Mongo Resources
- http://mongodb.org
- GORM for Mongo:
  - http://grails.github.io/grails-data-mapping/mongodb/manual/index.html
- **MongoDB: The Definitive Guide** by Chodorow and Dirolf
