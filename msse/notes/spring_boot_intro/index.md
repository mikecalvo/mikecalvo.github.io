---
title: Web Application Development
layout: default
---
autoscale: true
theme: Next, 3

## Web Services / Spring Boot Intro
### Adam Keyser
#### adam.keyser@gmail.com

---

## Agenda
- Spring Overview
- Spring Boot basics
- Testing
- Spock testing framework

---

## Spring
- One of the popular Java frameworks
- Foundation of Spring Boot
- First release 2002 - 1.0 in 2004
- Maintained by Pivotal Software

---

## Spring - Goals
- Dependency injection
- Decoupling
- Reduces boilerplate
- Testability

---

## Configuration
- Initially xml (Still possible)
- Annotations
- Move to automation of redundancy(Spring Boot)

---

## How it works
  1. Classpath scanning
  1. Create objects with defined annotations
  1. Objects are placed into container (Application Context)
  1. Servlet tie in to Application Context (Web Application Context)

---

## Spring Bean lifetime types
- Prototype (new instance)
- Singleton (only one instance)
- When in doubt, Singleton

---

## Spring Bean Stereotypes
- @Component - Generic
- @Repository - Data Access
- @Service - Service Layer
- @Controller - Controller Layer

---

## Popular Spring Add-Ons
- Spring MVC
- Spring Security
- Spring Data
- Spring Rest
- Spring Cloud

---

## Spring Boot

---

## Guiding Philosophy:
- Opinionated patterns to solve common issues
- Start out with a defined pattern of doing things
- Get out of the way quickly when things get more complicated

---

## Convention over configuration
- Framework handles common decision making patterns
  1. Domain classes map to table names by default
  1. Project folder structure constant through projects
  1. Reduce decision making without losing flexibility

---

## Key components
- Spring Boot Starters
- Spring Boot Auto Configurator
- Spring Boot CLI  
- Spring Boot Actuator

---

## Spring Boot Isn't
- Application server
- Specification implementation
- Code generator

---

## Grouping of popular, battle tested frameworks
- Gradle or Maven for building
- Spring for dependency management/configuration
- JPA (Java Persistence API) for Data Access

---

## Included servlet container for running applications
- Embedded in your built JAR
- Tomcat by default, can be swapped out
- Fully tunable and configurable

---

## Spring Boot Internals
- @SpringBootApplication is comprised of:
  1. @ComponentScan
  1. @EnableAutoConfiguartion
  1. @Configuration

---

## Spring Boot starter modules
- Spring provides starter modules for getting up and running quickly

- [https://github.com/spring-projects/spring-boot/tree/master/spring-boot-starters](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-starters)

---

## Spring CLI
- Command line interface for managing spring projects
- Powerful: Can create, run, and modify projects
- [CLI Documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/cli-using-the-cli.html)

---

### Installing the Spring Boot CLI

``` bash
> curl -s "https://get.sdkman.io" | bash
```

``` bash
> sdk install springboot
```

### Test it out

``` bash
> spring --version
```

---

## Demo

```groovy
@RestController
class HelloController {
	@RequestMapping("/hello/{name}")
	def hello(@PathVariable String name) {
		return [greeting: "Hello, " + name + "!" ]
	}
}
```

- Run it

``` bash
> spring run HelloController.groovy
```

---

## Creating Spring Boot projects

Many ways to start a spring project

- Spring CLI

``` bash
> spring init --dependencies=web,data-jpa --build gradle --language groovy my-project
```

- Spring Initializer [http://start.spring.io/](http://start.spring.io/)

---

## Creating Spring Boot projects (IntelliJ)

- IntelliJ IDEA integration (Non-CE)
  - Spring Initializer from new project
- CE IntelliJ Steps
  1. Create project using Initializer
  1. Import project into IntelliJ

---

## Building Spring Boot

---

## Gradle
- Groovy-based build system
- root_project_folder/build.gradle
- [http://gradle.org](http://gradle.org)

---

## Cont.
- Repositories: where Gradle looks for dependencies
- Plugins: extensions to Gradle
- Dependencies: libraries your app requires

---

## Configuration files
  - gradle.properties For gradle properties
  - settings.gradle For other modules within a project

---

## Dependency Management
- Declare the creator, library and version for any dependency
- Uses Maven style dependency management:
  `scope creator:library:version`
- example: `runtime 'mysql:mysql-connector-java:5.1.29'`
- Repositories section provides the ability to add custom dependency sources

---

## Maven Scopes
- compile: default scope, available in all classpaths of project
- provided: expect the JDK or a container to provide it
- runtime: not required at compile time
- test: required for running and compiling tests only
- system: similar to provided, but must be explicitly given. uncommon.

---

## Using Gradle
- Always use `./gradlew` "the wrapper"
- Command Options
  1. build (builds and tests the project)
  1. test (only run tests)
  1. clean (remove all class files)
  1. tasks (see all available tasks)

- ex. `./gradlew clean build`
