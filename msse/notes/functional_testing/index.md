---
title: Web Application Development
layout: default
---

# Functional Testing
## Mike Calvo
### mike@citronellasoftware.com

---
# Functional Tests
- Testing the view requires an end-to-end style approach
- Functional tests perform this verification
- Server is running
- Test issues an HTTP request

---
# Grails Functional Tests
- Grails includes Geb Framework
- Geb is a Groovy-based browser testing framework
  - Wrapper over Selenium
- http-builder library: simplify making HTTP requests

---
# http-builder
[https://github.com/jgritman/httpbuilder/wiki](https://github.com/jgritman/httpbuilder/wiki)

build.gradle:

``` groovy
testCompile 'org.codehaus.groovy.modules.http-builder:http-builder:0.7'
```

---
# Example http-builder

``` groovy
import groovyx.net.http.HTTPBuilder
import static groovyx.net.http.Method.GET
import static groovyx.net.http.ContentType.TEXT

// initialze a new builder and give a default URL
def http = new HTTPBuilder( 'http://www.google.com/search' )

http.request(GET,TEXT) { req ->
  uri.path = '/mail/help/tasks/' // overrides any path in the default URL
  headers.'User-Agent' = 'Mozilla/5.0'

  response.success = { resp, reader ->
    assert resp.status == 200
    println "My response handler got response: ${resp.statusLine}"
    println "Response length: ${resp.headers.'Content-Length'}"
    System.out << reader // print response reader
  }

  // called only for a 404 (not found) status code:
  response.'404' = { resp ->
    println 'Not found'
  }
}
```

---
# Selenium
- Browser automation tools and frameworks
- Automate running web applications
- Primarily use for testing
- Web-based administration tasks

---
# Selenium WebDriver
- Language-specific bindings to drive a browser
- Desktop browsers: Chrome, Firefox, IE, Safari
- Mobile browsers: Android
- Headless: PhantomJS, HtmlUnit
- Languages: C#, Java, JavaScript, Objective-C, Ruby

---
# Example Selenium Java Test

``` groovy
WebDriver driver = new FirefoxDriver();
driver.get(myAppUrl);
driver.findElement(By.id('textInput')).sendKeys('U2');
driver.findElement(By.id('submitBtn')).click();
assert driver.findElement(By.id('bandNameHeading')).getText() == 'U2';
```

---
# Geb
- Groovy layer built on top of Selenium WebDriver
- Content DSL
  `assert title == 'Welcome to Muzic'`
- jQuery style content inspection model:
  `$("#submitBtn").click();`

---
# Geb DSL Example

``` groovy
import geb.Browser

Browser.drive {
  go "http://google.com/ncr"

  // make sure we actually got to the page
  assert title == "Google"

  // enter wikipedia into the search field
  $("input", name: "q").value("wikipedia")

  // wait for the change to results page to happen
  // (google updates the page dynamically without a new request)
  waitFor { title.endsWith("Google Search") }

  // is the first link to wikipedia?
  def firstLink = $("li.g", 0).find("a.l")
  assert firstLink.text() == "Wikipedia"

  // click the link
  firstLink.click()

  // wait for Google's javascript to redirect to Wikipedia
  waitFor { title == "Wikipedia" }
}
```

---
# More Geb Details
- Functional tests live under test/functional
- Geb works with both JUnit and Spock
- Geb tests are running outside the Grails app
- Cannot directly use GORM, Controllers, or Services
- Provides jQuery style element lookup within Groovy

---
# Page Object Pattern
- Geb encourages a page-oriented approach
- Define Page objects that represent the key interaction elements of a page
- Pages are placed, by convention, in a sub-package called pages
- The test or specification tells the driver to navigate to a page
- Elements exposed by Page are available to the test or spec

---
# Benefits of Page Object Pattern
- All page-specific details are contained in the page Object
- Create logical representations of the page that remain more consistent than DOM
- Many tests can reuse common page definitions

---
# Adding a Geb Test
- Geb recommends a page-based approach to web testing
- Each url gets its own Page class which defines the url and interesting elements
- The Geb test navigates to the page and verifies the results and interacts with the page
- Pages typically live in a pages package under the test package

---
# Exmaple Get Artist Page

``` groovy
import geb.Page

class ArtistGetPage extends Page {

  static url = 'artist/get'

  static content = {
    id { $("#id") }
    name { $("#name") }
  }

}
```

---
# Example Geb Functional Test

``` groovy
@Integration
class ArtistFunctionalSpec extends GebSpec {

  // setup code...

  def "gets artist details"() {
    when:
    to ArtistGetPage, id: artistId

    then:
    name.text() == 'U2'
    id.text() == "Id: ${artistId}"
  }
}
```

---
# Configuring Geb
- Geb needs to know which test drivers to use
- Create a file called GebConfig.groovy in the root of tests/functional
- Define one or more configurations
- Type of drivers
- Browser size, behaviors, etc

---
# Simple GebConfig.groovy
``` groovy
import org.openqa.selenium.phantomjs.PhantomJSDriver
import org.openqa.selenium.remote.DesiredCapabilities

driver = {
  new PhantomJSDriver(new DesiredCapabilities())
}
```
---
# More on Geb
- Interact with JavaScript
- Interact with HTML form elements
- Test file downloading
- Much more:
  - [http://www.gebish.org/manual/current/index.html](http://www.gebish.org/manual/current/index.html)
  - [http://www.gebish.org/manual/current/testing.html](http://www.gebish.org/manual/current/testing.html)
