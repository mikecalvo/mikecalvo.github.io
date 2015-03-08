---
title: Assignment #2
layout: default
---

# Assignment #2
## sellit.com
### Due Date: 4/3/2015

---

# Introduction
- Add Security to web application
- Add RESTful web services to application
- Functional Testing

---
# Special Instructions:
1. Remove all generated tests from your project that are not implemented (have no test methods)
1. Be sure to have a working test for each requirement you are implementing
1. Functional tests means to Geb tests
1. Create at least one test that is data-driven  (uses Spock where functionality)

---

# Security Requirements
- Verify with functional tests the following requirements:
  - Protect access to adding a new listing for only logged in users
  - Create listing page will automatically set the logged in user as the seller
  - Active listing page will automatically set the logged in user as the bidder
  - Reviews are protected so that only sellers and winners can provide feedback
  - View/Edit profile page is protected by account

---

# REST API Requirements
- Verify with functional tests providing the following requirements via a RESTful API using JSON the following end points:
  - API endpoints creating or updating data must be secured
  - Get listings - default active only, support parameter for completed
  - Get bids for a listing
  - Get/Create/Edit/Delete listing
  - Get/Edit account
  - Create bid
  - Create feedback (both seller and buyer)
