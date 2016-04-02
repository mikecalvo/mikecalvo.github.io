## Angular Routing
### Mike Calvo
### mike@citronellasoftware.com

---

# Many Views Per App
- Single page app does not mean single view
- Break up monolithic web pages
- Composition - easier to manage
- User experience is seamless
- Pages don't reload (usually)


---

# Add Angular Route to Grails Project
1. Add the following to your build.gradle
`'angular-route'('1.5.x') {
    source 'angular-route.js'
  }`
1. run bowerRefresh to push the dependency
`./gradlew bowerRefresh`
1. Add dependency to application.js
1. Create a router to manage your routes
`/routes/<your-router.js>`
1. Add ng-view directive into main page (index.gsp)
`<div ng-view></div>`


---
# Include Reference to Angular Route
- Must come after main angular reference
- Make sure require_tree <your app name> in order to include all .js

``` javascript
//= require jquery/dist/jquery
//= require bootstrap/dist/js/bootstrap
//= require angular/angular
//= require angular-route/angular-route
//= require_tree app
var app = angular.module('app', ['ngRoute']);
```
---

#ngRoute
- Optional module in Angular
- 'ngRoute' module provides functionality
- Include "angular-route.js" in your page

``` javascript
angular.module("app", ["ngRoute"]);
```

---

# ngView
- Router needs to know where to put the views you've defined
- Use the ngView directive to mark the element

``` html
<ng-view> <!-- after route your partials go here --></ng-view>
```

OR

``` html
<div ng-view> <!-- stuff here --></div>

```

---

# Configuring Router

``` javascript
angular.module("app").config(function($routeProvider) {
  $routeProvider
    .when('/contact', {
      templateUrl: 'angular-router/partials/contact.html',
      controller: 'contactController'
    })
    .when('/manageUser/:action/:id?', {
      templateUrl: 'angular-router/partials/manageUser.html'
    })
    .otherwise({
      redirectTo: '/home'
    })
});
```
---

# URL Routes and Partials
- Map URLs with views within the app
- Partial is an HTML file that defines a view
- URL comes after the # in the single page:
`http://localhost/auktion/#/listings`
- Partials go in
`src main webapp`

---

# Partials (Templates, Snippets)
- Plain old HTML
- Does not include full page (HTML, HEAD, BODY)
- Contents are inserted into view placeholder
- Can contain directives
  - Including which controller feeds the view

---

# Example Partial

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
<ng-include src="partials/item.html" ng-show="item">
  <!-- contents of item.html go here (only displayed if item is truthy) -->
</ng-include>
```

---

# Benefits of angular-router

1. Minimal learning curve
1. Less things to make sense of

---

# Drawbacks of angular-router

1. Lack of complication makes complicated projects difficult
1. Applications become a series of toggles
1. Un-opinionated nature leads to less structure

---

# Advanced Routing
- Angular-UI Project provides an advanced router
- Introduces sub-routes and states
  - Navigate up, down and to siblings
- [https://github.com/angular-ui/ui-router](https://github.com/angular-ui/ui-router)

---

# Add ui-router to Grails Project
1. Add the following to your build.gradle
`'ui-router'('0.2.x')`
1. run bowerRefresh to push the dependency
`./gradlew bowerRefresh`
1. Add dependency to application.js
1. Create a router to manage your routes
`/routes/<your-router.js>`
1. Add ng-view directive into main page (index.gsp)
`<div ui-view></div>`

---

# Angular Router to UI Router
1. Works similiarly to angular-router
1. ngRoute -> state
1. ui-sref to Navigate
1. child views use the . notation

---

# Benefits of ui-router

1. Pages have a state
1. Simple to use for easy scenarios
1. View nesting is powerful
1. Can make a single pages more feature rich without sacrificing complexity
1. Understands hash view changes
1. Enforces more structure, can't be willy nilly

---

# Drawbacks of ui-router

1. Learning curve for children nesting
1. Enforces more structure, can't be willy nilly
1. Debugging is occasionally difficult

---

# Summary
- Angular routes help break up a complex app
  - Simplify views
  - Simplify controllers
- angular-router and ui-router are both decent, but for complicated apps you probably want ui-router
