---
title: Web Application Development
layout: default
---

# Testing Angular Apps

## Marc Kapke

## kapkema@gmail.com

---
# Unit Testing
- Use Jasmine to author unit tests
 - [Jasmine Docs](https://jasmine.github.io/2.4/introduction.html)
- Use Karma to run unit tests

---
# Jasmine
- Behavior-driven testing framework
- Plays role similar to JUnit or Spock
- Intended to read like assertive statements

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
- `callThrough`: call original function
- `and.returnValue`
- `and.callFake`: call replacement function
- `and.throwError`
- `and.stub`: do nothing

---
# Spy Call Checks
- Track the number of times a function has been called
- Track the arguments passed to the function
- Look at a specific call invocation (all(), mostRecent(), first())

---
# Advanced Matching
- `jasmine.any`: takes a type to match on
- `jasmine.anything()`: returns true for anything other than null or undefined
- `jasmine.objectContaining()`: returns true if the specified properties are matched
- `jasmine.arrayContaining()`: returns true if an array contains specified values

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
# Karma Config File
- Browser configuration
- Script file locations
  - Must load all scripts required to run tests including bower dependencies
- Karma Plugins
- Logging, coloring, code-coverage

---
# Example Karma Config

``` javascript

module.exports = function (config) {
  config.set({
    basePath: '',
    frameworks: ['jasmine', '@angular/cli'],
    plugins: [
      require('karma-jasmine'),
      ...
    ],
    client:{
      clearContext: false // leave Jasmine Spec Runner output visible in browser
    },
    files: [
      { pattern: './src/test.ts', watched: false }
    ],
    preprocessors: {
      './src/test.ts': ['@angular/cli']
    },    
    reporters: config.angularCli && config.angularCli.codeCoverage
              ? ['progress', 'coverage-istanbul']
              : ['progress', 'kjhtml'],
    port: 9876,
    colors: true,
    autoWatch: true,
    browsers: ['Chrome'],
    singleRun: false
  });
};
```

---
# Test Watcher
- Karma can be configured to run in the background
- when your app or test code changes karma will re-run your tests
- Tests run very quickly
- Effective way to get immediate feedback when you've broken something
- Enable in the config file

---
# Test Watcher Config

``` javascript
config.set({
  ...
  autoWatch: true,
  singleRun: false,
  ...
});
```

---

# Angular Integration Testing Basics
- Use Jasmine and Karma
- Add Angular's testing utilities
  - `TestBed`
  - helper functions in `@angular/core/testing`
     - `async`, `fakeAsync`, `tick`, etc...

---

# TestBed
-  `TestBed` is a testing `@NgModule`
-  Specialized root module designed to run component under test

---

# `configureTestingModule()`
- Initialize TestBed in a `beforeEach`
- This will reset component before each spec in suite

``` javascript
beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [ MyComponent ]
    });
  })
```

---

# TestBed - createComponent
- Create an instance of the component under test
- Do this in beforeEach as well.
- `TestBed.createComponent(MyComponent)` returns a ComponentFixture

---

# `ComponentFixture`

- Provides access to the component itself
- Access to `DebugElement` - which is component's DOM element
- Query the `DebugElement` to access template state

---

# `detectChanges()`
- detectChanges will trigger data binding
- You must do this when you want the view to reflect model

--- 

# Example Inline Template Test

``` javascript
describe('MyComponent (inline template)', () => {

  let component:    BannerComponent;
  let fixture: ComponentFixture<BannerComponent>;
  let debugElement:      DebugElement;
  let templateElement:      HTMLElement;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [ MyComponent ], // declare the test component
    });

    fixture = TestBed.createComponent(MyComponent);

    component = fixture.componentInstance; // MyComponent test instance

    // query for the title <h1> by CSS element selector
    debugElement = fixture.debugElement.query(By.css('h1'));
    templateElement = debugElement.nativeElement;
  });

  it('should display original title', () => {
    fixture.detectChanges();
    expect(templateElement.textContent).toContain(component.title);
  });
});
```

---
