---
title: Security Example
layout: default
---

# Git Repository

All examples found on this page can be seen in a working Grails 3 project and cloned at: [https://github.com/mikecalvo/grails3-security-example](https://github.com/mikecalvo/grails3-security-example)

---

# Security Plugin

The Grails REST Security plugin provides token-based, stateless security for Grails 3 applications leveraging the Spring Security framework.  Details on the plugin can be found here [https://grails.org/plugin/spring-security-rest](https://grails.org/plugin/spring-security-rest)

To enable the Grails REST Security plugin, add this dependency to grails.config:

``` Groovy
compile 'org.grails.plugins:spring-security-rest:2.0.0.M2'
compile 'org.grails.plugins:spring-security-rest-gorm:2.0.0.M2'
```

An overview of how the plugin works is shown below:
![Overview](http://alvarosanchez.github.io/grails-spring-security-rest/latest/docs/images/rest.png)
---

#
