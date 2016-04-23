---
title: GORM Examples
layout: default
---

# Example Domain classes

``` groovy

class Artist {
  String name
}

class Song {

  String title
  String lyrics
  Long releaseYear

  static belongsTo = [artist: Artist]

  static constraints = {
    lyrics nullable: true
    releaseYear nullable: true
  }
}
```

# Example Domain Unit Test

``` groovy

import grails.test.mixin.TestFor
import grails.test.mixin.TestMixin
import grails.test.mixin.domain.DomainClassUnitTestMixin
import spock.lang.Specification

@TestFor(Song)
@TestMixin(DomainClassUnitTestMixin)
class SongSpec extends Specification {

  def 'saves a song when required fields are specified'() {
    given:
    def radiohead = new Artist(name: 'Radiohead')
    def song = new Song(title: 'Creep', artist: radiohead, releaseYear: 1993)

    when:
    song.save()

    then:
    song.id
    !song.hasErrors()
  }

  def 'fails to save when required fields are missing'() {
    given:
    def radiohead = new Artist(name: 'Radiohead')
    def song = new Song(artist: radiohead)

    when:
    song.save()

    then:
    !song.id
    song.hasErrors()
    song.errors.getFieldError('title')
  }
}

```
