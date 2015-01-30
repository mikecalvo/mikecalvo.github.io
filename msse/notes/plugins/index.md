footer: © Citronella Software Ltd 2015
slidenumbers: true

# Grails Plugins
## Mike Calvo
### mike@citronellasoftware.com

---
# Grails Plugins
- Code reuse - done right
- Grails wrappers around
  - Popular Java Frameworks
  - UI libraries
  - Common functionality
- Parts of base Grails platform are plugins
  -GORM

---
# Enhanced Functionality
- Plugins can add functionality to Grails in a variety of ways
- Beans (services) that can be injected into your code
- Tag libs
- New methods and properties on domains, controllers or services
- Build targets
- Test phases

---
# Default External Plugins

```
plugins {
  // plugins for the build system only
  build ":tomcat:7.0.55"

  // plugins for the compile step
  compile ":scaffolding:2.1.2"
  compile ':cache:1.1.8'
  compile ":asset-pipeline:1.9.9"

  // plugins needed at runtime but not for compilation
  runtime ":hibernate4:4.3.6.1" // or ":hibernate:3.6.10.18"
  runtime ":database-migration:1.4.0"
  runtime ":jquery:1.11.1"
}
```

---
# Finding Plugins
- Grails Central Plugin Repository
  `grails list-plugins`

- http://grails.org/plugins
  - More complete list
- Stackoverflow.com also a good source

---
# Installing Plugins
- Add a reference to the plugins section of grails-app/conf/BuildConfig.groovy
- Include:
  - Dependency scope
  - Plugin name
  - Version number
- Plugin site typically specifies this

---
# Configuring Plugins
- As with most Grails configurations, plugins are configured by adding values to grails-app/conf/Config.groovy
- Typically scoped to names specific to the plugin
- Plugin sites will document what options they support

---
# Plugin Files
- Not placed in your source as they should not be checked into SCC
- Default location:
  `$HOME/.grails/<grailsVersion>/projects/<project>/plugins`
- Copied to target folder during build

# Plugin Recommendations
- Pick plugins that are actively worked on
  - Many commits
  - Recent versions
- Well-documented plugins are a good sign
- Consult StackOverflow if more than one exists

---
# Common Plugins
- Security
- Email
- Search
- Data Migrations
- No SQL

---
# Example Built-in Plugin: Cache
- Provides simple cache functionality with app
- Allows responses from methods to be cached
- Examples:
  - Cache a response from a commonly called controller method
  - Cache the response from an service method that returns infrequently changed data

---
# Cache Annotations
- Add these above methods to participate in caching:
- @Cacheable
- @CachePut
- @CacheEvict
- Each takes the name of a cache

---
# @Cacheable
- Annotate method to be cached
- Must specify a name of the cache that holds the value
- Parameters to the method are concatenated to form the key

---
# @CacheEvict
- Annotate method to have the cache remove an object when the method is called
- allEntries can be optionally specified to blast entire cache:
  `@CacheEvict('cache', allEntries=true)`

---
# Example Controller
```
class AristController {

  @Cacheable('popularArtists')
  def popularArtists() {
    // render list of popular artists here
  }

  @Cacheable('arist')
  def getArtist(String name) {
    // render artist with name
  }

  @CacheEvict('popularArtists')
  def resetPopularArtists() {
  }
}
```

---
# Cache Manager
- Cache plugin also adds the grailsCacheManager bean for injecting
- Programmatically evict, add and clear the cache

```
Cache c = grailsCacheManager.getCache('states')
c.evict('WI')
c.add('MI', value)
c.clear()
```

---
# Advanced Caching
- Core caching plugin is very simple
- Designed to be built upon
- More sophisticated caching libraries exist
  -Ehcache, Redis, Gemfire
- They enhance the core caching

---
# Selecting Ehcache Implementation
- Popular Java caching framework
- Allows for complex configuration
- Replace the cache plugin in BuildConfig.groovy:
  `compile: ":cache-ehcache:1.0.4"`
- Ehcache plugin depends on core plugin so the core cache plugin can be removed

---
# Example Ehcache Config.groovy
```
grails.cache.config = {
  defaultCache {
    maxElementsInMemory 1000
    eternal false
    timeToIdleSeconds 120
    timeToLiveSeconds 120
    overflowToDisk true
    memorySToreEvictionPolicy 'LRU'
  }

  cache {
    name 'popularArtists'
    timeToLiveSeconds 60*60*24
  }
}
```

---
# Caching Final Notes
- When writing cache to disk or distributing cached items need to be Serializable
- Cache configurations can be configured per-environment
  - For example: only cache to memory in dev
- Caching plugin also supports caching in the view layer
  - Dynamic portions of pages or custom tag results

---
# Creating Plugins
- Plugins are simply slightly different types of projects
- Grails target creates one:
  `grails create-plugin <name>`

---
# Plugin Project
- Very similar structure to normal Grails project
- Groovy class in the top directory is the main plugin class
  - Define plugin properties (author, version, url)
  - Attach code hooks to various aspects of Grails app

---
# Example GrailsPlugin.Groovy

```
class CoolGrailsPlugin {
  def version = "0.1"
  def grailsVersion = "2.4 > *"
  def pluginExcludes = [ "grails-app/views/error.gsp"]
  def title = "Cool Plugin"
  def author = "Your name"
  def authorEmail = ""
  def description = 'What this thing does!'
  def documentation = "http://grails.org/plugin/cool"

  def doWithWebDescriptor = { xml -> }
  def doWithSpring = { }
  def doWithDynamicMethods = { ctx -> }
  def doWithApplicationContext = { ctx -> }
  def onChange = { event -> }
  def onConfigChange = { event -> }
  def onShutdown = { event -> }
}
```

---
# Trying Plugin Locally
1. Install it
  - Run in plugin project
    `grails maven install`

1. Reference it directly
  - Add in referencing project's BuildConfig.groovy:
    `grails.plugin.location.cool = '../cool'`

---
# Publishing Plugins
- Plugins can be published
  - Central repository
  - Local repository
- Install the Release Plugin
  ```grails publish-plugin```

---
# Plugin Summary
- Before you build look to see if it exists
- Great way to see examples of how to do things
