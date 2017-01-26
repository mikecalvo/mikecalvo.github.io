---
title: Web Application Development
layout: default
---
theme: Next, 3

## Testing Overview
### MSSE 2017

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

# Testing strategies
- TLD (Test last development)
- TDD (Test driven development)
- BDD (Behavior driven development)
- Most important feature to have quality code coverage

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

# JaCoCo

- Popular test coverage tool
- Provides coverage reports on every build (build/reports/jacoco)

Add to build.gradle:

```groovy
apply plugin: 'jacoco'

check.dependsOn jacocoTestReport
```

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
- Assume environment has been setup externally
- Test setup should put system into proper state
