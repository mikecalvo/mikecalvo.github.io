---
title: Web Application Development
layout: default
---

## Introduction to Angular

### Marc Kapke

#### kapkema@gmail.com
---

# Angular Provides
- Opinionated approach to building single-page web apps
- Small number of components, roles well-defined
- 2-way binding
- Modular, component-based architecture
- Extensible plugin-model
- https://docs.angularjs.org/guide

---

# Why Single-Page Apps?
- More responsive
- More Interactive
- Easy way to serve mobile and desktop customers
- Users have come to expect it

---

# Angular Views
- Just plain HTML
- Special attributes tell Angular
  - How to bind data
  - Repeat elements
  - Show or hide elements
  - Respond to events

---

# Angular Data binding
- Two way binding
  - Model automatically updates view
  - View automatically updates model

---

# Angular Expressions
- Expressions bind view to model
- Binding in HTML attributes - (mustaches required)
  - `<span title="{{ attrBinding }}">{{ textBinding }}</span>`
- Binding in Angular directives - (no mustaces required)
  -  `ng-click="functionExpression()"`

---

# Angular Controllers
- Make data available in scope for view
- Handle user interactions: button presses, fetching data
- Business logic - no presentation logic!
- Akin to Grails Controllers

---

# Angular Routes
- Makes it easy to wire together controllers, view templates, and the current URL location in the browser.
- Handles deep linking, which lets you utilize the browser's history (back and forward navigation) and bookmarks.

---

# Angular Directives
- Components that manipulate the DOM
- Usually in the form of a custom HTML element or attribute
- Akin to Grails tags

---

# Angular Filters
- Components that format values for display
  - Format a decimal as currency
  - Change casing
- Works on single value or list
  - Filter out 'hidden' items in list
- Can be chained together to transform values multiple ways

---

# Filter syntax
- Use in views/templates
  - `{{ expression | filter1 | filter2 | ... }}`
- Can be used in Controllers/Services too
  - Code smell - likely puts presentation logic where it shouldn't be

  ---

# Angular Services
- Shared code used by multiple controllers or directive
- If two controllers, directives or filters need the same function, put it in a service
- Akin to Grails Services

---

# Angular Resources
- Components for easily fetching and posting REST services
- Configure base url
- All REST methods provided by default
- Customizable

---
# Angular Dependencies
- Angular requires jQuery
  - provides jqLite if no jQuery specified
- Must be included in the page before the angular include

---
# Summary
- Angular is an opinionated web client framework
- Controllers, Directives, Resources, Services and Filters are major components
