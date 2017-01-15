---
title: Web Application Development
layout: default
---

# JPA and Spring Data
## Mike Calvo
### mike@citronellasoftware.com

---

# ORM
- Object Relational Mapping
- Your program is a collection of objects
- Your data is relational
- Allow your program to treat data like objects

---

# ORM Benefits
- Prevent context switching between SQL and Objects
- Create abstraction layer for persistence functions
- CRUD operations
- Query operations

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
- Fetch mode
  - Lazy
  - Eager: disable lazy loading
- Mode can be configured on each relationship
  - Consider performance and use when configuring fetch

---

# Cascade
- Tells JPA whether to follow the path of the relationship on
  - Insert
  - Update
  - Delete
  - Delete all orphans
  - All of the above

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

---

# Spring Data
- Utility support for interacting with data sources
- Supports JPA and Mongo sources
- Data Repositories

---

# Repositories
- Typed Interfaces: domain class + id
- Support CRUD operations and queries
- Typically do not require implementations
- Added by SpringBoot as beans by default

---

# Repository Types
- Repository
- CrudRepository
- PagingAndSortingRepository
- JpaRepository
---

# Example Repository

``` groovy
interface UserRepository extends JpaRepository<User, Long> {
}
```
---

# Finder Syntax
- Find methods provide querying by method name convention

``` groovy
@Entity class Person {
  @Id Long id
  String firstName
  String lastName
}

interface PersonRepository<Person, Long> {
  List<Person> findByFirstName(String firstName)
  List<Person> findByFirstNameAndLastName(String firstName, String lastName)
}
```
---

# Custom Queries
- Use the `@Query` annotation to define a query

``` groovy
public interface UserRepository extends JpaRepository<User, Long> {

  @Query("select u from User u where u.firstname like %?1")
  List<User> findByFirstnameEndsWith(String firstname);
}
```

---

# Projections
- Retrieve a subset of the properties of the domain
- Define an interface with getter methods for the projected properties
- Return the interface from your Repository find method
- Projections can also contain combinations of properties

---

# Projection Interface Example

``` groovy
interface FullNameAndCountry {

  @Value("#{target.firstName} #{target.lastName}")
  String getFullName();

  @Value("#{target.address.country}")
  String getCountry();
}
```

---

# Criteria Queries
- Define a criteria object that specifies the query
- Repository should extend JpaSpecificationExecutor
- Adds `List<T> findAll(Specification<T> spec)` to Repository

---

# Example specification

``` groovy
public class CustomerSpecs {

  public static Specification<Customer> isLongTermCustomer() {
    return new Specification<Customer>() {
      public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query,
            CriteriaBuilder builder) {

         LocalDate date = new LocalDate().minusYears(2)
         return builder.lessThan(root.get(_Customer.createdAt), date)
      }
    };
  }
}

List<Customer> customers = customerRepository.findAll(CustomerSpecs.isLongTermCustomer());
```

---

# Example Queries
- Specify an example domain object to match in the datastore
- Repository extends QueryByExampleExecutor
- Adds `<S extends T> S findOne(Example<S> example)` to Repository

---

# Query By Example

``` groovy
Person person = new Person(firstName: 'Dave')                       

Example<Person> example = Example.of(person);
```

---

# Adding Spring Data to Spring Boot

``` groovy
compile('org.springframework.boot:spring-boot-starter-data-jpa')
```
