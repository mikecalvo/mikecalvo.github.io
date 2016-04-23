---
title: Web Application Development
layout: default
---

# Jasmine and Karma
## Mike Calvo
## mike@citronellasoftware.com

---
# Unit Testing Angular Code
- Geb is great for Functional testing
- How do we unit test Angular components:
  - Jasmine
  - Karma

---
# Jasmine
- Behavior-driven testing framework
- Plays role similar to JUnit or Spock
- Intended to read like assertive statements
- Can be run outside of browser

---
# Core Jasmine Concepts
- Suite
- Spec
- Expectations

---
# Suite
- Wraps a collection of tests
- Similar to Spock or JUnit class
- Defined by `describe` function

---
# Spec
- A test
- Similar to Spock or JUnit method
- Defined by `it` function

---
# Expectations
- Creates an assertion you want your test to make
- Similar to assert in JUnit or then clauses in Spock
- `expect()` takes a value and returns a chain which can be matched against specific things

---
# Example Expectations

``` javascript
expect(value).toEqual(expected);
expect(value).not.toBe(4);
expect('Hello World').toMatch(/lo /);
expect(value).not.toBeUndefined();
expect(something).toBeTruthy();
```
---
# Example Jasmine Test:

``` javascript
describe("A suite", function() {
  it("contains spec with an expectation", function() {
    expect(true).toBe(true);
  })
});
```

---
# Grouping and Nesting Suites

``` javascript
describe('my component', function() {
  describe('does these things', function() {
    it('does a', function() {});
    it('does b', function() {});
  });

  describe('does other things too', function() {
    it('does c', function() {});
    it('does d', function() {});
  });
});
```

---
# Setup and Teardown
- `beforeEach` and `afterEach` perform setup and cleanup functions that run around each spec within a suite
- `beforeAll` and `afterAll` perform setup and cleanup around all the specs within a suite

---
# Spies
- Tracks when functions have been called to ensure proper sequence of functions happens
- Similar to mocks
- Introduces an expect matchers `toHaveBeenCalled` and `toHaveBeenCalledWith`

---
# Spies Example

```javascript
describe("A spy", function() {
  var foo, bar = null;

  beforeEach(function() {
    foo = {
      setBar: function(value) {
        bar = value;
      }
    };

    spyOn(foo, 'setBar');

    foo.setBar(456, 'another param');
  });

  it("tracks that the spy was called", function() {
    expect(foo.setBar).toHaveBeenCalled();
  });

  it("tracks all the arguments of its calls", function() {
    expect(foo.setBar).toHaveBeenCalledWith(456, 'another param');
  });
});
```

---
# Spy Responses
- Spies return a chain which allow modifying what happens when spied function is called
- callThrough: call original function
- and.returnValue
- and.callFake: call replacement function
- and.throwError
- and.stub: do nothing

---
# Spy Call Checks
- Track the number of times a function has been called
- Track the arguments passed to the function
- Look at a specific call invocation (all(), mostRecent(), first())

---
# Advanced Matching
- jasmine.any: takes a type to match on:
- jasmine.anything(): returns true for anything other than null or undefined
- jasmine.objectContaining(): returns true if the specified properties are matched
- jasmine.arrayContaining(): returns true if an array contains specified values

---
# Advanced Matching Examples

``` javascript
expect({}).toEqual(jasmine.any(Object));
expect({}).toEqual(jasmine.anything());
expect(1).toEqual(jasmine.any(Number));
expect({name: 'Mike', age: 21}).toEqual(jasmine.objectContaining({age:21}));
expect([1,2,3,4]).toEqual(jasmine.arrayContaining([1,4]));
```

---
# Karma
- JavasScript test runner
- Used to execute JavaScript normally run within a page in standalone manner
- Works with Jasmine
- Integrates tests with an executing browser
- Runs JavaScript unit tests
- Generates test result reports

---
# Karma Install
- Karma is an npm module
- Add to package.json
- Also install karma-jasmin plugin and browser launchers

---
# package.json Example

``` json
"devDependencies": {
  "jasmine-core": "^2.2.0",
  "karma": "^0.12.31",
  "karma-mocha-reporter": "^1.0.0",
  "karma-ng-scenario": ">=0.1",
  "karma-remote-reporter": "0.1.5",
  "karma-chrome-launcher": "*",
  "karma-firefox-launcher": "*",
  "karma-phantomjs-launcher": "^0.1.4",
  "karma-jasmine": "^0.3.0",
  "karma-junit-reporter": "*",
  "karma-coverage": "^0.2.7"
}
```

---
# Karma Config File
- Browser configuration
- Script file locations
  - Must load all scripts required to run tests including bower dependencies
- Karma Plugins
- Logging, coloring, code-coverage

---
# Karma Config Note
- Be sure to put your test file references
AFTER your bower dependencies and your app file references

---
# Example Karma Config

