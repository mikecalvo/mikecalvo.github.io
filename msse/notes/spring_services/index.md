---
title: Web Application Development
layout: default
---
slidenumbers: true

# Spring Services

## Mike Calvo

### mike@citronellasoftware.com

---

# `@Bean`
- Annotate a method that returns an instance of a bean that will be available to other Spring components
- Bean is the Java term for component
- Replaces XML configuration for Spring
- Put them into `@Configuration` classes

---
# Example Bean

``` groovy
@Configuration
class AppConfig {

    @Bean
    TransferService transferService() {
        return new TransferServiceImpl()
    }

}
```

---
# Bean Dependencies
- Pass them to the creator method and Spring will inject them

``` groovy
@Configuration
class AppConfig {

    @Bean
    TransferService transferService(AccountRepository accountRepository) {
        return new TransferServiceImpl(accountRepository)
    }

}
```

---
# Bean Lifecycle Events
- Annotate class methods with `@PostConstruct` or `@PreDestroy`
- Implement bean lifecycle interface:
  - InitializingBean, DisposableBean, Lifecycle
- Implement Spring lifecycle interface:
  - BeanFactoryAware, BeanNameAware, MessageSourceAware, ApplicationContextAware
- Specify custom methods in `@Bean` init-method or destroy-method properties

---
# Example Lifecycle implementation

``` groovy
public class Foo {
    public void init() {
        // initialization logic
    }
}

public class Bar {
    public void cleanup() {
        // destruction logic
    }
}

@Configuration
public class AppConfig {

    @Bean(initMethod = "init")
    public Foo foo() {
        return new Foo();
    }

    @Bean(destroyMethod = "cleanup")
    public Bar bar() {
        return new Bar();
    }

}
```

---
# Bean Scope
- Recipe for creating instances of a bean
- singleton: one instance per application
- prototype: bean created for each instance required
- request: bean created for each HTTP request
- session: bean created for HTTP session
- globalSession: portlets (don't use)
- application: servlet context
- websocket

---
# `@Scope`

``` groovy
@Configuration
class MyConfiguration {

    @Bean
    @Scope("prototype")
    Encryptor encryptor() {
        // ...
    }

}
```
---
# Spring Components
- Declare a class as a component and it will be auto-scanned and detected
- Creation of instance is handled by Spring
- Several types based on function
  - Controller, Repository, Service

---
# Service Layer
- Transactional
- Business Logic
- Typically has no state

---
# `@Service`
- Spring service layer stereotype component
- Typically inject Repositories into
- Handle coarse-grained functions

---
# Spring Transactional Support
- Declarative
  - Mark your class or method with `@Transactional`
  - Commits happen automatically on success
  - Rollbacks happen automatically on exception
- Programmatic
  - Helper classes to simplify transaction functionality

---
# Declarative

---
# Programmatic Transactions
- PlatformTransactionManager
  - Based on type of transaction in systems
  - Inspect transaction status
  - Manually commit or rollback
- TransactionTemplate
  - Simplified access to JTA APIs

---
# Example Service

``` groovy
@Service
class ArtistService {

  @Autowired
  ArtistRepository artistRepository

  @Transactional
  Artist findOrCreateArtist(String name) {
    def artist = artistRepository.findByName(name)

    if (!artist) {
      artist = artistRepository.save(new Artist(name: name))
    }

    return artist
  }
}
```

---
# Example Service (Programmatic)

``` groovy
@Service
class ArtistService {

  @Autowired
  ArtistRepository artistRepository

  @Autowired
  PlatformTransactionManager platformTransactionManager

  @Autowired
  TransactionTemplate transactionTemplate

  Artist findOrCreateArtist(String name) {
    transactionTemplate.execute {
      def artist = artistRepository.findByName(name)

      if (!artist) {
        artist = artistRepository.save(new Artist(name: name))
      }

      return artist
    }
  }
}
```

---
# Benefits of Services
- Part of a tiered architectural model
  - Home for business logic
- Declarative transactions simplify transactional logic
