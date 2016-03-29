---
title: Web Application Development
layout: default
---

# Bower and Angular in Grails

## Marc Kapke

### kapkema@gmail.com

---

# Goal
- Integrate a Grails app with bower packages
 - Angular && Bootstrap
- Use the Grails Asset pipeline to make bower installed dependencies available in app
- Create a basic angular app to verify assets are correctly wired
- Write a functional test to verify app

---

# Gradle Bower Plugin
- Gradle manages Groovy library dependencies for Grails Projects
- The Gradle Bower Plugin integrates Gradle with Bower to provide web (JS and CSS) dependency management
- [https://github.com/craigburke/bower-installer-gradle](https://github.com/craigburke/bower-installer-gradle)

---

#Add plugins to build.gradle
```gradle
plugins {
  id "io.spring.dependency-management" version "0.5.4.RELEASE"
  id 'com.craigburke.bower-installer' version '2.5.1'
}
```
- run `./gradlew idea` to refresh dependencies

---

# Bower dependencies
- Use bower to manage Javascript packages
- Use gradle to manage bower packages like Java (i.e maven)
- Angular and Bootstrap are a good start

---

#Add Angular and Bootstrap
- In build.gradle add bower block

```gradle
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

---

# Build and 'install'
- `.gradlew bowerInstall` will add the mapped source files
- You should see files in the grails-app/assets/bower folder within the angular and bootstrap folders.

---

# Make it automatic
- Update your build.gradle to make `bowerInstall` and `bowerClean` a part of every build and clean
- Add the following right after your bower block

```gradle
compileJava.dependsOn bowerInstall
clean.dependsOn bowerClean
```

---

# Grails Asset Pipeline
- Simplifies bundling and uglifying scripts for deployment
- Options for including assets: tags and manifests
- Building war file will 'compile' assets for deployment

---

# Grails Asset Pipeline Integration
- Application.js and application.css are the two files that integrate with the Grails asset pipeline to bring in the required javascript/css for the page.
- The directives are comments at the top of the file that tell the pipeline which files or trees of files should be loaded.

---

# Asset Pipeline Directives
- require: include a single file
- require_self: include the body of the current file
- require_tree: include all files and subdirectories in the path
- require_full_tree: used for including files from plugins

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

# Add the Javascript assets
- grails-app/assets/javascripts/application.js

```javascript
//= require ../bower/bootstrap/bootstrap.js
//= require ../bower/angular/angular.js
```

---

# Add the CSS assets
- grails-app/assets/stylesheets/application.css

```css
*= require ../bower/angular/angular-csp.css
*= require ../bower/bootstrap/css/bootstrap.css
```

---

# Create Angular App view
- Create, or replace, index.gsp with basic angular app
- Standard html with a few special attributes

```html
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
    <h2></h2>
  </div>

</body>
</html>
```

---

# In the `<head>`
- `asset` elements are grails asset pipeline directives
- `asset:javascript` maps to grails-app/assets/javascripts
- `asset:stylesheet` maps to grails-app/assets/stylesheets

```html  
  <asset:javascript src="application.js"/>
  <asset:stylesheet src="application.css"/>
```

---

# On the `<body>`
- `ng-app` is an angular attribute directive declaring your app
- `"app"` corresponds to the name of angular app defined in application.js

```html
<body ng-app="app">
  ```

---

# The rest
- `ng-controller` attribute directive declares what controller is used for the scope of that element (`<div>`)

```html
<div ng-controller="welcomeController">
```  

- Lots more to discuss on Angular later

---

# Functional Testing
- Geb is a functional testing framework for creating automated UI tests.
- It is bundled by default with Grails.
- Works with many browsers by adding depenency javascripts

---

# Add Chrome and Firefox support
- Add `testDependencies`

```gradle
dependencies {
  // Other dependencies...

  testRuntime 'org.seleniumhq.selenium:selenium-firefox-driver:2.44.0'
  testRuntime 'org.seleniumhq.selenium:selenium-chrome-driver:2.44.0'

}
```

- remember to run ```./gradlew idea``` when you add new dependencies

---

# Tell Geb to use Firefox & Chrome
- GebConfig.groovy is the manifest that tells Geb what browsers to use
- By default, Geb looks in `src/test/resources/GebConfig.groovy`

```groovy
driver = {
  def path = System.properties['os.name'].toLowerCase().contains('windows') ?
      'node_modules\\.bin\\chromedriver.exe' : './node_modules/.bin/chromedriver'
  System.setProperty("webdriver.chrome.driver", path);
  System.properties['browser']?.toString()?.toLowerCase() == 'chrome' ? new ChromeDriver(new DesiredCapabilities()) : new FirefoxDriver()
}
```

---

# Write a test
- Create functional specs in 'src/integration-test'
- use Grails annotation `@Integration`
- extends GebSpec
- Use selectors to locate elements in the page
- compare the elemets contents with expected values

---

# In action

```groovy
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

---

# Run the test
- `./gradlew check`
 - uses default browser from GebConfig.groovy (Firefox)
- `./gradlew check -Dbrowser=chrome`
 - Runs test in chrome

---

# Wrap up
-  http://mikecalvo.github.io/msse/examples/grails_angular/

- kapkema@gmail.com
