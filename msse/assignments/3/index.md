---
title: Assignment #3
layout: default
---

# Assignment #3

### playlist

### Due Date: 4/14/2017

# Assignment Overview
In this assignment you will build on the domain and controller project started in assignments 1 and 2 to create a simple single page app user interface using AngularJS.  All requirements should be verified using Geb functional tests against a running Spring Boot application.

Feel free to use your creativity to build a usable and attractive web UI.  However, points will not be subtracted for 'ugly but functional implementations'.


# Search
- S1: Search screen allows users to query against Spotify search API for by each of the supported types
  - [Spotify API Reference](https://developer.spotify.com/web-api/search-item/)
  - [Search by Track](https://api.spotify.com/v1/search?q=abba&type=track)
- S2: Search results are displayed as list

# Search by Song
- SS1: Individual search result displays Artist name, song title and release date
  - Use Spotify API to retrieve additional required track/artist details about individual search results
  - [API call for details](https://api.spotify.com/v1/albums/0C36RlW2Fa0C7n1JnWBBMP)
- SS2: Clicking an individual song takes to the Album details its a part of.

# Search by Artist Results
- SA1: Individual Artist Search results display Artist name, and related image
- SA2: Clicking an individual artist routes to page that displays Artist name, image, genres and number of followers

# Search Results by Album
- Album1: Individual Album results display Artist name, Album name and album cover
- Album2: Clicking an individual album display Artist Name, Album Name, Album and cover and track listings, and release year
- Album3: Track listings should display track number, song title, and duration in minutes

