footer: © Citronella Software Ltd 2015
slidenumbers: true

# Grails Introduction
## Mike Calvo
### mike@citronellasoftware.com

---

# What Is Grails?
- Rapid web application development platform
- Inspired by concepts in Rails
- MVC Architecture
- Based upon popular open source Java frameworks
  - Spring
  - Hibernate

---

# How Did We Get To Grails?
- Servlet API
- Struts
- Spring MVC
- JSF

---

# Grails Big Idea #1:
# Convention Over Configuration
- No more fighting over the web.xml file
- All projects have a standard dir structure
- Domain classes automatically map to tables

---

# Grails Big Idea #2:
# Agile Philosophy
- Concise code
- See your changes without restarting
- Use dynamic language to create DSLs

---

# Grails Big Idea #3:
# Rock-solid Foundations
- JVM
- Built on proven technologies
- Familiar plugins: Quartz, Lucene, SiteMesh

---

# Grails Big Idea #4:
# Scaffolding and Templating
- Low cost to bootstrapping application
- Creating new app components scripted
- Generate UI based on data model

---

# Grails Big Idea #5:
# Java Integration
- Leverage existing investment in programmer knowledge
- Leverage existing libraries and ecosystem
- Infrastructure connectivity (eg: JDBC drivers)
- Deploy to standard Java Servers

---

# Grails Big Idea #6:
# Incredible Community
- Active mailing list
- Lots of StackOverflow activity
- Manage dependencies
- Plugins, plugins, plugins

---

# Grails Big Idea #7:
# Productivity Ethos
- Eliminate needlessly time consuming tasks
- Make debugging easier
- Reduce time to market

---

# Grails Big Idea #8:
# Emphasize Developer Testing
- Unit tests
- Integration tests
- Mocking

---

# Installing Grails
- GVM
  `gvm install grails`
- Download archive and extract
  - http://grails.org
  - Add grails directory/bin to your path

---

# Creating a project
- Ensure grails installed correctly
  `grails -version`
- Change directory to where you want project to live
  - Top level directory will be created for you

##`grails create-app <project-name>`

^ Go do this and show them around directory structure
^ Grails wrapper
^ run the app

---

# Loading project Into IntelliJ
- Import project
- Existing sources
- Create your Grails libary from your install directory

---

# Adding Grails Project to Git
- Create project on git server
- cd to top folder
`
git init
git remote add origin PATH/TO/REPO
git fetch
git checkout -t origin/master
`

---

# Git Ignore
- Don't check in generated files
- .gitignore file will allow output files to be ignored
- Github will create a default Grails .gitignore file for you

---

# Grails Concepts
- Domain Classes
- Controllers
- Views, Tags, Layouts
- Services

---

# Domain Classes
- The model of your application
- Mapped to tables in an RDBMS
- Class properties are columns
- Table relationships are mapped as object relationships
- Live in grails-app/domain

---

# Controllers
- Respond to HTTP requests
- Actions: methods which respond do different paths
- Live in grails-app/controllers
- Often defined to match Domain Class
  - Customer -> CustomerController
  - http://server/app/customer

---

# Controller Responses
- Direct render (HTML, JSON, XML)
- Model: data used by view
- Default view for controller action
  - Convention-based: grails-app/views/controller/action.gsp

---

# Views
- Responsible for rendering response
- Typically HTML intended for browser
- GSP: Groovy Server Page
- Code + Markup
- References model populated by controller

---

# Layouts
- Templates to provide a common layout for all app pages
- View contents are merged into a layout
- Grails uses the SiteMesh framework for layouts
- Live in grails-app/views/layouts

---

# Tags
- Reusable bits of view code
- Typically generate markup that is injected in place in a GSP
- Radically simpler to create than JSP Custom Tags
- Live in grails-app/taglib

---

# Scaffolding
- Dyamically created CRUD actions and views for a controller
- Good starting point for a basic set of app functionality

```
class ArtistController {
  static scaffold = true
}
```

---

# Services
- Reusable behavior shared by controllers
  - Grails makes all services available to controllers
- Can be transactional
- Live in grails-app/services

---

# Live Updating of Changes
- Development cycle:
  1. Start server
  1. Make code changes
  1. Save
  1. Refresh your browser and see the changes reflected
- No more restart/redeploy of app!

---

# Command line
- Use grailsw to perform many common actions
- ./grailsw run-app
- ./grailsw test-app
- ./grailsw war
- ./grailsw create-controller
