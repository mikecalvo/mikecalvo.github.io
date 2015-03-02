---
title: Web Application Development
layout: default
---

# Angular Services and Factories
## Mike Calvo
## mike@citronellasoftware.com

---
# Sharing Code Angular-Style
- Services: reusable components
  - Singletons
- Factories: functions to produce reusable components:
  - Functions or Objects
- Both can be injected into other components

---
# Defining a Service

``` javascript
app.service('usefulService', function() {
  this.goGrazy() { console.log('Yeah Baby!!!'); }
});
```

Use the service:

``` javascript
app.controller('MyController', function(usefulService) {
    usefulService.goCrazy();
});
```

---
# Defining a Factory

``` javascript
angular.module('app').factory('confirmDialog', function() {
  // return something of value that can be injected into other things

  return function(template, scope) {
    // put logic here to create a dialog and populate scope with values
    return modalInstance;
  }
});
```

---
# Use a Factory

``` javascript
app.controller('SongController', function(confirmDialog) {
  $scope.removeSong = function(song) {
    confirmDialog('deleteSongConfirmation', {message: 'Delete song '+song.title+'?'}).
      then(function() {
        song.$delete({id: song.id});
      });
  }
})
```
