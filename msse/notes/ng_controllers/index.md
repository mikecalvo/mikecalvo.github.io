---
title: Web Application Development
layout: default
---

# Angular Controllers

## Mike Calvo

## mike@citronellasoftware.com

---

## Goal
- Build a simple app that
  - Recaps basics of angular application
  - Covers controller fundamentals
  - Leverages controller from the view
  - Uses bootstrap along the way for some `pizazz`

---

## Demo app
- Party invitation tracker called `Party People!`
- 3 main sections
  - Header
  - List of people invited
  - Form to invite others

---

## Mockup of `Party People!`

```html
<div id="container">
  <div id="header">
      <h1>Party People!</h1>
  </div>

  <div id="list">
      <h3>Who's invited?!</h3>
      <ul>
          <li>Person 1</li>
          <li>Person 2</li>
      </ul>
  </div>

  <div id="form">
      <h3>Invite someone else?!</h3>

      <div id="first-name">First Name</div>

      <div id="last-name">Last Name</div>
  </div>
</div>
```

---

## Creating an Angular Application
`angular.module('appname', [ ])`
- The array after the name is important
- List of application dependencies
  - More on this later
- Returns a reference to the angular application
- The instance can also be obtained at any time:
  `angular.module('appname')`

---

## Creating the Angular View
- Add ng-app attribute to your page
- Value is the app name provided in the `angular.module` call
- Typically this is at the body or html element level:
`<html ng-app="app">`

---

## ng-app
- What is ng-app?
- Angular directive
- It is an attribute that the Angular system knows how to process
- When Angular starts up it looks for an ng-app attribute to know where on the page the app will live
- All angular HTML code must exist within an element annotated with ng-app

---

## Controllers
- Support the view
- Provide data/model to the view
- Handle user interactions
- Control the flow of the application

---

## One Screen/View Per Controller
- Single page app, multiple views
- Single page means the entire page is only loaded once
- Application may show different screens of information
- Each screen should have its own controller

---

## Creating a Controller
- Controllers are created from the application
- Specify the name of the controller
- Specify a function defining the controller logic
  - Parameters to the function are controller dependencies

---

## Example Controller Definition
``` javascript
angular.module('appname').controller('exampleController', function($scope) {
  // controller logic is in here
})
```

---

## Good Practice: Controller Per File
- Angular promotes component-based development in JavaScript
- Common Conventions:
  - Create a controllers directory for your controllers
  - Put each controller in its own file

---

## Controller Lifecycle
- The controller function is called when an element on the page includes the controller
- ng-controller="controllerName"
- An app usually has many controllers
- When angular finds an ng-controller attribute the function defining that controller is called
- Parameters to the function are resolved by Angular

---

## Angular dependency injection
- Angular provides a dependency resolution model
- Some dependencies are a core part of angular
- These are typically prefixed with a $
- Example: $scope

---

## Scope
- Every Angular component has a scope
- This defines the set of data that is visible to the component
- Scopes are hierarchical
- Child scopes can see data their parents scopes

---