``` javascript
module.exports = function (config) {
  config.set({

    basePath: '../',

    files: [
      'node_modules/jasmine-core/lib/jasmine-core/jasmine.js',
      'node_modules/karma-jasmine/lib/adapter.js',
      'grails-app/assets/bower_components/jquery/dist/jquery.js',
      'grails-app/assets/bower_components/bootstrap/dist/js/bootstrap.js',
      'grails-app/assets/bower_components/angular/angular.js',
      'grails-app/assets/bower_components/angular-bootstrap/ui-bootstrap-tpls.js',
      'grails-app/assets/bower_components/angular-mocks/angular-mocks.js',
      'grails-app/assets/bower_components/angular-resource/angular-resource.js',
      'grails-app/assets/bower_components/angular-route/angular-route.js',
      'grails-app/assets/bower_components/jasmine-data_driven_tests/src/all.js',
      'grails-app/assets/javascripts/application.js',
      'grails-app/assets/javascripts/config/**/*.js',
      'grails-app/assets/javascripts/controllers/**/*.js',
      'grails-app/assets/javascripts/services/**/*.js',
      'test/karma/*.js'
    ]
}
```

---
# Test Watcher
- Karma can be configured to run in the background and watch when your app or test code changes to re-run your tests
- Tests run very quickly
- Effective way to get immediate feedback when you've broken something
- Enable in the config file

---
# Test Watcher Config

``` javascript
config.set({
  basePath: '../',
  files: [ /* put your file references in order here...*/],
  reporters: ['remote'],
  frameworks: ['jasmine'],
  autoWatch: true,
  singleRun: false,
  remoteReporter: {
    host: 'localhost',
    port: '9877'
  },
});
```

---
# Integrating Karma and Grails
- Install Karma test runner grails plugin:

In Build.Config:
`test ":karma-test-runner:0.2.2"
`

- Create a new unit test spec that launches the karma tests

---
# Add a JavaScript Test Spock Spec

``` groovy
package muzic

import de.is24.util.karmatestrunner.junit.KarmaTestSuiteRunner
import org.junit.runner.RunWith

@RunWith(KarmaTestSuiteRunner)
@KarmaTestSuiteRunner.KarmaProcessName('node_modules/karma/bin/karma')
@KarmaTestSuiteRunner.KarmaConfigPath('./test/karma.conf.js')
@KarmaTestSuiteRunner.KarmaRemoteServerPort(9877)
class JavaScriptUnitTestKarmaSuite {
}
```

---
# ng-mock
- Testing angular code is simplified by using ng-mock
- Allows for mocking of backend calls (replacing $http)
- Allows for overriding dependency injection
- Install using bower - only referenced within karma config file (not app)

---
# $httpBackend
- ng-mock provides a mocked server backend for your tests to use to verify calls to the server and provide responses
- all server calls are replaced by calls to $httpBacked
- Tests can define expected calls:

``` javascript
$httpBackend.expectGET('play/').respond(200, plays);
// do something to make this call happen
$httpBackend.flush();
```

---
# data-driven tests
- Jasmine, unlike Spock, does not support data-driven tests directly
- Add plugin for this (Jasmine Data-Driven Tests) via bower

---
# bower.json

``` json
{
  "name": "muzic",
  "version": "0.0.0",
  "homepage": "https://github.com/mikecalvo/muzic",
  "authors": [
    "Mike Calvo <mike@citronellasoftware.com>"
  ],
  "license": "MIT",
  "private": true,
  "dependencies": {
    "jquery": "~2.1.3",
    "angular": "~1.3.13",
    "bootstrap": "~3.3.2",
    "angular-bootstrap": "~0.12.0",
    "angular-mocks": "~1.3.15",
    "angular-route": "~1.3.14",
    "angular-resource": "~1.3.14"
  },
  "devDependencies": {
    "jasmine-data_driven_tests": "~1.0"
  },
  "resolutions": {
    "angular": "~1.3.13"
  }
}
```

---
# Writing an Angular Jasmine Test
- Must initialize the module
- Usually define a beforeEach to inject dependencies required for your component being tested

``` javascript
beforeEach(module('app'));

beforeEach(inject(function ($injector) {
  $controller = $injector.get('$controller');
  $httpBackend = $injector.get('$httpBackend');
  $scope = $injector.get('$rootScope').$new();
}));
```

---
# Example Angular Jasmine Test

``` javascript
describe('SongPlaysController', function () {

  var $controller, $httpBackend, $scope;
  var now = new Date();
  var plays = [
    {song: {title: 'Loser', artist: {name: 'Beck'}}, timestamp: now},
    {song: {title: 'Enjoy The Silence', artist: {name: 'Depeche Mode'}}, timestamp: now}];

  beforeEach(module('app'));

  beforeEach(inject(function ($injector) {
    $controller = $injector.get('$controller');
    $httpBackend = $injector.get('$httpBackend');
    $scope = $injector.get('$rootScope').$new();
  }));

  describe('initializes scope', function () {

    it('fetches the play data into scope', function () {
      $httpBackend.expectGET('play/').respond(200, plays);
      $controller('SongPlaysController', {$scope: $scope});
      $httpBackend.flush();

      expect($scope.plays).toEqual(plays);
    });

  });

  afterEach(function () {
    $httpBackend.verifyNoOutstandingExpectation();
    $httpBackend.verifyNoOutstandingRequest();
  });
});
```
