---
title: Web Application Development
layout: default
---

# Angular Bootstrap
## Mike Calvo
## mike@citronellasoftware.com

---
# Modular Components
- Recall creating an app came from the following call that defines a module

``` javascript
angular.module('app', [ ])
```

- The second argument is the list of modules the new module depends on
- You can break up your functionality into libraries of modules that can be reused between applications
- Third-party Angular plugins/modules work this way

---
# Example Module: Angular UI Bootstrap
- Twitter Bootstrap provides a collection of rich web UI controls
  - Examples: dialogs, accordion, tabs, typeahead
- [http://angular-ui.github.io/bootstrap/](http://angular-ui.github.io/bootstrap/)
- The Angular UI module makes these available in an Angular friendly way:
  - Directives
  - Controllers

---
# Adding Angular UI Bootstrap To Grails Project
1. Use bower/grunt to install bootstrap and angular-ui
1. Update application.js and application.css to include dependencies
1. Add the module reference to your app module definition

---
# Install Latest Version of Bootstrap and Angular UI
- From your root directory
`./node_modules/.bin/bower install bootstrap angular-bootstrap --save`
- Verify:
  - Check bower.json
  - Check grails-app/assets/bower_modules

---
# Update JavaScript Asset References
- In grails-app/assets/javascripts/application.js:

``` javascript
//= require jquery/dist/jquery
//= require bootstrap/dist/js/bootstrap
//= require angular/angular
//= require angular-bootstrap/ui-bootstrap-tpls
//= require_self
//= require_tree .
```

---
# Update CSS Asset References
- In grails-app/assets/stylesheets/application.css

``` css
/*
*= require bootstrap/dist/css/bootstrap
*= require bootstrap/dist/css/bootstrap-theme
*= require main
*= require mobile
*= require_self
*/
```

---
# Upate App Module Dependencies
- Where you define your module (likely application.js):

``` javascript
angular.module('app', ['ui.bootstrap']);
```

- Run your app: make sure Angular still without errors to Javascript console

---
# Control 1: Typeahead
- Let's add a typeahead control to our edit artist name control
