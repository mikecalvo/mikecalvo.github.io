---
title: Assignment #3
layout: default
---

# Assignment #3
### twtr
### Due Date: 4/9/2016

# Assignment Overview
- Create an AngularJS single-page app for the twtr application
- Use Geb to implement functional tests in a browser for the implemented UI features

# Login Requirements
- L1: When not logged in, route user to the login screen
- L2: Login screen allows a user to enter username and password to gain access
- L3: Invalid login will be rejected with an error message

# Search Requirements
(Note: these requirements are allowed once a user logs in)
- S1: Provide a search box for finding messages by message poster and message contents
- S2: Display matching results in a scrollable area below the search box
- S3: Search result messages will display the message contents as well as the posting user.
- S4: Clicking on the posting user's name in a message will route to the user's detail page

# User Detail Requirements
- U1: User's detail page will display the user's name as well as a scrollable list of that user's postings
- U2: User's detail page will provide a way for the logged in user to follow the detail user
- U3: When the logged in user is following the detail user, the detail page will display a message or icon indicating this
- U4: When the logged in user goes to their own detail page, they can edit their name and email

# Navigation Requirements
Provide a way (links, buttons, etc.) for the logged in user to navigate between the following screens:
N1: Logged in user's detail page
N2: Search box
