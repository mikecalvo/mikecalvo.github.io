---
title: Web Application Development
layout: default
---

# Angular Bootstrap
## Mike Calvo
## mike@citronellasoftware.com

---
# Modular Components
- Recall creating an app came from the following call that defines a module

``` javascript
angular.module('app', [ ])
```

- The second argument is the list of modules the new module depends on
- You can break up your functionality into libraries of modules that can be reused between applications
- Third-party Angular plugins/modules work this way

---
# Angular UI Bootstrap Module
- Twitter Bootstrap provides a collection of rich web UI controls
  - Examples: alerts, typeahead, modals, accordion
- [http://angular-ui.github.io/bootstrap/](http://angular-ui.github.io/bootstrap/)
- The Angular UI module makes these available in an Angular friendly way:
  - Directives and controllers

---
# Adding Angular UI Bootstrap To Grails Project
1. Use bower/grunt to install bootstrap and angular-ui
1. Update application.js and application.css to include dependencies
1. Add the module reference to your app module definition

---
# Install Latest Version of Bootstrap and Angular UI
- From your root directory
`./node_modules/.bin/bower install bootstrap angular-bootstrap --save`
- Verify:
  - Check bower.json
  - Check grails-app/assets/bower_modules

---
# Update JavaScript Asset References
- In grails-app/assets/javascripts/application.js:

``` javascript
//= require jquery/dist/jquery
//= require bootstrap/dist/js/bootstrap
//= require angular/angular
//= require angular-bootstrap/ui-bootstrap-tpls
//= require_self
//= require_tree .
```

---
# Update CSS Asset References
- In grails-app/assets/stylesheets/application.css

``` css
/*
*= require bootstrap/dist/css/bootstrap
*= require bootstrap/dist/css/bootstrap-theme
*= require main
*= require mobile
*= require_self
*/
```

---
# Upate App Module Dependencies
- Where you define your module (likely application.js):

``` javascript
angular.module('app', ['ui.bootstrap']);
```

- Run your app: make sure Angular still without errors to Javascript console

---
# Control 1: Typeahead
- Let's add a typeahead control to our edit artist name control
- As the user types the artist name, show a list of choices that match the name

---
# Typeahead: Example

``` javascript
$scope.getArtist = function (input) {
  var results = [];
  input = input.toLowerCase();
  var plays = $scope.plays;
  for (var i = 0; i < plays.length && results.length < 8; i++) {
    var artist = plays[i].artist;
    if (artist && artist.name.toLowerCase().indexOf(input) !== -1) {
      results.push(artist.name);
    }
  }

  return results;
}
```

``` html
<input placeholder="Artist"
                 ng-model="newPlay.artist.name"
                 typeahead="artist for artist in getArtist($viewValue)"/>
```

---
# Control 2: Alerts
- Alerts are highlighted areas for displaying messages
- danger: errors
- success: confirmation
- info: plain messages
- [http://angular-ui.github.io/bootstrap/#/alert](http://angular-ui.github.io/bootstrap/#/alert)

---
# Add a Confirmation Alert on Add
```javascript
$scope.alerts = [];

$scope.savePlay = function () {
  $scope.plays.push($scope.newPlay);
  $scope.alerts.push({type: 'success', msg: 'Song play added'});
  delete $scope.newPlay;
};

$scope.closeAlert = function(index) {
  $scope.alerts.splice(index, 1);
}
```

```html
<alert ng-repeat="alert in alerts" type="{{alert.type}}" close="closeAlert($index)">{{alert.msg}}</alert>
```

---
# Control 3: Modal Dialogs
- Add a delete button for each play
- Confirm the delete with a modal dialog
- Bootstrap Modal control
  - Supply a template for the modal (view)
  - Supply a controller for the modal
  - Provide handlers for close (ok) and dismiss (cancel)

---
# Add New Column To Plays Table

``` html
<table>
  <thead>
  <tr>
    <td>Title</td><td>Artist</td><td>Date/Time</td><td></td>
  </tr>
  </thead>
  <tr ng-repeat="play in plays">
    <td>{{ play.song.title }}</td>
    <td>{{ play.artist.name }}</td>
    <td>{{ play.time | date :'medium' }}</td>
    <td>
      <button ng-click="deletePlay(play)"><i class="glyphicon glyphicon-trash"></i></button>
    </td>
  </tr>
```

---
# Add deletePlay Behavior

```javascript
angular.module('app').controller('songPlaysController', function ($scope, $modal) {
  ...
$scope.deletePlay = function (play) {
  var modalInstance = $modal.open({
    templateUrl: 'confirmDialog.html',
    size: 'lg',
    controller: 'confirmDialogController',
    resolve: {
      message: function () {
        return 'Are you sure you want to delete "' + play.song.title + '" by ' + play.artist.name + '?'
      },
      title: function () {
        return 'Confirm Play Delete';
      }
    }
  });

  modalInstance.result.then(function () {
    $scope.plays.splice($scope.plays.indexOf(play), 1);
    $scope.alerts.push({type: 'success', msg: 'Song play removed'});
  });
}
```

---
# Create confirmDialog.html Template
- Put this in the web-app folder so it is served up from Grails:

``` html
<div>
  <div class="modal-header">
    <h3 class="modal-title">{{ title }}</h3>
  </div>
  <div class="modal-body">
    {{ message }}
  </div>
  <div class="modal-footer">
    <button class="btn btn-primary" ng-click="ok()">OK</button>
    <button class="btn btn-warning" ng-click="cancel()">Cancel</button>
  </div>
</div>
```

---
# Add a Controller for Confirm Dialog

``` javascript

  .controller('confirmDialogController', function ($scope, $modalInstance, title, message) {
    $scope.message = message;
    $scope.title = title;

    $scope.ok = function () {
      $modalInstance.close();
    };

    $scope.cancel = function () {
      $modalInstance.dismiss('cancel');
    };
  });
```

---
# Explore Other Controls
- Glyph Icons
- [http://angular-ui.github.io/bootstrap/](http://angular-ui.github.io/bootstrap/)
