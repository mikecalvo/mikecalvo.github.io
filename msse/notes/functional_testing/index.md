footer: © Citronella Software Ltd 2015
slidenumbers: true

# Functional Testing
## Mike Calvo
### mike@citronellasoftware.com

---
# Functional Tests
- Testing the view requires an end-to-end style approach
- Functional tests perform this verification
- Server is running
- Test issues an HTTP request
- Verifies the results against actual HTML and JavaScript runner

---
# Grails Functional Tests
- Several plugins exist
- Example: Geb Plugin
- Geb is a Groovy-based browser testing framework
- Wrapper over Selenium
- Multiple web drivers exist (Chrome, Firefox, PhantomJS)

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
```
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
```
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
- Geb and Selenium support a page-oriented approach
- Define Page objects that represent the key interaction elements of a page
- Pages are placed, by convention, in a sub-package called pages
- The test or specification tells the driver to navigate to a page
- Elements exposed by Page are available to the test or spec

---
# Installing Geb Grails Plugin
- Add Geb plugins block of BuildConfig.groovy
- Add Selenium and PhantomJS Test Driver to dependencies block
- Add Geb spock support to dependencies block

---
# BuildConfig.groovy
```
plugins{
  test "org.grails.plugins:geb:0.10.0"
}

dependencies {
  test "org.gebish:geb-spock:0.10.0"
  test "org.seleniumhq.selenium:selenium-support:2.44.0"
  test("com.github.detro.ghostdriver:phantomjsdriver:1.0.1") {
    transitive = false
  }
  // Optionally install other drivers here for debugging...
}
```

---
# Functional Test Setup
- Functional tests will likely require data to complete
- Each functional test should setup their own data
- Functional tests execute outside the Grails app
- How can we leverage domain classes easily?

---
# Remote Control Plugin
- Allows a external process to send commands to Grails app
- Pass closures to RemoteControl instance:
```
def remote = RemoteControl()
def count = remote {
  // This will get run inside Grails server and returned to test!
  return Arist.count()
}
```

---
# Installing Remote Control Plugin
BuildConfig.groovy:
```
dependencies {
  // ... other stuff ...
  compile ":remote-control:1.5"
}
```

---
# Adding a Geb Test
- Geb recommends a page-based approach to web testing
- Each url gets its own Page class which defines the url and interesting elements
- The Geb test navigates to the page and verifies the results and interacts with the page
- Pages typically live in a pages package under the test package

---
# Exmaple Get Artist Page
```
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
```
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
```
import org.openqa.selenium.phantomjs.PhantomJSDriver
import org.openqa.selenium.remote.DesiredCapabilities

driver = {
  new PhantomJSDriver(new DesiredCapabilities())
}
```

---
# Running Functional Tests
`grailsw test-app -functional`
- Launches the grails app first
- Runs all tests in tests/functional

---
# More on Geb
- Interact with JavaScript
- Interact with HTML form elements
- Test file downloading
- Much more:
  - http://www.gebish.org/manual/current/index.html
  - http://www.gebish.org/manual/current/testing.html
