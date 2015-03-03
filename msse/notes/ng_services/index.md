---
title: Web Application Development
layout: default
---

# Angular Services and Factories
## Mike Calvo
## mike@citronellasoftware.com

---
# What are Services?
- Reusable components
- Cross-cutting concerns (not controller or directive logic)
- How to share code between controllers, directives or filters
- Examples: logging, security, networking

---
# Built-in Services
- Services that start with $ are a part of angular
  Examples: $http, $window

---
# Registering Services
- Services can be registered as
  - Factory
  - Constructor

---
# Service Factory
- Register a service by name and the factory method to create the service
- The service is a singleton and will be lazy created
  `angular.module(name).factory(name, $getFn)`
- Example:

``` javascript
angular.module('app').factory('Song', function($resource) {
  return $resource('songs/:id', {}, {create: {method:'PUT'}});
});
```

---
# Using the service
- Inject the service into components the same way you inject built-in angular services:

``` javascript
angular.module('app').controller('SongController', function($scope, Song) {
  $scope.songs = Song.query();
});
```
---
# Service Constructor
- Register a service as a function to be called with 'new' keyword
- Creates a separate instance each time it is used
- Use this approach if you have state data in the service that needs to be preserved on a per-instance basis
`angular.module(name).service(name, constructor)`;

---
# Service Constructor Example

``` javascript
angular.module('app').service('ConfirmDialog', function($dialog) {
  return function(message, title) {
    this.modalInstance = $modal.open({
      templateUrl: 'templates/dialogs/confirmDialog.html',
      size: 'lg',
      controller: 'ConfirmDialogController',
      resolve: {
        message: function () {
          return message;
        },
        title: function () {
          return title;
        }
      }
    });
  };
});
```

---
# Using the Constructor-based Service

``` javascript
angular.module('app').controller('SongController', function($scope, ConfirmDialog) {

  $scope.deleteSong = function(artist) {
    ConfirmDialog('Are you sure you want to delete the song', 'Confirm Remove Song')
      .modalInstance.result.then(function() {
        artist.$delete({id: artist.id});
      });
  }
});
```

---
# Other Methods of Creating Dependencies
- value: inject a commonly used value (can be overridden
  `angular.module(name).value('states', ['FL', 'GA'])`
- constant: inject a constant value (cannot be overridden)
  `angular.module(name).constant('PI', 3.14)`
- provider: allow for customizations of your service

---
# Provider Example

``` javascript
angular.module('app').provider('logService', function() {
  var allowed = {
    debug: ['debug', 'info', 'error'],
    info: [ 'info', 'error' ],
    error: [ 'error' ]
  };
  var logLevel = 'info';

  return {
    setLevel: function(setting) {
      logLevel = setting;
    },
    $get: function() {
      return {
        log: function(level, message) {
          if (allowed[logLevel].indexOf(level) != -1) {
            console.log(level+': '+message);
          }
        }
      }
    }
  }
});
```

---
# Configuring the logService

``` javascript
angular.module('app')
  .config(function(logServiceProvider) {
    logServiceProvider.setLevel('error');
  })
  .controller('MenuController', function(logService) {
    logService.log('info', 'Started'); // Won't be logged
    logService.log('error', 'Something!!'); // Logged
  });
```

---
# Example Built-in Services
$anchorScroll - scroll browser window
$animate - animate content transitions
$compile - process HTML fragment with directives
$document - DOM window.document access
$exceptionHandler - handle exceptions in the app
$http - AJAX
$q - Create your own promises
$resource - REST access

---
# More Built-in Services
$rootElement - top level DOM element
$rootScope - top level scope
$route - change the view
$routeParams - parameters to the view
$swipe - swipe gestures
$timeout - enhanced setTimeout function
$window - DOM window object

---
# Adding a Service to Muzic
- Add a confirm dialog service that can be reused by 2 controllers to produce a confirmation dialog
