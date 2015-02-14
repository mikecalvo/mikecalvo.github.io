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
app.controller, function(usefulService) {
    usefulService.goCrazy();
}
```

---
# Defining a Factory

``` javascript
angular.module('app').factory('factoryName', function() {
  // return something of value that can be injected into other things
  // example: return function(param) { doSomethingCoolWithValue(param); }

  return {
  }
});
```
