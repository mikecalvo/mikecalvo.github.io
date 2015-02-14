---
title: Web Application Development
layout: default
---

# Angular Rest Support
## Mike Calvo
## mike@citronellasoftware.com

---
# $http
- Core angular service that provides http request access to Angular app
- Usually used to make AJAX calls
- Inject into controllers, filters, services, etc
- Methods for making any type of HTTP request (get, post, put, etc)
- Default request goes back relative to the orinating server

---
# Examples: $http

``` javascript
$http.get('static/data/songs.json', function(data) {
  $scope.songs = data;
});
```

---
# Interceptors

---
# $resource
