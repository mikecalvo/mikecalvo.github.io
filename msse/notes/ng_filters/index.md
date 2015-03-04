---
title: Web Application Development
layout: default
---

# Angular Filters
## Mike Calvo
## mike@citronellasoftware.com

---

# Role of Filters
- Transform data usually for display
- Format: currency, dates, numbers
- Filter: reduce collections sets
- Typically used with directives

---
# Example Built-in Filters:
- currency
- date
- json
- number
- uppercase
- lowercase

---
# Using a Filter
- Within data bindings:

``` html
<td class="text-right">{{ p.price | currency }}</td>
<td class="date">{{ sale.date | date:"MM-dd-yy" }}</td>
<td class="date">{{ p.price | number:0 }}</td>
```

---
# Locale-Specific Formats
- Include the angular local file-specific to user's locale to get correct currency, number and date formats:

``` html
<script src="angular-locale_fr-fr.js"></script>
```

---
# Date Filter
- Supports standard formatting using y, M, d, m, h, s
- Logical formats

---
# Date Logical Formats
medium MMM d, y h:mm:s a
short M/d/yy h:mm a
fullDateEEE, MMM d, y
longDate MMM d, y
mediumDate MMM d, y
shortDate M/d/yy
mediumTime h:mm:ss a
shortTime h:mm a

---
# JSON Filter
- Good debugging tool
- Converts the filtered value into JSON

``` html
<div>{{order |  json}}</div>
```

---
# Filtering Collections
- Used with directives that operate on arrays (ng-repeat)
- Example: limitTo
  - Constrains the array to only n items

``` html
<tr ng-repeat="p in products | limitTo:5">
```

---
# Selecting Items: filter Filter
- Only use values that match the properties specified

``` html
<tr ng-repeat="p in products | filter{category: 'memory'}">
```

---
# Ordering Items: orderBy Filter

``` html
<tr ng-repeat="p in products | orderBy:'price'">

<!-- decending -->
<tr ng-repeat="p in products | orderBy:'-price'">

<!-- custom sort method (defined in controller) -->
<tr ng-repeat="p in products | orderBy:'customSort">

<!-- multiple predicates -->
<tr ng-repeat="p in products | orderBy:'[customSort, '-price']">
```

---
# Chaining Filters
- Use the | to chain filters together

``` html
<tr ng-repeat="p in products | orderBy:'price' | limitTo: 5">
```

---
# Creating Your Own Filter
- The module service has a method to create a filter
- Return a function that accepts a value or array and returns the filtered result

``` javascript
angular.module('app').filter('labelCase', function() {
  return function(value) {
    if (angular.isString(value)) {
      var upper = value.toUpperCase();
      var lower = value.toLowerCase();
      return upper[0] + lower.substr(1);
    }

    return value;
  };
})
```

---
# Example Collection Filter

``` javascript
angular.module('app').filter('skip', function() {
  return function(data, count) {
    if (angular.isArray(data) && angular.isNumber(count)) {
      if (count > data.length || count < 1) {
        return data;
      }

      return data.slice(count);
    }

    return data;
  }
})
```

``` html
<tr ng-repeat="o in orders | skip:2 | limitTo: 5">
```

---
# Reusing Filters In Code
- Inject the $filter into your component
- Look up the filter you want using the $filter service

``` javascript
module.controller('MyController', function($scope, $filter, values) {
  $scope.values = $filter('limitTo')(values, 5);
});
```

---
# Summary
- Angular is strongly opinionated on where data transformation for display should happen
- Do it in filters
- Filters can accept arguments and use other filter
- Filters can operate on single values and collections
