---
title: Assignment #3
layout: default
---

# Assignment #3

### playlist

### Due Date: 4/8/2017

# Assignment Overview
In this assignment you will build on the domain and controller project started in assignments 1 and 2 to create a simple single page app user interface using AngularJS.  All requirements should be verified using Geb functional tests against a running Spring Boot application.

Feel free to use your creativity to build a usable and attractive web UI.  However, points will not be subtracted for 'ugly but functional implementations'.

The Navigation requirements are for providing a global way (available on every page) to navigate to commonly used screens or functionality.  Think about having a global navigation header, footer or side panel on each screen (doesn't matter which you pick).  These navigation controls can be text links, images or buttons.

# Create Account Requirements
- C1: Create account screen allows a user to enter account details
- C2: Valid account details submitted to server will create account
- C3: Invalid account details should present user with feedback on what needs correcting

# Login Requirements
- L1: When not logged in, route user to the login screen
- L2: Login screen allows a user to enter username and password to gain access
- L3: Invalid login will be rejected with an error message
- L4: Successful login, routes user to search page

# Search by Song
- S1: Search by Song screen allows users to query against Spotify search API for songs
  - [Spotify API Reference](https://developer.spotify.com/web-api/search-item/)
  - [Search by Track](https://api.spotify.com/v1/search?q=abba&type=track)
- S2: Search results are displayed as list
- S3: Individual search result displays Artist name, song title and release date
  - Use Spotify API to retrieve additional required track/artist details about individual search results
  - [API call for details](https://api.spotify.com/v1/albums/0C36RlW2Fa0C7n1JnWBBMP)
- S4: Search screen shows the currently logged in user
