---
title: Assignment #1
layout: default
---

# Assignment #1
## twtr
### Due Date: 2/12/2016

# Assignment Overview
- Implement a social media platform similar to Twitter
- Implement the domain model and persistence of the model using Grails Domain classes
- Write Grails unit and integration tests to verify that you've implemented the requirements

# Deliverables
1. Access to source code repository (preferrably git)
1. Working Grails project on which I can run without error or failure:
  `grails test-app`
1. All requirements implemented and tested with passing tests

# System Description
Create an application that works like a simplified Twitter.  The initial
requirements will include storage of data to the model required to support
accounts, messages, and account following.

Messages are posted by accounts.  An account can follow another account.

# Testing Requirements
For each requirement below, the type of required test is specified.  Unit tests are Grails unit tests (they do not require Grails to be running).  These should use the DomainClassUnitTestMixin annotation to provide mocking of Grails-provided domain methods.  Integration tests are Grails integration tests that require the Grails application to be running.  These will interact with the running database instance within your Grails app.

Some requirements need to be verified using data-driven tests.  These should be Spock tests that use the `where` directive.  For example:

```
def 'test adding: #description'() {
  when:
  def result = a + b

  then:
  result == expected

  where:
  description            | a  | b | expected
  'Two positive numbers' | 4  | 6 | 10
  'A negative number'    | -1 | 3 | 2
  'Adding zero'          | 0  | 1 | 1
}
```

# Account Requirements
- A1. Saving an account with a valid handle, email, password and name will succeed (unit test)
- A2. Saving an account missing any of the required values of handle email, password and name will fail (data-driven unit test)
- A3. Saving an account with an invalid password will fail.  Passwords must be 8-16 characters and have at least 1 number, at least one lower-case letter, at least 1 upper-case letter (data-driven unit test)
- A4. Saving account with a non-unique email or handle address must fail (integration test)

# Message Requirements
- M1. Saving a message with a valid account and message text will succeed (unit test)
- M2. Message text is required to be non-blank and 40 characters or less (data-driven unit test)

# Follow Requirements
- You may choose to implement the follow functionality how you choose.  Prove that you can save data with your domain model that supports the following requirements:
- F1. An account may have multiple followers (integration test)
- F2. Two accounts may follow each other (integration test)
