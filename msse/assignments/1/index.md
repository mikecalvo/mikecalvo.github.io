---
title: Assignment #1
layout: default
---

# Assignment #1
## twtr
### Due Date: 2/12/2016

---

# Introduction
- Implement a social media platform similar to Twitter
- Implement requirements over the course of 4 assignments

---
# Deliverables
1. Access to source code repository (preferrably git)
1. Working Grails project on which I can run without error or failure:
  `grails test-app`
1. All requirements implemented and tested with passing tests

---

# System description
Create an application that works like a simplified Twitter.  The initial
requirements will include storage of data to the model required to support
accounts, messages, and account following.

Messages are posted by accounts.  An account can follow another account.

---

# Account Requirements
A1. Saving an account with a valid email, password and name will succeed (unit test)
A2. Saving an account missing any of the required values of email, password and name will fail (data-driven unit test)
A3. Saving an account with an invalid password will fail.  Passwords must be 8-16 characters and have at least 1 number, at least one lower-case letter, at least 1 upper-case letter (data-driven unit test)
A4. Saving account with a non-unique email address must fail (integration test)

---
# Message Requirements
M1. Saving a message with a valid account and message text will succeed (unit test)
M2. Message text is required to be non-blank and 40 characters or less (data-driven unit test)

---

# Follow Requirements
You may choose to implement the follow functionality how you choose.  Prove that you can save data with your domain model that supports the following requirements:
F1. An account may have multiple followers (integration test)
F2. Two accounts may follow each other (integration test)
