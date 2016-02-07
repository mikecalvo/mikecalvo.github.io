---
title: Assignment #3
layout: default
---

# Assignment #3
### twtr
### Due Date: 4/9/2016

# Assignment Overview
In this assignment you will build on the domain and controller project started in assignments 1 and 2 to create a simple single page app user interface using AngularJS.  All requirements should be verified using Geb functional tests against a running Grails application.

Feel free to use your creativity to build a usable and attractive web UI.  However, points will not be subtracted for 'ugly but functional implementations'.

The Navigation requirements are for providing a global way (available on every page) to navigate to commonly used screens or functionality.  Think about having a global navigation header, footer or side panel on each screen (doesn't matter which you pick).  These navigation controls can be text links, images or buttons. 

# Login Requirements
- L1: When not logged in, route user to the login screen
- L2: Login screen allows a user to enter username and password to gain access
- L3: Invalid login will be rejected with an error message

# Search Requirements
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
- N1: User's detail page
- N2: Search box
- N3: Logout - clicking this should bring you to the login screen and provide a helpful message 'Sorry to see you go... etc'
