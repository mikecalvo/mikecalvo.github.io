---
title: Web Application Development
layout: default
---

# UI Testing
## Marc Kapke
### kapkema@gmail.com

---

# UI Tests

- UI tests perform end-to-end verification
- UI tests perform this verification
- Server is running
- Verifies the results against actual HTML and JavaScript runner

---

# Selenium

- Browser automation tools and frameworks
- Automate running web applications
- Primarily used for testing
- Web-based administration tasks

---

# Selenium WebDriver

- Language-specific bindings to drive a browser
- Desktop browsers: Chrome, Firefox, IE, Safari
- Mobile browsers: Android
- Headless: PhantomJS, HtmlUnit

---

# Example Selenium Test

``` groovy
WebDriver driver = new FirefoxDriver();
driver.get(myAppUrl);
driver.findElement(By.id('textInput')).sendKeys('U2');
driver.findElement(By.id('submitBtn')).click();
assert driver.findElement(By.id('bandNameHeading')).getText() == 'U2';
```

---
# Geb

- Geb is a Groovy-based browser testing framework
- Groovy layer built on top of Selenium WebDriver
- Multiple web drivers exist (Chrome, Firefox, PhantomJS)
- Content DSL
  `assert title == 'Nyt Viewer!'`
- jQuery style content inspection model:
  `$("#submitBtn").click();`

---

# Geb DSL Example

``` groovy
import geb.Browser

Browser.drive {
  go "http://google.com/ncr"

  assert title == "Google"

  $("input", name: "q").value("wikipedia")

  waitFor { title.endsWith("Google Search") }

  def firstLink = $("li.g", 0).find("a.l")
  assert firstLink.text() == "Wikipedia"

  firstLink.click()

  waitFor { title == "Wikipedia" }
}
```

---

# More Geb Details

- Geb works with both JUnit and Spock
- Geb tests are running outside Spring Boot app
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

# Browser

- Entry point to Geb is `Browser` object
- Connects `WebDriver` to current page

---

# Browser `drive()`
- `drive()` enables browser scripting
- closure param evaluates against browser instance
- navigate with `go()`
- use `$()` to interact with content

---

``` groovy
Browser.drive {
    go "http://google.com"
    assert $("h1").text() == "Google"
}
```

---

# Interacting with Content
- `$()` is access point to current page content
- function has 3 params - all are optional
- `$(«css selector», «index or range», «attribute / text matchers»)`
- Matchers returns `Navigator` object

---

