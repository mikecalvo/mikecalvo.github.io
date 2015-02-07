---
title: Web Application Development
layout: default
---

# Introduction to Angular
## Mike Calvo
## mike@citronellasoftware.com

---

# Angular Provides
- Opinionated approach to building single-page web apps
- Small number of components, roles well-defined
- 2-way binding
- Modular, component-based architecture
- Extensible plugin-model

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
  - how to bind data
  - Repeat elements
  - Show or hide elements
  - Respond to events

---
# Angular Controllers
- Make data available in scope for view
- Handle user interactions: button presses, fetching data
- Akin to Grails Controllers

---
# Angular Directives
- Components that manipulate the DOM
- Akin to Grails tags

---
# Angular Filters
- Components that format values for display
- Can also filter out objects that should not be displayed

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
