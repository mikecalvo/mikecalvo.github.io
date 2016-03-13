---
title: GORM Examples
layout: default
---

# Complete Example

See the GitHub [repository](https://github.com/mikecalvo/grails3-angular-example) to see the entire project.

---

# Add Gradle Bower Plugin

See more info [here](https://github.com/craigburke/bower-installer-gradle).  Add to the plugins block of build.gradle:
` id 'com.craigburke.bower-installer' version '2.5.1' ``

# Configure Bower dependencies

Create a new block in build.gradle:

``` groovy
bower {

  'bootstrap'('3.3.x') {
    source 'dist/css/bootstrap.css' >> 'css/'
    source 'dist/css/bootstrap-theme.css' >> 'css/'
    source 'dist/fonts/**' >> 'fonts/'
    source 'dist/js/bootstrap.js'
    excludes 'jquery'
  }

  'angular'('1.3.x') {
    source 'angular.js'
    source 'angular-csp.css'
  }

}```

# Confirm Configuration
Run the following gradle task to verify you've added the plugin correctly and added the bootstrap and angular dependencies.

`./gradlew bowerInstall`

You should see files in the grails-app/assets/bower folder within the angular and bootstrap folders.

---

# Update application.js and application.css
These two files integrate with the Grails asset pipeline to bring in files required for the page.
The directives are comments at the top of these files that tell the pipeline which files or trees of files should be loaded.  See more on the Grails Asset Pipeline [here](https://grails.org/plugin/asset-pipeline).

## Example Directives for application.js
grails-app/assets/javascripts/application.js

``` JavaScript
// This is a manifest file that'll be compiled into application.js.
//
// Any JavaScript file within this directory can be referenced here using a relative path.
//
// You're free to add application-wide JavaScript to this file, but it's generally better
// to create separate JavaScript files as needed.
//
//= encoding UTF-8
//= require jquery-2.1.3.js
//= require ../bower/bootstrap/bootstrap.js
//= require ../bower/angular/angular.js
//= require_self
//= require_tree app
```

-- Example Directives for application.css
grails-app/assets/stylesheets/application.css

``` CSS
/*
*= require_self
*= require ../bower/angular/angular-csp.css
*= require ../bower/bootstrap/css/bootstrap.css
*= require_tree ../bower/bootstrap/css/bootstrap-theme.css
*= encoding UTF-8
*/ ```
