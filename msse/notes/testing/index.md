footer: © Citronella Software Ltd 2015
slidenumbers: true

# Testing Overview
## Mike Calvo
### mike@citronellasoftware.com

---

# Basic Concept
- Write code to verify system
- Automatable
- Multiple levels:
  - Unit
  - Integration
  - Functional
  - Smoke/End-to-end

---

# Rationale
- Code not done until tested
- Find mistakes sooner
- Refactor without regression
- Can help improve design
- Many, many more...

---

# Test-Driven Development
- Write a test before you write code
  - Test fails until the code works
- Pairing
  - Take turns writing test/code

---

# Mocks
- Help isolate code under test
- Replace system component or external system with a fake
- Provide expected responses or exceptions
- Can be used to verify call sequences

---

# Unit Testing
- Tests at the class or method level
- External system components are mocked out
- Fastest to run

---

# Integration Testing
- Purpose: Tests the interaction of system components
- Can often include testing of
  - Data Access
  - Dependency injection

---

# Functional Testing
- Purpose: Test a complete functional unit of a system
- System is deployed and invoked in a running environment
- Mock out external systems

---

# Smoke Testing
- Purpose: major functions of the system are operational
- Tests end-to-end functionality
  - Including integrations with external systems
- Can be used to verify a deployment

---

# Code Coverage
- Metrics on how much of your code is covered by your tests
- Tools can instrument your code to monitor line coverage
- Intellij provides it
- Many opinons on good coverage percentage

---

# Running tests
- Build tools are used to run tests in bulk
- Typically run in order of specificity
  - No point running smoke tests if a unit test fails
- Grails works this way

---

# Unit Testing Frameworks
- Provide test and assertion classes
- Test run harness
- Provide pass/failure reporting functionality
- Exist for all platforms
  - Objective C, .NET, Java, Javascript, etc

---

# JUnit
- Created by Kent Beck (original founder of Agile)
- Oldest, most-widely used Java unit test framework
- Uses approach of annotating classes and methods to mark as tests
- Great integration with most Java IDEs

---

# Do's
- Cover edge cases and pathological situations
- When a bug is found, write a test that exposes it
- Cleanup system (data, files) after test completes
- Tests should pass if run in any order
- Favor black-box tests
- Always have functional tests

---

# Don'ts
- Use print statements in tests as verification
- Ignore tests that fail intermittently
- Assume enviornment has been setup externally
  - Test setup should put system into proper state

---

# Grails Testing
- Grails supports 4 phases of testing
- Supports many test frameworks: JUnit, TestNG, Spock
- Executed using the grails command
  `grails test-app`
- HTML report produced in target/test-results
- Organized by type in the project structure
- Generates tests automatically

---

# Grails Unit Tests
- Simple tests that do not provide any of the Grails infrastructure at runtime
- Test a single class or method
- Mixins provide support for auto-wiring and mocking
- Run quickly

---

# Grails Mixins
- @TestFor
  - Defines the class under test
  - Automatically injects a member of this type
- @Mock
  - Defines mocked classes used by test
  - Automatically injects the mocks into test

---

# TestFor
- Provides convenient injections for controllers, services and domain classes
- Example members added: @TestFor(SomeControllerClass)
  - controller - instance of the SomeControllerClass
  - params - a map of parameters sent to controller methods
  - request - mock HTTP request with
  - response - mock HTTP response with result of controller

---

# More Grails Mixins
- @GrailsUnitTestMixin
    - doWithSpring
    - doWithConfig
- @FreshRuntime
  - Clean app and Grails context for before each test

---

# Even More Grails Mixins
- @DirtiesRuntime
  - A test will modify the runtime, will be cleaned after
- @SharedRuntime
  - Marks tests that can share a common runtime

---

# Grails Integration Tests
- GORM is initialized
  - Data source specified in test config is used
- Services and Controllers are wired together
- Only thing missing is HTTP response
- These take longer but get more coverage

---

# Grails Functional Tests
- Interact with the app via HTTP
- Typically used to test the front door of the application
  - UI or REST API

---

# Final Grails Testing Tips
- Grails Interactive Console
  - Makes interactive test debugging faster
- Complete documentation at grails.org
  - http://grails.org/doc/latest/guide/testing.html
