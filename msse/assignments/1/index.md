---
title: Assignment #1
layout: default
---

# Assignment #1

## playlist

### Due Date: 2/10/2017

# Assignment Overview
- Implement the domain model for a music playlist system
- Use JPA and SpringBoot
- Use Spock to test persistence requirements


# Deliverables
1. Access to source code repository
1. Working SpringBoot project should build with passing tests:
  `./gradlew clean build`
1. All requirements implemented and tested with passing tests

# System Description
Create an application that allows a user to create a playlist of songs.  The songs will be saved in the database along with their release and artist information.

# Chicken Feet Diagram
`Account -< Playlist >-< Songs >- Release >- Artist`

# Recommended Implementation
Use JPA annotations to define your domain model on Groovy classes.  Use the JPA repository functionality in Spring Data to interact with the data store.

# Testing Requirements
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
- A1. Accounts with valid required properties will save: email, password, name
- A2. Saving an account missing any of the required values of  email, password and name will fail (data-driven unit test)
- A3. Saving an account with an invalid password will fail.  Passwords must be 8-16 characters and have at least 1 number, at least one lower-case letter, at least 1 upper-case letter (data-driven unit test)
- A4. Saving account with a non-unique email or handle address must fail (integration test)
- A5. Password must be saved as encrypted data - not plain text

# Playlist Requirements
- P1. A valid playlist requires a name and related account
- P2. A playlist is related to an ordered list of songs
- P3. A song can be included in multiple playlists

# Song Requirements
- S1. Song require a title and a Release to be saved
- S2. Release requires a title, related artist and a release type (single, album, compilation)
- S3. Release can have an optional date field
- S4. Artist can be saved with a required name field
- S5. An artist can be related to many releases
- S6. Search for song by title with wildcard (`*Love*`)
