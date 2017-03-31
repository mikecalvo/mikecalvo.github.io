---
title: Web Application Development
layout: default
---
slidenumbers: true

## Angular Templates

### Marc Kapke
#### kapkema@gmail.com
---

# Templates

- A template is the component's view.
- Tells angular how to render component
- Angular template syntax provides data binding, conditional logic, etc..
- Templates can include other other components

---

# Template example

``` html
<h2>Article List</h2>
<p><i>Pick an article from the list</i></p>
<ul>
  <li *ngFor="let article of articles" (click)="selectArticle(article)">
    {{article.title}}
  </li>
</ul>
<article-detail *ngIf="selectedArticle" [article]="selectedArticle"></article-detail>

```

---

# Template syntax
- All Html is valid
- Extend Html with Angular specific directives

---

# Template expressions
- Expression executed and assigned to a binding target
- `{{1+1}}` uses Interpolation to bind the result of `1+1` template expression
- Expression are Javascript like
- Some Javascript expressions are excluded (assignment, new, chaining, bitwise operators)
- Includes template expression operators

---

# Expression context
- Expression context is almost always the component instance
- `{{title}}` means bind the value of the title property from the current component

---

# Expression operators
- The pipe operator `|`
 - Pass expression result through one or more transformers
 - `{{title | uppercase}}`
- The safe navigation operator (?)
  - fluent null check just like Groovy
  - `{currentArticle?.title}`

---

# Expression guidelines
- Expressions should NOT modify any application state other then the target property
- Should evaluate quickly
- Keep expressions simple.
  - If binding requires logic, put it in a method and call it

---

# Data binding refresher
- Binding markup in template defines how to connect both sides.
- Four forms of data binding
- Each form has a direction - to the DOM, from the DOM, or in both directions

---

# Interpolation
- Binds to the DOM from the component
- `<li>{{article.title}}</li>`
- Interpolation `{{ }}` displays the component's `article.title` property within the `<li>` element

---

# Property Binding
- Binds to the DOM from the component
- `<article-detail [article]="selectedArticle"></article-detail>`
- Passes the `selectedArticle` from the `ArticleListComponent` to the `article` property of the child `ArticleDetailComponent`
- When setting an element property to a non-string data value, you must use property binding.

---

# Event Binding
- From the DOM to the component
- `<li (click)="selectArticle(article)"></li>`
- The `(click)` event binding calls the components `selectArticle` function when an article is clicked.

---

# Two way Binding
- Combines Property and Event binding in a single notation.
- Data property flows to the DOM from the Component as a property binding.
- User's changes flow back to the component - resetting the property to the latest value.
- `<input [(ngModel)]="article.title">`
- `ngModel` is a special directive that maps user changes back to component

---

# Additional Binding scenarios
- Less common but useful
  - Attribute binding
  - Class binding
  - Style binding

---

# Attribute Binding
- One way bindings
- Use when there is no element property to bind.

``` html
<table border=1>
  <!--  expression calculates colspan=2 -->
  <tr><td [attr.colspan]="1 + 1">One-Two</td></tr>

  <!-- ERROR: There is no `colspan` property to set!
    <tr><td colspan="{{1 + 1}}">Three-Four</td></tr>
  -->

  <tr><td>Five</td><td>Six</td></tr>
</table>
```

---

# Class binding
- Add and remove css class names dynamically
- Resembles property binding.

``` html
<!-- reset/override all class names with a binding  -->
<div class="bad curly special"
     [class]="badCurly">Bad curly</div>
```

---

# Class binding - toggle
- Instead of overriding the whole class hierarchy
- Toggle on or off specific class names

``` html
<!-- toggle the "special" class on/off with a property -->
<div [class.special]="isSpecial">The class binding is special</div>

<!-- binding to `class.special` trumps the class attribute -->
<div class="special"
     [class.special]="!isSpecial">This one is not so special</div>
```

---

# Style binding
- Set inline styles dynamically
- Resembles property binding.
- `[style.style-property]`
- Dash or camelCase style-property names are valid

``` html
<button [style.font-size.em]="isSpecial ? 3 : 1" >Big</button>
<button [style.font-size.%]="!isSpecial ? 150 : 50" >Small</button>

```

---

# Built-in Attribute directives
- Attribute directives listen to and modify the behavior of HTML elements.
- Applied like attributes.
- Most common are
  - NgClass - add and remove set of classes
  - NgStyle - and remove set of inline styles
  - NgModel - two way data-binding

---

# NgClass
- NgClass accepts map of cssClassName:[true|false]

``` javascript
// CSS classes: added/removed per current state of component properties
  this.currentClasses =  {
    saveable: this.canSave,
    modified: !this.isUnchanged,
    special:  this.isSpecial
  };
```

``` html
<div [ngClass]="currentClasses">Some div</div>
```

---

# NgStyle
- NgStyle sets multiple inline stlyes dynamically.
- Similar format to NgClass
- Use map of key/values to control styles
- Keys are css 3 style names

``` javascript
// CSS styles: set per current state of component properties
  this.currentStyles = {
    'font-style':  this.canSave      ? 'italic' : 'normal',
    'font-weight': !this.isUnchanged ? 'bold'   : 'normal',
    'font-size':   this.isSpecial    ? '24px'   : '12px'
  };
```

``` html
<div [ngStyle]="currentStyles">
  This div is initially italic, normal weight, and extra large (24px).
</div>
```

---

# ngModel
- NgModel provides two way data binding
- Requires FormModule be imported
- Encapsulates property binding and event binding (onChange)

```html
<input [(ngModel)]="currentHero.name">
```

---

# ngModel
- Can declare property and event binding separately
- Useful if you need to intercept a changed value

``` html
<input
  [ngModel]="currentHero.name"
  (ngModelChange)="setUppercaseName($event)">
```

---

# Built-in Structural directives
- Structural directives add, remove or manipulate elements in the DOM.
  - `*ngIF` - conditionally add or remove from the DOM
  - `*ngFor` - repeat a template

---

# `*ngIf`
- Conditionally add/remove from DOM
- Show/Hide is different - elements remain in

``` html
<div *ngIf="isActive">Am I in the Dom?</div>
```

``` html
<!-- show/hide -->
<div [style.display]="isSpecial ? 'block' : 'none'">Show with style</div>
<div [style.display]="isSpecial ? 'none'  : 'block'">Hide with style</div>
```

---
# `*ngFor`
- Repeater directive
- Presents a list of items
- each item has properties - index, first, last, even, odd

``` html
<div *ngFor="let article of articles; let i=index">Article: {{i}} - {{article.title}}</div>
```
---

# Input properties
- Properties that can be bound as a binding target
- Targets are the left hand side of binding expression
- Every member of a `source` component is available for binding
- Must declare what is available to bind as a target

---

# Target property declaration

``` javascript
@Component({
  inputs: ['article'],
})
```

```html
<article-detail [article]="selectedArticle">
```

---

# Input properties
- Input properties usually receive data values.
- 'article' is an Input from the perspective of the ArticleDetailComponent
- Data flows into that property from a binding expression
