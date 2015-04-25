---
title: Web Application Development
layout: default
---

# Angular Routing
## Mike Calvo
## mike@citronellasoftware.com

---
# Rationale 1: Many Views Per App
- Single page app does not mean single view
- Google Mail:
  - Inbox
  - Compose Message
  - Read Message
  - Settings

---
# Rationale 2: Back Button
- Users have be come accustomed to using back/forward buttons
- Mostly on desktop browsers
- Should be have as expected in single page apps

---
# URL Routes and Templates
- Map URLs with views within the app
- Template is an HTML file that defines a view
  - Not a complete page
- URL comes after the # in the single page:
`http://localhost/auktion/#/listings`

---
#ngRoute
- Optional module in Angular
- 'ngRoute' module provides functionality
- Include "ng-route.js" in your page

``` javascript
angular.module("app", ["ngRoute"]);
```

---
# Configuring Router

``` javascript
angular.module("app").config(function($routeProvider) {
  $route.when("/checkout", {
    templateUrl: "/views/checkout.html"
  });
  $route.when("/products", function {
    templateUrl: "/views/products.html"
  });
  $route.otherwise("/home", function {
    templateUrl: "/views/home.html"
  })
});
```

---
# Templates
- Plain old HTML
- Does not include full page (HTML, HEAD, BODY)
- Contents are inserted into view placeholder
- Can contain directives
  - Including which controller feeds the view

---
# Example Template

``` html
<div ng-controller="CartController">
<table>
  <tr ng-repeat="item in cart.items">
    <td>{{ item.name }}</td>
    <td>{{ item.price }}</td>
    <td><input ng-model="item.quantity" /></td>
    <td><button ng-click="remove(item)">Remove</button></td>
  </tr>
</table>
</div>
```

---
# ngView
- Router needs to know where to put the views you've defined
- Use the ngView directive to mark the element

``` html
<ng-view> <!-- after route your templates go here --></ng-view>
```

OR

``` html
<div ng-view> <!-- stuff here --></div>

```
---
# Getting to Routes
- Use a simple HTML link:

``` html
<a href="#/checkout">Proceed to checkout</a>
```

---
# Getting to Routes in Code
- $location service
- Inject into your controller and specify path

``` javascript
angular.module("app")
  .controller("cartController", function($scope, $location) {

  $scope.editItem = function(item) {
    $location.path("/item")
  }
});
```

---
# Route Parameters
- Parameters can be supplied to route - prefix with : in url

``` javascript
$routeProvider.when("/product:id", {
  templateUrl: "/product.html"
});

$routeProvider.when("/customerProfile/:id/:addressId*") {
  templateUrl: "/customerAddress.html"
})
```

---
# Accessing Route Parameters
- The $routePrarams service can be injected into controller
- Each param defined in the route definition is a property

``` javascript
controller("addressController", function($scope, $routeParams) {
  var customerId = $routeParams.id;
  var addressId = $routeParams.addressId;
  // ...
});
```

---
# ngInclude
- Directive to include template or snippet into a view
- Does not require routing
- Simply includes the content in place
- Can be combined with ng-show or ng-if to conditionally show
- Can be used to break up complexity size of templates

---
# ngInclude example

``` html
<ng-include src="templates/item.html" ng-show="item">
  <!-- contents of item.html go here (only displayed if item is truthy) -->
</ng-include>
```

---
# Advanced Routing
- Angular-UI Project provides an advanced router
- Introduces sub-routes and states
  - Navigate up, down and to siblings
- [https://github.com/angular-ui/ui-router](https://github.com/angular-ui/ui-router)

---
# Add Angular Route to Grails Project
1. Add bower dependency for angular-route:
  `node_modules/.bin/bower install angular-route --save`
1. Include reference to angular-route in your application.js
1. Create a route configuration
1. Add ng-view directive into main page

---
# Include Reference to Angular Route
- Must come after main angular reference

``` javascript
//= require jquery/dist/jquery
//= require bootstrap/dist/js/bootstrap
//= require angular/angular
//= require angular-route/angular-route
angular.module('app', ['ngRoute']);
```

---
# Create a Route Configuration
- Good convention to put routes in their own file

``` javascript
angular.module('app').config(function($routeProvider) {
  $routeProvider.when("/plays", {templateUrl: "/templates/plays.html"});
  $routeProvider.when("/artist/:id", {templateUrl: "/templates/artist.html"});

  // Default route: plays screen
  $routeProvider.otherwise({templateUrl: "/templates/plays.html"});
});
```

---
# Page

``` html
<!DOCTYPE html>
<html>
<head>
  <asset:stylesheet href="application.css"/>
  <asset:javascript src="application.js"/>
</head>

<body ng-app="app">

<ng-view></ng-view>

</body>
</html>
```

---
# Add Routing to Muzic App
- Create views for plays and artist detail
- Add angular routing dependency
- Update application JS references
- Move all views into template files
- Add controllers

---
# Summary
- Angular routes help break up a complex app
  - Simplify views
  - Simplify controllers
- Angular routes allow browser buttons to work predictably
