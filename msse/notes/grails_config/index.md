footer: © Citronella Software Ltd 2015
slidenumbers: true

# Grails Configuration
## Mike Calvo
### mike@citronellasoftware.com

---

# Overview
- Grails provides configuration scripts for the following:
  - Build
  - Spring beans
  - Database access
  - Bootstrap functinality
  - URL mappings

---

# Configuration Home
- All configurations live in grails-app/conf
- Config.groovy contains grails-specific and custom properties settings for your app
- project_home/application.properties contains app name and version properties

---

# Build Config
- grails-app/conf/BuildConfig.groovy
- Configures project dependencies
- Configures dependency repositories
- Configures plugin dependencies

---

# Dependency Management
- Declare the creator, library and version for any dependency
- Uses Maven style dependency management:
  `scope creator:library:version`
- example: `runtime 'mysql:mysql-connector-java:5.1.29'`
- Repositories section provides the ability to add custom dependecy sources

^ Show the default file

---

# Maven Scopes
- compile: default scope, available in all classpaths of project
- provided: expect the JDK or a container to provide it
- runtime: not required at compile time
- test: required for running and compiling tests only
- system: similar to provided, but must be explicitly given. uncommon.

---

# Enable Debugging
- Important setting found in Build.Config
  - grails.project.fork
- Must be set to false for both test and run to allow debugger to stop at breakpoints

``` groovy
grails.project.fork = [
  test: false
  run: false
]
```

---

# Spring Config
- grails-app/conf/spring/resources.groovy
- Spring is a dependency injection (DI) framework
- Concept: allow a container to wire system components together
- Spring uses the beans convention for getting/setting properties

---

# Simple Spring Bean Example
- Create a component to extract lyrics from a web response
- Define a new Groovy class in src/groovy
- Add bean definition to grails-app/conf/spring/resources.groovy
- Add a member variable with the bean name to a controller
- Write an integration test verifying that the bean is wired correctly

---

# Database Config
- grails-app/conf/DataSource.groovy
- Configure data source: pooled database resource manager
- Configure by settings by environment
- By default Grails comes with Java-based RDBMS called H2
- Hibernate settings also configured here

---

# dbCreate Options
- The dataSource.dbCreate property controls what Hibernate does on startup
- Domain Classes are synchronized with RDBMS
- Several options exist: create, create-drop, update, validate
- Can be set differently for each environment

---

# Bootstrap Functionality
- BootStrap.groovy files in grails-app/conf get run at system startup and shutdown
- Classes can use Domain and GORM classes to persist bootstrap data
- Code can be targeted to an environment
- grails-app/conf/BootStrap.groovy

---

# Example BootStrap

``` groovy
class BootStrap
def init = { ServletContext ctx ->
  environments {
    production {
      ctx.setAttribute("env", "prod")
    }
    development {
      ctx.setAttribute("env", "dev")
      new Artist(name: 'U2').save()
      new Artist(name: 'New Order').save()
    }
  }
  ctx.setAttribute("foo", "bar")
}
```

---

# URL Mappings
- URL Mappings configures how HTTP requests are routed to controllers
- Also defines default view mappings and error mappings
- grails-app/conf/UrlMappings.groovy

---

# Default URLMappings.groovy

``` groovy
class UrlMappings {

  static mappings = {
    "/$controller/$action?/$id?(.$format)?"{
      constraints {
        // apply constraints here
      }
    }

    "/"(view:"/index")
    "500"(view:'/error')
  }
}
```
