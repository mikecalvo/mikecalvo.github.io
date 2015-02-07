---
title: Web Application Development
layout: default
---

# Twitter Bootstrap
## Mike Calvo
## mike@citronellasoftware.com

---

# What Is Bootstrap
- Responsive HTML, CSS and JS Framework
- Includes CSS designed to look good on variety of platforms
- Makes use of CSS Media Queries

---

# Installing Bootstrap
- `bower install bootstrap`
- Adds bower_components/bootstrap
- Resources to include are in 'dist' folder
  - CSS
  - JavaScripts
  - Glyph icons

---

# Bootstrap Page Requirements
- Include jQuery library before Bootstrap
- Set the viewport:
`<meta name="viewport" content="width=device-width, initial-scale=1">`
- Include the bootstrap css AND js files (from bower_components)

---

# Mobile Friendly
- The viewport ensures property rendering and touch zooming
- Disabling zooming can make the site feel more like an app:
`<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">`

---

# Bootstrap Grid System
- 12 columns wide
- Scales when the device/viewport size grows and shrinks
  - Columns wrap to rows at the appropriate time
- Grid system requires a container
  `<div class=container>...</div>`
  `<div class="container-fluid">...</div>`

---

# Use Bootstrap Classes to Control Grid
- .row - define a new row
- .col-xs-[colspan] : phones
- .col-sm-[colspan] : tablets
- .col-md-[colspan] : desktop (992px)
- .col-lg-[colspan] : desktop (1200px)

---
# Examples of Grid System
[http://getbootstrap.com/css/](http://getbootstrap.com/css/)

---

# Offsets
- Classes allow providing blank spaces between columns
- .col-xs-[cols]
- .col-sm-[cols]
- .col-md-[cols]
- .col-lg-[cols]

---
# Beautiful CSS
- Styling for semantic elements (Headings, paragraphs, emphasize, small ,etc)
- Alignment classes: text-left, text-center, text-right
- Code blocks: <code>, <kbd>, <var>
- Tables - row seperators, nice headers, striped rows, borders, hover
- Forms - clean labels, inline forms, placeholders, horizontal forms, validation

---
# Components
- Glyphs - icons
- Button groups
- Dropdown buttons
- Tabs
- Breadcrumbs
- Pagination

---
# Explore Components
[http://getbootstrap.com/components/](http://getbootstrap.com/components/)

---
# JavaScript
- Rich set of interactive controls
- Transition
- Modals
- Dropdowns
- Tooltips

---
# Explore Bootstrap JS
[http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/)

---
# JavaScript Warning
- Be sure to use the Angular UI plugin to get components that play well with Angular
[http://angular-ui.github.io/bootstrap/](http://angular-ui.github.io/bootstrap/)
