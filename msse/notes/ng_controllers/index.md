---
title: Web Application Development
layout: default
---

# Angular Controllers
## Mike Calvo
## mike@citronellasoftware.com

---
# Controllers
- Support the view
- Provide data/model to the view
- Handle user interactions
- Control the flow of the application

---
# One Screen/View Per Controller
- Single page app, multiple views
- Single page means the entire page is only loaded once
- Application may show different screens of information
- Each screen should have its own controller

---
# Creating an Angular Application
`angular.module('appname', [ ])`
- The array after the name is important
- List of application dependencies
  - More on this later
- Returns a reference to the angular application
- The instance can also be obtained at any time:
  `angular.module('appname')`

---
# Creating the Angular Application in HTML
- Add ng-app attribute to your page
- Value is the app name provided in the `angular.module` call
- Typically this is at the body or html element level:
`<html ng-app="app">`

---
# Angular Attributes
- What is ng-app?
- Angular directive
- It is an attribute that the Angular system knows how to process
- When Angular starts up it looks for an ng-app attribute to know where on the page the app will live
- All angular HTML code must exist within an element annotated with ng-app

---
# Creating a Controller
- Controllers are created from the application
- Specify the name of the controller
- Specify a function defining the controller logic
  - Parameters to the function are controller dependencies

``` javascript
angular.module('appname').controller('exampleController', function($scope) {
  // controller logic is in here
})
```

---
# Good Practice: Controller Per File
- Angular promotes component-based development in JavaScript
- Common Conventions:
  - Create a controllers directory for your controllers
  - Put each controller in its own file

---
# Controller Lifecycle
- The controller function is called when an element on the page includes the controller
- ng-controller="controllerName"
- An app usually has many controllers
- When angular finds an ng-controller attribute the function defining that controller is called
- Parameters to the function are resolved by Angular

---
# Angular dependency injection
- Angular provides a dependency resolution model
- Some dependencies are a core part of angular
- These are typically prefixed with a $
- Example: $scope

---
# Scope
- Every Angular component has a scope
- This defines the set of data that is visible to the component
- Scopes are hierarchical
- Child scopes can see data their parents scopes

---
# Example Controller: songPlaysController

``` javascript
angular.module('app').controller('songPlaysController', function ($scope) {
  var getPlayData = function () {
    return [
      {song: {title: 'Blue Monday'}, artist: {name: 'New Order'}, time: new Date('02/14/2015 12:37:00')},
      {song: {title: 'We Want the Airwaives'}, artist: {name: 'Ramones'}, time: new Date('02/14/2015 11:32')},
      {song: {title: 'Kids With Guns'}, artist: {name: 'Gorillaz'}, time: new Date('02/14/2015 11:22')}
    ];
  };

  $scope.plays = getPlayData();
});
```

---
# Angular View Expressions
- Output value from scope directly into the page
- Wrap expression within {{ }}
- Can be any valid javascript expression
- One-way binding: view follows the model

---
# Repeating over Lists
- Angular ng-repeat directive
- Causes the element on which it is placed to be repeated for each time in the expression
- Repeat expression defines a loop variable that can be referenced within the ng-repeated element

``` html
<tr ng-repeat="play in plays">
  <td> {{ play.song.title }}
</tr>
```

---
# View Example: Display Song Plays

``` html
<div ng-controller="songPlaysController">
  <table>
    <thead>
    <tr>
      <td>Title</td><td>Artist</td><td>Date/Time</td>
    </tr>
    </thead>
    <tr ng-repeat="play in plays">
      <td>{{ play.song.title }}</td>
      <td>{{ play.artist.name }}</td>
      <td>{{ play.time | date :'medium' }}</td>
    </tr>
  </table>
</div>
```

---
# Handling Input
- Getting a controller to respond to UI events
1. Create a function on the controller
1. Assign the function to the scope
1. Add a directive to the element on the page connecting the event to the function

---
# Example: Handle Button Press
- In HTML:
`<button ng-click="addPlay()">Add a Play</button>`

- In Controller:

``` javascript
$scope.addPlay = function() {
  var now = new Date();
  $scope.newPlay = {time: now};
  $scope.plays.push($scope.newPlay);
};
```

---
# Editing New Play
- Allow the user to specify song name and artist
- Conditionally display input fields
- Conditionally display a save button

---
# Directive: ng-show
- Conditionally show or hide an element
- Value of attribute is expression that evaluates to true/false

``` html
<div ng-show="flag">Only shown if $scope.flag is truthy</div>
```

---
# Directive: ng-model
- Bind an input field to a model value
- 2-Way binding:
  - View can update model
  - Model can update view
- Available on all input fields (input, select, textarea, etc)

```html
<input ng-model="newPlay.artist.album" >
```

---
# Create a Row For Editing
- Only shown if there is a new play available to edit

``` html
<tr ng-show="newPlay">
  <td><input placeholder="Title" ng-model="newPlay.song.title"></td>
  <td><input placeholder="Artist" ng-model="newPlay.artist.name"></td>
  <td>{{ newPlay.time | date :'medium' }} <button ng-click="savePlay()">Save</button></td>
</tr>
```

- Also, hide the add button while we're creating a new play:

``` html
<button ng-click="addPlay()" ng-show="!newPlay">Add a Play</button>
```

---
# Controller Changes:

```javascript
$scope.addPlay = function() {
  var now = new Date();
  $scope.newPlay = {time: now};
};

$scope.savePlay = function() {
  $scope.plays.push($scope.newPlay);
  delete $scope.newPlay;
};
```

---
# Controller Behaviors
- Functions on the controller that the view can call are behaviors
- They can be invoked from any directive:
  - Result of an event (ng-click)
  - Evaluating a collection (ng-repeat)
  - Determining state (ng-show="isItVisible()")

---
# Behavior Parameters
- Example:

``` html
<div ng-repeat="i in items">
  <span ng-show="showIt(i)">Conditionally Show This baed on item type</span>
  <span ng-hide="!showIt(i)">Otherwise show this</span>
</div>
```

---
# Dependencies

---
# Watching Scope

---
# App Routes

---
# Sub views

---
# Angular Plugins/Dependencies
- ui-router

---
# Modular Components
