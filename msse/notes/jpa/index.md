---
title: Web Application Development
layout: default
---

# JPA
## Mike Calvo
### mike@citronellasoftware.com

---

#ORM
- Object Relational Mapping
- Your program is a collection of objects
- Your data is relational
- Allow your program to treat data like objects
---

# JPA
- Java Persistence API
- De facto standard for ORM in Java
- Only a specification
- Most commonly used implementation: Hibernate

---

# Core JPA Annotations
- `@Entity`
- `@Id`
- `@GeneratedValue`
- `@Column`
- `@JoinColumn`
- `@OneToMany`
- `@ManyToOne`
- `@ManyToMany`

---

# JPA Annotation Rules
- Any persistent class must be annotated with `@Entity` (or inherit from a class that does)
- All persistent classes must have a field annotated by `@Id` signifying the primary key
- All instance fields in a persistent class are assumed to be mapped to columns with the same name as the field
- `@Transient` will remove a field from mapping
- Relationships are not automatically mapped
  - Relationships are modeled as aggregate members
  - Require an `@OneToMany`, `@ManyToOne`, or `@ManyToMany`

---

# Overriding Defaults
- Class annotation @Table defines specific table mappings
  - Name
- Field annotation @Column defines specific column mappings
  - Name, Nullability, Size, Uniqueness

---

#Relationships
- `@ManyToOne`
  - Annotate a member variable which is an `@Entity` annotated type
- `@OneToMany` or `@ManyToMany`
  - Annotate a member variable which is a standard Java collection with parameter of type which is an `@Entity`
  - Type of collection is significant
  - Use `@JoinColumn` to define specifics about the join column

---

# Relationship Features
- Lazy Loading and Fetch
- Cascading
- Fetch
- Polymorphic

---

# Lazy Loading
- Performance optimization
- Related items are not retrieved until they are first accessed
- Field level access
- Limited to work only within the Session or EntityManager that loaded the parent object
- Causes a big problem in web applications

---

# Fetch
Fetch mode
Lazy
Eager – disable lazy loading
Mode can be configured on each relationship
Consider performance and use when configuring fetch

---

# Cascade
Tells Hibernate whether to follow the path of the relationship on
Insert
Update
Delete
All
Hibernate adds options
Delete All Orphans

---

# Inheritance
- JPA and Hibernate support mapping Class inheritance to relational tables
- Three approaches provided:
  - Table per class hierarchy
  - Table per sub class
  - Table per concrete class

---

# Querying with JPA
- JPAQL/HQL
- Named Queries
- Criteria
- By Example
- Native SQL

---

# JPAQL and HQL
- Query languages that are similar to SQL but are more object-centric
- Supports
  - Selection of object instances from persistent types
  - Selection of properties (rather than columns)
  - Polymorphic selection
  - Automatic joining for nested properties
- Should be very comfortable for those familiar with SQL
- JPAQL is a subset of HQL
- Hibernate implementation will support both
