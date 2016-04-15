---
title: Assignment #4
layout: default
---

# Assignment #4

### twtr

### Due Date: 5/7/2016

---

# Assignment Overview
In this assignment you will complete the twtr messaging application.  All requirements should be verified using Geb functional tests against a running Grails application.

Feel free to use your creativity to build a usable and attractive web UI.  However, points will not be subtracted for 'ugly but functional implementations'.

---

# Requirements
The following screens/features must be implemented within a single page application using AngularJS:

- R1. Use a alert control from the Angular UI library to display an info message saying 'Message Posted!'.
- R2. Use Angular validation to validate a message prior to posting it to the server via the REST API (client side validation).
- R3. Write a Jasmine test to validate the Angular controller for the feed page.  Use the $httpBackend functionality to mock calls to the server.
- R4. Create an Angular directive to display the follow button or "Following User" indicator on another user's account page. Validate this control with either a functional test or a Jasmine test.
- R5. Use the AngularJS date filter to format the date of a message in the feed in this style: `Mar 16`.  Validate with a functional test.
