---
title: GORM Examples
layout: default
---

## Example @Resource REST via Domain Class

``` groovy
@Resource(uri='/restaurants', formats = ['json', 'xml'])
class Restaurant {

  String name
  String city
  String state

  static constraints = {
    state inList: ['FL', 'IL', 'MN', 'WI']
  }
}
```

## Example RestfulController

``` groovy
package grails.rest

class ReservationController extends RestfulController<Reservation> {

  static responseFormats = ['json', 'xml']

  ReservationController() {
    super(Reservation)
  }

  @Override
  protected Reservation queryForResource(Serializable id) {
    def restaurantId = params.restaurantId
    Reservation.where {
      id == id && restaurant.id == restaurantId
    }.find()
  }
}
```

## URLMappings for nested REST paths

``` groovy
class UrlMappings {

  static mappings = {
    "/$controller/$action?/$id?(.$format)?" {
      constraints {
        // apply constraints here
      }
    }

    "/restaurants"(resources:'restaurant') {
      "/reservations"(resources:'reservation')
    }

    "/"(view: "/index")
    "500"(view: '/error')
    "404"(view: '/notFound')
  }
}
```

## REST Functional Spec

``` groovy
package grails.rest

import geb.spock.GebSpec
import grails.converters.JSON
import grails.test.mixin.integration.Integration
import groovyx.net.http.HttpResponseException
import groovyx.net.http.RESTClient
import spock.lang.Shared
import spock.lang.Stepwise

@Integration
@Stepwise
class RestaurantResourceFunctionalSpec extends GebSpec {

  @Shared
  def punchId

  RESTClient restClient

  def setup() {
    restClient = new RESTClient(baseUrl)
  }

  def 'get all restaurant resources'() {

    when:
    def resp = restClient.get(path: '/restaurants')

    then:
    resp.status == 200
    resp.data.size() == 0
  }

  def 'saves a new restaurant'() {
    given:
    def restaurant = new Restaurant(name: 'Punch', city: 'Minneapolis', state: 'MN')
    def json = restaurant as JSON

    when:
    def resp = restClient.post(path: '/restaurants', body: json as String, requestContentType: 'application/json')

    then:
    resp.status == 201
    resp.data

    when:
    punchId = resp.data.id

    then:
    punchId
    resp.data.name == 'Punch'
    resp.data.city == 'Minneapolis'
  }

  def 'gets a new restaurant'() {
    when:
    def resp = restClient.get(path: "/restaurants/${punchId}")

    then:
    resp.status == 200
    resp.data.id == punchId
    resp.data.name == 'Punch'
    resp.data.city == 'Minneapolis'
    resp.data.state == 'MN'
  }

  def 'updates an existing restaurant'() {
    given:
    def restaurant = new Restaurant(id: punchId, name: 'Punch', city: 'St. Paul', state: 'MN')
    def json = restaurant as JSON

    when:
    def resp = restClient.put(path: "/restaurants/${punchId}", body: json as String, requestContentType: 'application/json')

    then:
    resp.status == 200

    when:
    resp = restClient.get(path: "/restaurants/${punchId}")

    then:
    resp.status == 200
    resp.data.city == 'St. Paul'
  }

  def 'deletes a restaurant'() {
    when:
    def resp = restClient.delete(path: "/restaurants/${punchId}")

    then:
    resp.status == 204

    when:
    restClient.get(path: "/restaurants/${punchId}")

    then:
    thrown(HttpResponseException)

    when:
    resp = restClient.get(path: '/restaurants')

    then:
    resp.status == 200
    resp.data.size() == 0
  }
}
```
