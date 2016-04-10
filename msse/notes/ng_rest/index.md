---
title: Web Application Development
layout: default
---

# Angular Ajax and REST Support

## Mike Calvo

## mike@citronellasoftware.com

---

# Promises
- Pattern commonly used in JavaScript for callbacks
- Invoke a function passing it a success and optionally an error function to call when complete
- Functionality will happen in the future
- Call returns immediately but callback called in the future

---

# Promise Example

``` javascript
  doSomethingThatTakesAwhile().then(function(result) {
    console.log('Yeah!')
    }, function(error) {
      console.log('Boo!')
    });
```

---

# AJAX
- Making calls from Javascript back to server to retrieve data
- Data used to be all XML now is all JSON
- It happens behind the scenes - the url of the page doesn't change
- Foundation of modern web applications

---

# $http
- Core angular service that provides http request access to Angular app
- Used to make AJAX calls
- Inject into controllers, filters, services, etc
- Methods for making any type of HTTP request (get, post, put, etc)
- Default request goes back relative to the originating server

---

# $http Methods
- get(url, config)
- post(url, config)
- delete(url, config)
- head(url, config)
- jsonp(url, config)

---

# $http Config Options
- data: data sent with request (for example on POST)
- headers: HTTP request headers
- params: HTTP request parameter
- timeout: request timeout

---

# $http Promises
- All $http methods return a promise that has three functions:
- success(fn)
- error(fn)
- then(fn, fn)
- Data returned from the server is passed as the argument to the fn specified

---

# Examples: $http

``` javascript
$http.get('static/data/songs.json').success(function(response) {
  if (response.status === 200) {
    $scope.contentType = response.headers('content-type')
    $scope.songs = response.data;
  }
});
```

---

# Setting Defaults
- All HTTP request traffic can be configured to have common properties
- Security settings
- Transforming common data (dates for example)
- Custom headers used by your system
- Use the $httpProvider to configure defaults

---

# Example Default HTTP Settings

``` javascript
angular.module('app').config(function($httpProvider) {
  $httpProvider.defaults.transformResponse.push(function(data, headers) {
    // Remove the MongoDB id from any result returned from the server
    if (data._id) {
      delete data._id;
    }
    return data;
  })
});
```

---

# Interceptors
- Similar to filters in a Java Web Server
- Intercept all requests and responses
- Examples:
  - Provide default config values
  - Log traffic statistics to console
  - Setting breakpoints
  - Security tokens

---

# Interceptor Example

``` javascript
angular.module('app').config(function($httpProvider) {
  $httpProvider.interceptors.push(function() {
    return {
      request: function(config) {
        config.headers['REQUEST_TIME'] = new Date();
      },
      response: function(response) {
        console.log('Data count: '+response.data.length)
        return response
      }
    }
  })
})
```

---

# Connecting to REST Services
- $resource factory provides easy access a RESTful resource
- Provide $resource the URL path to the resource: `$resource('songs/:id')`
- Returns an object providing methods to perform CRUD operations on the resource using REST
- Simplifies access to server with less JavaScript code

---

# Resource Methods

``` javascript
var Song = $resource('songs/:id')
Song.get({id:33}); // returns object returned from of GET at songs/33
Song.query(); // returns array returned from GET at songs/
Song.remove({id:22}) // sends a DELETE to songs/22
Song.delete({id:44}) // sends a DELETE to songs/22
Song.save({title: 'Loser', artist: {id: 3, name: 'Beck'}}) // POST
```

---

# Resource Response
- Returns object that contains a promise
- When fulfilled the object returned is automatically populated with the response

``` javascript
$scope.song = Song.get({id: 3})
$scope.song.$promise.then(function(result) {
  $scope.songLoadComplete = true;
});
```

---
# Additional Resource Actions
- Custom resource methods can be added when creating the resource:

``` javascript
var Song = $resource('songs/:id', {}, {
  create: {method: 'PUT'}
  });
```

---

# Resource Action Configuration
- method: HTTP method
- params: segment variables for call
- url: override default url
- isArray: by default resources assume single values

---

# Custom Action Examples

``` javascript
$resource("songs/:id", {}, {
  create: {method: "POST"},
  save: {method: "PUT"},
  find: {
    method: "GET",
    isArray: true,
    url:"songs/?sort=:sort&offset=:offset",
    params:{offset:0}},
});
```

---

# Resource Instances
- After retrieving resource instances, crud methods can be called directly on the instance
- Similar to GORM style:

``` javascript

$scope.song = $resource('songs/:id', {id: $routeParams.id});
$scope.save = function() {
  $scope.song.$save(); // pushes changes to the server
}
```
---

# Views Backed by Resources
- Most built-in directives know how to properly respond to resources
- For example:
  - ng-repeat will automatically refresh itself when the resource is resolved

---

# Resource Conventions
- Define a resource once and assign it to an upper case variable name
- Gives it a calling semantic similar to GORM domain class

``` javascript
var Song = $resource('songs/', {})
$scope.songs = Song.query(); // gets a collection of songs
$scope.song = Song.get({id: routeParams.id}); // gets instance
```

---

# Advantages of Resources
- Simplified server access (pre-defined methods)
- Follows REST pattern
- Keeps client instances in sync with server responses automatically

---

# Adding ng-resource Module
- Install with bower:
`node_modules/.bin/bower install angular-resource --save`

- Add reference to application.js comment directive:
`//= require angular-resource/angular-resource`

- Add the module to your app module creation:
`angular.module('app', ['ngRoute', 'ngResource', 'ui.bootstrap']);`

---

# Angular Resource + Grails Resources
- Angular and Grails play well here
- Use both to reduce complexity on both client and server

---

# Muzic: Use Angular Resource
- Connect an Angular resource to the Grails Artist REST resouce
1. Add ngResource to the project
2. Create a view for listing artists
3. Support add and remove for artists

---

# Summary
- Angular includes easy access to server data via $http service
- Advanced RESTful resource access available via the ngResource module
