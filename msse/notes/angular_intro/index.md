---
title: Web Application Development
layout: default
---

# Introduction to Angular
## Mike Calvo
## mike@citronellasoftware.com

---

# Angular Provides
- Opinionated approach to building single-page web apps
- Small number of components, roles well-defined
- 2-way binding
- Modular, component-based architecture
- Extensible plugin-model

---

# Why Single-Page Apps?
- More responsive
- More Interactive
- Easy way to serve mobile and desktop customers
- Users have come to expect it

---

# Angular Views
- Just plain HTML
- Special attributes tell Angular
  - How to bind data
  - Repeat elements
  - Show or hide elements
  - Respond to events

---

# Angular Controllers
- Make data available in scope for view
- Handle user interactions: button presses, fetching data
- Akin to Grails Controllers

---

# Angular Directives
- Components that manipulate the DOM
- Akin to Grails tags

---

# Angular Filters
- Components that format values for display
- Can also filter out objects that should not be displayed

---

# Angular Services
- Shared code used by multiple controllers or directive
- If two controllers, directives or filters need the same function, put it in a service
- Akin to Grails Services

---

# Angular Resources
- Components for easily fetching and posting REST services
- Configure base url
- All REST methods provided by default
- Customizable

---

# Angular Dependencies
- Angular requires jQuery
- Must be included in the page before the angular include

---

# Setting up Angular on Grails
1. Update Asset Pipeline Plugin
1. Add references to application.js

---

# Install AssetPipeline Plugin
- Edit grails-app/conf/BuildConfig.groovy
- Edit update the asset pipeline plugin:
`compile ":asset-pipeline:2.1.1"`

---

# Asset Pipeline
- Grails plugin to make including of web assets into pages easier
- Included by default in Grails projects
- Assets are placed in grails-app/assets by convention
- Supports javascript, css and images

---

# Asset Pipeline Directives
- Files in the assets folder can contain directives
- Directives (special comments) instruct the file to include other files when serving this file
- Asset files are requested in GSP files via the `<asset>` tag:

``` html
<head>
  <asset:stylesheet href="aplication.css"/>
  <asset:javascript src="application.js"/>
</head>
```

---

# Example Directives
- Include a specific file:
`//= require jquery/dist/jquery`
- Include all files in a directory:
`//= require_tree .`
- Include the contents of the resource
`//= require_self`
- Files in the assets folder are flattened up one level

---

# Including Bower Components
- We configured Bower to install components into assets/bower_components
- We can now include bower assets using resources
- By convention these are included in the files:
  - assets/javascript/application.js
  - assets/stylesheets/application.css

---

# Include jQuery and Angular
- Example application.js
  - We don't need the bower_components because they are rolled up

``` javascript
//= require jquery/dist/jquery
//= require angular/angular
//= require_tree .
//= require_self
```

---

# Use Bower to install jQuery and Angular
- Ensure your .bowerrc file is configured to install in grails-app/assets
- In root project folder:
  `bower install jquery`
  `bower install angular`
- Generate bower.json file:
  `bower init`

---

# Add JQuery and Angular to application.js
- assets/javascript/application.js
- Manifest file for including other javascript assets
- Omit the bower_components folder:

```
//= require jquery/dist/jquery
//= require angular/angular
```

---

# Grails Asset Pipeline Plugin
- Comes as part of Grails
- Manages static web assets in Grails applications
  - Scripts, CSS, images
- Simplifies bundling and uglifying scripts for deployment
- Options for including assets: tags and manifests
- Building war file will 'compile' assets for deployment

---

# Asset Pipleline Tags:

``` html
<head>
  <asset:javascript src="application.js" />
  <asset:stylesheet href="application.css" />
</head>
```

---

# Manifests
- Purpose: instructions for including files into your pages
- Define included assets in one file and use <asset> tag to include that file
- Directives provide include functionality

---

# Asset Pipeline Directives
- require: include a single file
- require_self: include the body of the current file
- require_tree: include all files and subdirectories in the path
- require_full_tree: used for including files from plugins

---

# Initializing Angular App
- Creating an Angular App with a controller:

``` javascript
var app = angular.module('app', []);
app.controller('welcomeController', function($scope) {
  $scope.message = 'Welcome to the Muzic App'l
});
```

---

# Angular HTML
- HTML page must define an 'ng-app' tag which marks the start of the Angular app
- HTML element defines an ng-controller attribute which assigns the controller for that part of the page:

``` html
   <body ng-app="app">
      <div ng-controller="welcomeController">
      {{ message }}
      </div>
   </body>
```

---

# Complete Example

``` html
<!DOCTYPE html>
<html>
<head>
  <asset:stylesheet href="application.css"/>
  <asset:javascript src="application.js"/>

</head>

<body ng-app="app">

<div ng-controller="welcomeController">
  {{ message }}
</div>

<script>
  var app = angular.module('app', []);
  app.controller('welcomeController', function ($scope) {
    $scope.message = 'Welcome to the Muzic App'
  })
</script>
</body>
</html>
```

---

# Summary
- Angular is an opinionated web client framework
- Controllers, Directives, Resources, Services and Filters are major components
- Grails assets pipeline allows for easy inclusion of JavaScript and CSS files
