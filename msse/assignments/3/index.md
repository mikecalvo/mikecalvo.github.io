---
title: Assignment #3
layout: default
---

# Assignment #3

### playlist

### Due Date: 4/14/2017

# Assignment Overview
In this assignment you will build on the domain and controller project started in assignments 1 and 2 to create a simple single page app user interface using AngularJS.
All requirements should be verified using Geb functional tests against a running Spring Boot application.
All components should validate Angular Dependency injection wiring and initialization using Jasmine.

Feel free to use your creativity to build a usable and attractive web UI.  However, points will not be subtracted for 'ugly but functional implementations'.


# Search by Artist, Album or Song
- S1: Search screen allows users to select what to search for (Artist, Album or Song)
- S2: Query against Spotify search API by selected type and search term.
  - [Spotify API Reference](https://developer.spotify.com/web-api/search-item/)
- S3: Search results are displayed as list
- S4: Search results can be paged through
- S5: Unit test all search page component methods using Jasmine

# Search by Song
- SS1: Search results display each song as artist name, album name and song title
  - [Search by Track](https://api.spotify.com/v1/search?q=abba&type=track)
- SS2: Clicking an individual song takes you to the Album details its a part of.

# Search by Artist Results
- SA1: Search results display each artist as Artist name, and related image
- SA2: Clicking an individual artist routes to page that displays Artist name, image, genres and number of followers

# Search Results by Album
- Album1: Search results list display each album as Artist name, Album name and album cover
- Album2: Clicking an individual album display Artist Name, Album Name, Album and cover and track listings, and release year
- Album3: Track listings should display track number, song title, and duration in minutes

