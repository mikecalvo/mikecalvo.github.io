---
title: GORM Examples
layout: default
---

# Complete Example

See the GitHub [repository](https://github.com/mikecalvo/grails3-angular-example) to see the entire project.

---

# Add Gradle Bower Plugin

See more info [here](https://github.com/craigburke/bower-installer-gradle).  Add to the plugins block of build.gradle:

``` groovy
plugins {
  id "io.spring.dependency-management" version "0.5.4.RELEASE"
  id 'com.craigburke.bower-installer' version '2.5.1'
}
```

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

}
```

# Confirm Configuration
Run the following gradle task to verify you've added the plugin correctly and added the bootstrap and angular dependencies.

`./gradlew bowerInstall`

You should see files in the grails-app/assets/bower folder within the angular and bootstrap folders.

---

# Update application.js and application.css
These two files integrate with the Grails asset pipeline to bring in files required for the page.
The directives are comments at the top of these files that tell the pipeline which files or trees of files should be loaded.  See more on the Grails Asset Pipeline [here](https://grails.org/plugin/asset-pipeline).

## Example application.js
grails-app/assets/javascripts/application.js

``` javascript
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

// Create the angular application called 'app'
angular.module('app', []);

// Define a controller called 'welcomeController'
angular.module('app').controller('welcomeController', function($scope) {
  $scope.greeting = 'Hello Stranger'
});
```

---

Create Angular application
Create a controller

``` javascript
angular.module('app', []);

// Define a controller called 'welcomeController'
angular.module('app').controller('welcomeController', function($scope) {
  $scope.greeting = 'Hello Stranger'
});
```

## Example Directives for application.css
grails-app/assets/stylesheets/application.css

``` CSS
/*
*= require_self
*= require ../bower/angular/angular-csp.css
*= require ../bower/bootstrap/css/bootstrap.css
*= require_tree ../bower/bootstrap/css/bootstrap-theme.css
*= encoding UTF-8
*/
```

---

# Create Single Page App index.gsp
The application.js and application.css assets will be loaded into the page using a GSP tag that interacts with the Asset Pipeline plugin.  This will become the 'single page' of your application.

Update grails-app/views/index.gsp to:

``` html
<!doctype html>
<html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">

  <asset:javascript src="application.js"/>
  <asset:stylesheet src="application.css"/>
</head>

<body ng-app="app">
  <h1>Welcome to the sample Grails 3 Angular App</h1>

  <div ng-controller="welcomeController">
    <h2>{{ greeting }}</h2>
  </div>

</body>
</html>
```

---

# Configure Geb
Geb is a functional testing framework for creating automated UI tests.  It is bundled by default with Grails.  By default Geb is configured to use the HtmlUnit browser.

## Add support for Chrome and Firefox
Selenium provides a different jar file dependency for each browser.  Add the Firefox and Chrome dependencies to build.gradle:

``` groovy
dependencies {
  // Other dependencies...

  testRuntime 'org.seleniumhq.selenium:selenium-firefox-driver:2.44.0'
  testRuntime 'org.seleniumhq.selenium:selenium-chrome-driver:2.44.0'

}
```

## Create a GebConfig.groovy file
This is the default location where Geb looks to see which browser should be used for the functional test.  This example shows how to use a system property to switch between Firefox and Chrome.  Put this file into your project at src/test/resources/GebConfig.groovy

``` groovy
import org.openqa.selenium.chrome.ChromeDriver
import org.openqa.selenium.firefox.FirefoxDriver
import org.openqa.selenium.remote.DesiredCapabilities

waiting {
  timeout = 2
}

driver = {
  def path = System.properties['os.name'].toLowerCase().contains('windows') ?
      'node_modules\\.bin\\chromedriver.exe' : './node_modules/.bin/chromedriver'
  System.setProperty("webdriver.chrome.driver", path);
  System.properties['browser']?.toString()?.toLowerCase() == 'chrome' ? new ChromeDriver(new DesiredCapabilities()) : new FirefoxDriver()
}
```

---

# Create Geb Functional Spec
- Create a functional spec in src/integration-test which extends GebSpec.
- Use selectors to locate elements in the page and compare their contents with expected values

``` groovy
package grails.angular

import geb.spock.GebSpec
import grails.test.mixin.integration.Integration

@Integration
class WelcomePageFunctionalSpec extends GebSpec {

  def 'welcome page displays welcome message'() {
    when:
    go '/'

    then: 'Static welcome displayed properly'
    $('h1').first().text() == 'Welcome to the sample Grails 3 Angular App'

    and: 'Angular generated test displayed properly'
    $('h2').first().text() == 'Hello Stranger'
  }
}
```

- Run the test with
  `./gradlew check`
- Change the browser to chrome:
  `./gradlew check -Dbrowser=chrome`
