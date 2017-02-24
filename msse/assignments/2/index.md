---
title: Assignment #2
layout: default
---

# Assignment #2

## playlist

### Due Date: 3/10/2017

# Assignment Overview
- Use SpringBoot and Controllers to add a web layer to the domain model created in assignment 1 for the playlist application.

# Account Requirements
Create REST endpoints meeting:
- A1: Receives JSON data to create an Account
- A2: Return an error response from the create Account endpoint if the account values are invalid
- A3: Returns JSON data with Account values for a user based on an account id or email. (data-driven test)
- A4: Returns the playlists for an Account and is sortable and pageable
- A5: All steps must have valid functional tests
- A6: Create integration tests for Account

# Playlist Requirements
- P1: Create a REST endpoint that receives JSON data to create a playlist
- P2: Create a REST endpoint that receives JSON data to add a song to a playlist
- P3: All steps must have valid functional tests

# Song Requirements
- S1: Create a REST endpoint that searches for a songs based on title and artist name including wildcards
- S2: Create a REST endpoint that creates a song, release, and artist
- S3: Create a REST endpoint that adds a song to an existing release
- S4: All steps must have valid functional tests

# General Requirements
- G1: Create a REST endpoint that searches across Artists, Releases and Songs by wildcard.  It should return a JSON payload that includes lists of artists which match the name, releases that match the title and songs that match the title.
