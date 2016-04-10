---
title: Angular Example with Security
layout: default
---

See the example on [GitHub](https://github.com/mikecalvo/grails3-angular-with-security) for complete details.

See the examples on [REST](../rest) and [REST Functional Tests](../rest_functional_tests) to see Grails REST Basics.

See the [Grails Angular Example](../grails_angular) for setting up Grails with Bower and the Asset Pipeline

# build.gradle Updates

Enable the Gradle-Bower integration with the plugin:

``` groovy
plugins {
  id "io.spring.dependency-management" version "0.5.4.RELEASE"
  id 'com.craigburke.bower-installer' version '2.5.1'
}
```

Update the dependencies section to include the asset pipeline templates support:

``` groovy
// Angular Templates Plugin
assets "com.craigburke.angular:angular-template-asset-pipeline:2.2.7"
}
```

Add the Bower dependencies for Bootstrap, Angular, Angular Routing, and Angular Resources:

``` groovy
bower {

  'bootstrap'('3.3.x') {
    source 'dist/css/bootstrap.css' >> 'css/'
    source 'dist/css/bootstrap-theme.css' >> 'css/'
    source 'dist/fonts/**' >> 'fonts/'
    source 'dist/js/bootstrap.js'
    excludes 'jquery'
  }

  'angular'('1.5.x') {
    source 'angular.js'
    source 'angular-csp.css'
  }

  'angular-route'('1.5.x') {
    source 'angular-route.js'
  }

  'angular-resource'('1.5.x') {
    source 'angular-resource.js'
  }
}
```

# Create a SecurityService

Define an angular service to perform the login against the SpringSecurity login API and store the user.  Use the scope notification to post a message when a user authenticates:

``` javascript
angular.module('app').factory('securityService', ['$http', '$rootScope', function ($http, $rootScope) {
  var service = {};
  var currentUser;

  var loginSuccess = function (response) {
    currentUser = {
      username: response.data.username,
      roles: response.data.roles,
      token: response.data['access_token']
    };

    $rootScope.$emit('userChange', currentUser)
  };

  var loginFailure = function () {
    currentUser = undefined
    delete $rootScope.currentUser;
  };

  service.login = function (username, password) {
    var loginPayload = {username: username, password: password};
    return $http.post('/api/login', loginPayload).then(loginSuccess, loginFailure);
  };

  service.currentUser = function () {
    return currentUser;
  };

  return service;
}]);
```

# Define a Login View

Create a view and controller for the login functionality:

```
<div class="container">
    <div class="row error" ng-show="error">{{ error }}</div>

    <div class="row">
        <label>Username</label>
        <input ng-model="loginAttempt.username" />
    </div>

    <div class="row">
        <label>Password</label>
        <input type="password" ng-model="loginAttempt.password">
    </div>

    <button ng-click="doLogin()">Login</button>
</div>
```

The loginController will use the securityService to authenticate the user:

``` javascript
angular.module('app').controller('loginController', function($scope, $location, securityService) {

  $scope.loginAttempt = {};

  $scope.doLogin = function() {
    securityService
      .login($scope.loginAttempt.username, $scope.loginAttempt.password)
      .finally(function(result){
        var currentUser = securityService.currentUser();
        if (currentUser) {
          delete $scope.error;
          $location.path('/feed');
        } else {
          $scope.error = 'Invalid login';
        }
      });
  };

});
```

# Define Routing

Use the routing rules and the securityService to prevent non-logged in users from seeing anything but the login screen:

```
angular.module('app')

  // configure the routes
  .config(function ($routeProvider) {

    $routeProvider
      .when('/login', {
        templateUrl: '/app/login.html',
        controller: 'loginController'
      })
      .when('/home/:handle?', {
        templateUrl: '/app/home.html',
        controller: 'homeController'
      })
      .when('/feed', {
        templateUrl: '/app/feed.html',
        controller: 'feedController'
      })
      .otherwise({
        redirectTo: '/feed'
      })
  })

  // Protect all routes other than login
  .run(function ($rootScope, $location, securityService) {
    $rootScope.$on('$routeChangeStart', function (event, next) {
      if (next.$$route.originalPath != '/login') {
        if (!securityService.currentUser()) {
          $location.path('/login');
        }
      }
    });
  });
```

# Add Auth Token to HTTP Requests

Define an HTTP interceptor for all requests that applies the X-Auth-Token for logged in users.  Listen for the 'userChange' event to store the token:

``` javascript
angular.module('app').config(function ($provide, $httpProvider) {
  $provide.factory('httpAuthTokenInterceptor', function ($rootScope) {

    var token;

    $rootScope.$on('userChange', function (event, user) {
      if (user) {
        token = user.token;
        console.log('token: '+token);
      } else {
        token = undefined;
      }
    });

    return {
      'request': function (config) {
        if (token) {
          config.headers['Content-Type'] = 'application/json';
          config.headers['X-Auth-Token'] = token;
        }
        return config;
      }
    };
  });

  $httpProvider.interceptors.push('httpAuthTokenInterceptor')
});
```

# Define a Secured View

Use an Angular Resource to invoke a protected REST API in grails.  The Controller uses the Resource to fetch a list of restaurants:

``` javascript
angular.module('app').controller('feedController', function ($resource, $scope) {

  var Restaurant = $resource('/api/restaurants/:restaurantId', {restaurantId: '@id'});

  $scope.restaurants = Restaurant.query();

});
```

Define a view for the controller:

```
<h1>Your feed!</h1>

<div ng-repeat="r in restaurants">
    <h2>{{ r.name }}</h2>
</div>
```
