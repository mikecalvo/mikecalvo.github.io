---
title: Web Application Development
layout: default
---

# Angular Directives

## Mike Calvo

## mike@citronellasoftware.com

---

# Angular Opinion
- To manipulate the DOM thou must use a directive

---

# Built-in Directives
- ng-repeat
- ng-model
- ng-if and ng-show
- ng-class
- ng-click and ng-change

---

# Third Party Directives
- Angular ui: typeahead, alerts, popups, accordion
- Data Table
- Angular UI Tree
- Sortable: drag and drop

---

# Reasons to Create Directives
- Reduce duplicative markup in views
- New UI metaphors unique to your application
- Share components between projects

---

# Observations
- Working with Angular will most likely involve creating directives
- It can be challenging
- Expect things to take longer that you would think

---

# Directive Types
- Element
`<alert>`

- Attribute
`<div ng-show="realDeal()">`

---

# Naming Directives
- Every directive has a name (used as either attribute or element)
- To avoid collisions, Angular recommends prefixing directives
- All built-in directives start with ng- (the actual directive name is ngDirectiveName)
- Angular ui directives start with ui-

---

# Matching Directives
- Angular looks for directive names by HTML tag or attribute names
- The following characters are removed and trigger camelCasing: `: - _`
- Most typical is the '-'
- alert resolves to alert
- ng-model resolves ngModel
- ui-accordion resolves to uiAccordion

---

# Directive Parameters
- scope
  - The scope for the directive.  Each directive gets its own
- element
  - The DOM element - jQueryLite or jQuery
- attributes
  - The DOM element attributes

---

# Creating a Directive
- Similar to creating controllers and filters
- Define a factory for directive using Angular module:

``` javascript
angular.module('app').directive('prefixName', function() {
  return directiveDefinition
})
```

---

# Template-expanding Directive
- Most simple directive to create
- Return markup that will be inserted into view where your directive lives
- Simplest ones rely on information provided by a collaborating controller
- Template can be provided inline or as a url reference

---

# Example Template-expanding

``` javascript
angular.module('docsSimpleDirective', [])
.controller('Controller', ['$scope', function($scope) {
  $scope.customer = {
    name: 'Naomi',
    address: '1600 Amphitheatre'
  };
}])
.directive('myCustomer', function() {
  return {
    template: 'Name: {{customer.name}} Address: {{customer.address}}'
  };
});
```

``` html
<div myCustomer></div>
```

---

# Restricting Directive Use
- Return a 'restrict' value to limit how the directive can be used:
- 'A' : Attribute name
- 'E' : Element name
- 'C' : Class name

``` javascript
return {
  restrict: 'A',
  templateUrl: 'my-directive.html'
}
```

---

# Recommendations on Restricting
- Use element when directive completely controls the contents
- Use attribute when decorating an existing element with new functionality
- Never use class directives

---

# Scope Isolation
- Directive can specify characteristics of it's scope
- Define directive scope values from element attributes

``` javascript
return {
  restrict: 'E',
  scope: {
    customerInfo: '=info' // scope.customerInfo is value from info attribute
  },
  templateUrl: 'template.html'
}
```

---

# Scope Isolation Best Practices
- Isolating scope makes your directive more reusable
- Each directive can have it's own data
- Keep In Mind: isolate scope means the only values in the directive scope are the ones defined in the directive

---

# DOM-Manipulating Directive
- Directives can also work with jQuery-like functionality to add elements to the DOM
- These directives define a link function which accepts the following arguments:
  - scope - scope object for the directive
  - element - jqLite (simplified jQuery) element
  - attrs - normalized element attributes (dashes converted to camelCase)

---

# Example DOM Directive

``` javascript
angular.module('app').directive('currentTime', function($interval, dateFilter) {
  return {
    link: function(scope, element, attrs) {
      var timeoutId;
      function updateTime() {
        element.text(dateFilter(new Date(), attrs.currentTime))
      }

      element.on('$destroy', function() {
        $interval.cancel(timeoutId)
      });

      timeoutId = $interval(function() {
        updateTime();
      }, 1000);
    }
  }
});
```

``` html
<span current-time="yyyy-MM-dd h:mm:ss a"></span>
```

---

# Creating DOM elements
- Use the function angular.element() to create new elements
  Example: `var listElem = angular.element("<ul>")`
- Elements can be appended to the element on which the directive lives
  Example: `element.append(listElem)`

---

# Directives that Wrap Content
- Set the 'transclude' property to true
- This will render elements inside the directive element as normal
- They will NOT inherit the scope of the directive
- Best for creating directives that wrap arbitrary content

---

# Example Transcluded Directive

``` javascript
angular.module('app').directive('boxedContent', function() {
  return {
    restrict: 'E',
    transclude: true,
    link: function(scope, elem) {
      elem.css({'border', '1px solid black'});
    }
  }
});
```

``` html
<boxed-content>
  <span>This text will have a box around it: {{ scopeValue }}</span>
</boxed-content>
```

---

# jqLite
- Element in link function is a jqLite element
- It is a reduced-functionality version of jQuery
- Many of the commonly used functions exist
- Full jQuery can be used - simply include jQuery in your page

---

# Example of jqLite functions
- find(selector) - find child elements using a CSS selector
- append(element), prepend(element)
- text(elementText) - set the text within the element
- html(htmlText) - set the html contents within the element
- on(events, handler), off(events, handler), triggerHandler(event)

---

# Directive Controllers
- A directive can define a controller that responds to user events and exposes behaviors just like regular controllers
- Complex directives will often have a controller

---

# Requiring Other Directives
- Directives can use other directives
- For example your directive enhances the input field but requires the 2-way binding provided by the ngModel directive
- Requiring a directive makes it available to your directive
- The controller for required directives are injected into the link function

---

# Example Required Directive

``` javascript
angular.module('app').directive('blacklist', function() {
  return {
    require: 'ngModel',
    link: function(scope, elem, attr, ngModel) {
      var blacklist = attr.blacklist.split(',');

      ngModel.$parsers.unshift(function(value) {
        var valid = blackist.indexOf(value) === -1;
        ngModel.setValidity('blacklist', valid);
        return valid ? value : undefined;
      });

      ngModel.$formatters.unshift(function(value) {
        ngModel.$setValidity('blacklist', blacklist.indexOf(value) === -1);
        return value;
      })
    }
  }
})
```

``` html
<input type="text" ng-model="data.fruitName" blacklist="apples, grapes, cherries">
```

---

# Notes On Required Directives
- Directives can be required on the element OR in a parent somewhere
  - Prefix the directive name with a '^'
- Multiple directives can be required
  - In this case require should be an Array of directive names
  - When multiples are required they are passed as an Array to the link function in the order required

---
# Directive Summary
- Use directives to create reusable components for modifying the DOM
- Simple, template-based directives are a good way to get started
- Most useful directives will manipulate the DOM or interact with other directives
- Isolate the scope of your directives to enable repeated usage within a view