## Angular View Expressions
- Output value from scope directly into the page
- Wrap expression within {{ }}
- Can be any valid javascript expression
- Two-way binding when bound to an input element
- One-way binding (model -> view for all other elements

---

## Repeating over Lists
- Angular 'ng-repeat' directive
- Causes the element on which it is placed to be repeated for each time in the expression
  - Everything within that element is a template.
- Repeat expression defines a loop variable that can be referenced within the ng-repeated element  

---

## Example Controller: Party Planning Controller

``` javascript
angular.module('partyPeople').controller('invitationController', function ($scope) {
    $scope.title = "Party People!";

    var getInvitees = function () {
        //this would come from your rest api
        return [
            {name: {first: "Homer", last: "Simpson"}},
            {name: {first: "Ned", last: "Flanders"}},
            {name: {first: "Milhouse", last: "Van Houten"}}
        ];
    };

    $scope.invitees = getInvitees();
});
```

---

## Example Table Row

``` html
<tr ng-repeat="invitee in invitees">
    <td>{{invitee.name.first}}</td>
    <td>{{invitee.name.last}}</td>
</tr>
```

---

## View Example: Display Guest list

``` html
    <div id="list">
        <h3>Who's invited?!</h3>
        <table class="table table-condensed table-striped table-bordered">
            <thead>
            <tr>
                <th>First</th>
                <th>Last</th>
            </tr>
            </thead>
            <tbody>
            <tr ng-repeat="invitee in invitees">
                <td>{{invitee.name.first}}</td>
                <td>{{invitee.name.last}}</td>
            </tr>
            </tbody>
        </table>
    </div>
```

---

## Handling Input
- Getting a controller to respond to UI events
1. Create a function on the controller
1. Assign the function to the scope
1. Add a directive to the element on the page connecting the event to the function

---

## Directive: ng-model
- Bind an input field to a model value
- 2-Way binding:
  - View can update model
  - Model can update view
- Available on all input fields (input, select, textarea, etc)

---

## Example: Bind input to property

```html
<input type="text" ng-model="user.name.last"/>
```

---

## Create a Form for Inviting
- Inputs: First name, last name, E-mail
- Buttons:
 - Invite - adds to list
 - Reset - clears form

---

## Example View: Invite someone
```html
<form id="add-user-form" novalidate>
    <div class="form-group">
        <label for="firstNameInput">First Name</label>
        <input id="firstNameInput" class="form-control" placeholder="First Name" type="text"/>
    </div>

    <div class="form-group">
        <label for="lastNameInput">Last Name</label>
        <input id="lastNameInput" class="form-control" placeholder="Last Name" type="text"/>
    </div>

    <div class="form-group">
        <label for="emailAddressInput">Email</label>
        <input id="emailAddressInput" class="form-control" placeholder="E-mail address" type="email"/>
    </div>
    <button type="submit" class="btn btn-primary">Invite</button>
    <button type="submit" class="btn btn-default">Reset</button>
</form>
```

---

## Controller Behaviors
- Functions on the controller that the view can call are behaviors
- They can be invoked from any directive:
  - Result of an event (ng-click)
  - Evaluating a collection (ng-repeat)
  - Determining state (ng-show="isItVisible()")

---

## Behavior Parameters
- Example:

``` html
<div ng-repeat="i in items">
  <span ng-show="showIt(i)">Conditionally Show This based on item type</span>
  <span ng-hide="!showIt(i)">Otherwise show this</span>
</div>
```

---

## Invite Behavior (function):

``` javascript
$scope.invite = function(guest) {
  $scope.invitees.push(guest);
  $scope.reset();
};
```

---

## Example: Bind to invite behavior
- In HTML:
```html
<button ng-click="invite(user)">Invite</button>
```

---

## Directive: ng-show

- Conditionally show or hide an element
- Value of attribute is expression that evaluates to true/false

---

## Example : Debug a $scope value
- Use a checkbox to toggle on or off a debug box
- pretty prints json of invite form

``` html
<div class="checkbox">
    <label>
        <input type="checkbox" ng-model="debug"> Debug
    </label>
</div>
<pre id="debug" ng-show="debug">user = {{user | json}}</pre>
```

---

## Creating Dynamic HTML Attributes
- If your element id requires a dynamic value, use ng-id
- If your class requires a dynamic value (dynamically styled) use ng-class

---

## Example ng-id
- Html element ids must be unique
-  Especially useful for finding elements in Geb tests

``` html
<tr ng-repeat="invitee in invitees track by $index" ng-id="invitee-{{$index}}">
    <td ng-id="invitee-first-name-{{$index}}">{{invitee.name.first}}</td>
    <td ng-id="invitee-last-name-{{$index}}">{{invitee.name.last}}</td>
</tr>
```

---

## Form validation
- Angular provides robust form validation
- Bootstrap provides great form state styling support
- Together they make form validation really easy

---

## Enabling form validation
- Add name attribute to your Form and input elements
  - how angular references values for validation
- Use `ng-class` on `form-group` element to toggle `has-error` class

```html
ng-class="{ 'has-error' : userForm.first.$invalid && !userForm.first.$pristine  }"
```

---

## Create re-usable hasError behavior
- Simplify validation by putting logic in your controller

```javascript
$scope.hasError = function(formValue) {
        return formValue.$invalid && !formValue.$pristine
    };
```

- Update your form-group elements to call behavior

```html
ng-class="{ 'has-error' : hasError(userForm.first) }"
```

---

## Example Form-group with validation

```html
<form id="add-user-form" name="userForm" novalidate>
            <div class="form-group" ng-class="{ 'has-error' : hasError(userForm.first) }">
                <label for="firstNameInput">First Name</label>
                <input id="firstNameInput" name="first" class="form-control" placeholder="First Name" type="text"
                       ng-model="user.name.first" required/>
            </div>
             <!-- rest of form -->
```

---

## Set button states
- Disable Invite button when form is invalid
- Disable Reset button when form is pristine

```html
<button type="submit" ng-disabled="!userForm.$valid" class="btn btn-primary" ng-click="invite(user)">Invite</button>
<button type="submit" ng-disabled="userForm.$pristine" class="btn btn-default" ng-click="reset()">Reset</button>
```

---

## Select Control
- Angular provides support for the select/dropdown control
- ng-options directive used to populate the options
- Values can be objects
  - Different properties can be used for display labels and selected values
- Controller generally populates the list that backs the select

---

## Example ng-options for who's invited
- use our collection of people invited

``` html
<select ng-model="selectedGuest" ng-options="invitee.name.first for invitee in invitees">
     <option value="">Pick someone</option>
 </select>
 <p>Current Guest: {{selectedGuest.email}}</p>
```

---

## Reacting to changes
- Angular allows you to react to changes in two ways
  - $scope.watch
  - ng-change
- $scope.watch will notify for any change, including created
- ng-change won't notify on create.
- ng-change is generally all you need

---

## $scope.watch basics

- $scope.$watch('value', callback)
  - callback called any time the $scope.value changes
- $scope.$watchCollection('collection', callback)
  - callback called any time something added or removed

---

## ng-change example
- Track how many times debug checkbox has been activated
- Update controller

```javascript
$scope.debugCount = 0;
$scope.trackDebug = function () {
    if($scope.debug) {
        $scope.debugCount++
    }
}
```

---

## Track debug in view
- track checkbox clicks with `ng-change`

```html
<input type="checkbox" ng-model="debug" ng-change="trackDebug()">
```

- add an element to display debug counter

```html
 <pre id="debugTracker">Debug has been activated {{debugCount}} times</pre>
```

---

## Controller and Views Recap
- Controllers and views have specific functions
- They should be used for specific things and only those specific things

---

## Controllers Should
- Initialize the scope
- Contain functionality required by the view to present the model
- Contain functionality to update the scope based on user interaction

---

## Controllers Should Not
- Manipulate the DOM
- Persist data
- Manipulate data outside the scope

---

## Views Should
- Contain markup required to present data to the user
- Contain minimal amounts of logic relating to the presentation of the data

---

## Views Should Not
- Contain any complex logic
- Contain any logic that alters the model

---

## Questions?