# CSS selector matching
- Any CSS selector the WebDriver supports is valid
- Most CSS 3 selectors supported
- [CSS3 Selectors](https://www.w3.org/TR/css3-selectors/)

---

# CSS selector syntax
- element: `h1`
- class: `.some-class` or `h1.some-class`
- id: `#some-element`
- Advanced: `$('div.some-class p:first-child[title^="someth"]')`

---

# Index Matching
- A single positive integer or integer range will restrict Matching
- First paragraph element: `$("p", 0)`
- 3rd paragraph element: `$("p", 2)`

---

# Attribute Matching
- Match based on attributes defined on an element
- Can provide multiple attributes to narrow matching
- Find left aligned paragraph element: `$("p", align: "left")`
- Find paragraph matching two attributes: `$("p", align: "left", attr2: "foo")`

---

# Text matching
- The attribute value text with match against the node's text.
- Find paragraph with text `foo`: `$("p", text: "foo")`

---

# Text/Attribute Pattern matchers
- Multiple pattern methods available
- startsWith, contains, endsWith
- case insensitive versions prefixed with `i`: `iStartsWith`

---

# RegEx Pattern matchers
- RegEx pattern matching available
- Paragraph with text that tarts with `p`: `$("p", text: ~/p./)`

---

# Content Matching example
- `$("h1", 2, class: "heading")`
- Find the 3rd `h1` element with class attribute equal to heading.

---

# Navigator
- Result of a content match
- JQuery DOM traversal interface
- Traverse DOM with `previous()`, `next()`, `parent()`, etc..
- Provides iterator, find, filter, etc...
- `displayed` - determines visibility of element

---

# Element interaction
- Navigator object provides element interaction
- `click()` - executes mouse click on that element
- Send keystrokes to a specific element
- Set form control values

---

# Element interaction
- Click: `$("a").click()`
- Send keys: `$("input") << "foo"`
- Send special keys: `$("input") << Keys.chord(Keys.CONTROL, "c")`
- Get value of input: `$("input").value()`

---
# Select Control

``` html
<select id="artist">
    <option value="1">Jethro Tull</option>
    <option value="2">Abba</option>
</select>
```

``` groovy
$("artist") = 1
```

---

# Checkbox

``` html
<form>
    <label>Search this site</label>
    <input type="radio" id="site-current" name="site" value="current">
    <label>Search Google</label><input type="radio" name="site" value="google">
</form>
```

``` groovy
$("#site-current") = "google"
```
---

# Radio buttons
``` html
<form>
    <input type="checkbox" name="pet" value="dog" />
</form>
```

``` groovy
$("form").pet = true
```

---

# Text input

``` html
<form>
    <input type="text" name="language"/>
    <input type="text" name="description"/>
</form>
```

``` groovy
$("form").language = "groovy"
$("form").description = "dynamic lang"
```
---

# Page
- Page usually defines url to be used for navigation
- Provides `at` checker that defines when browser has reached page
- Browser `to()` and `via()` navigate to page instance
- Browser instance holds reference to current page

---

# Page Example

``` groovy
class SignupPage extends Page {
    static url = "signup"

    static at = {
        $("h1").text() == "Signup Page"
    }
}

Browser.drive {
    to SignupPage
}
```

---

# Page Content DSL
- static closure property called content
- define key page elements within content closure
- use Matching functions `$()` to reference elements

``` groovy
class PageWithDiv extends Page {
    static content = {
        theDiv { $('div', id: 'a') }
    }
}
```

---

# Page `at` verification
- Page can define a check that the browser is actually at page.
- Static `at` closure - returns true if at page, false if not

``` groovy
class PageWithAtChecker extends Page {
    static at = { $("h1").text() == "Example" }
}
```
---

# Page Url
- Page can define url via static url property.
- This url will be used when navigating `to(PageClass)`


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
// Location where Geb saves the screenshots and HTML dumps at the end of each test
reportsDir = 'build/test-reports'

//implicitly wrap at call with waitFor
atCheckWaiting = true

// Run tests in Firefox by default
driver = {  
  FirefoxDriverManager.getInstance().setup()

  new FirefoxDriver()
}
```

---

# build.gradle

``` java
dependencies {
  testCompile "org.gebish:geb-spock:1.1.1"
	testCompile "org.seleniumhq.selenium:selenium-firefox-driver:3.3.1"
	testCompile "org.seleniumhq.selenium:selenium-chrome-driver:3.3.2"
	testCompile "org.seleniumhq.selenium:selenium-support:3.3.1"

  // Automatic Selenium driver manager
	testCompile("io.github.bonigarcia:webdrivermanager:1.5.0") {
		exclude group: 'org.seleniumhq.selenium'
	}
}
```

---

# Functional Test Setup
- Functional tests will likely require data to complete
- Each functional test should setup their own data
- Functional tests execute outside the Spring Boot app

---
# Exmaple Get Article Detail Page
``` groovy

class ArticleDetailPage extends Page {

  static at = {
    title == 'articel detail'
  }

  static content = {
    id { $("#id") }
    name { $("#name") }
  }

}
```

---
# Example Geb Functional Test

``` groovy
class ArticleDetailFunctionalSpec extends BaseGebSpec {
  // setup code...  
  def "view Article"() {
    when:
    ArticleDetailPage expected = to new ArticleDetailPage(categoryPathParam: "candy", timePathParam: "8", idPathParam: "123")

    then:
    expected.articleAbstract == "very abstract"
    expected.articleTitle == "foo"
    expected.articleByline == "candy"
    expected.articleImage.attr("src").endsWith("baz.png")
    expected.articleImage.attr("alt") == "wahoo!"
  }
}
```

---
# Spring Boot / Angular Geb Tests
- Launch the Spring Boot server first
- Runs all tests in tests/functional
- Use `@SpringBootTest` annotation to start server
- Extend `GebReportingSpec`
- Configure Geb browser base Url to point at running server

---
# More on Geb
- Interact with JavaScript
- Interact with HTML form elements
- Test file downloading
- Much more:
  - [http://www.gebish.org/manual/current/index.html](http://www.gebish.org/manual/current/index.html)
  - [http://www.gebish.org/manual/current/testing.html](http://www.gebish.org/manual/current/testing.html)
