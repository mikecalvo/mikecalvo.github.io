---
title: Web Application Development
layout: default
---
slidenumbers: true

## Angular Overview

### Marc Kapke
#### kapkema@gmail.com

---

# Angular Provides
- Opinionated approach to building single-page web apps
- Small number of constructs, roles of each well-defined
- 2-way binding
- Modular, component-based architecture
- [https://angular.io/docs/ts/latest/guide/](https://angular.io/docs/ts/latest/guide/)

---

# Why Single Page apps (SPA)
- Native-like app experience
- Responsive - navigate without reloads
- Rich interaction using HTML, CSS and JavaScript
- Serve mobile and desktop users

---

# Angular SPA architecture

![inline](https://angular.io/resources/images/devguide/architecture/overview2.png)

---

# Angular Style guide
- Provides opinion on syntax, conventions and structure
- [angular.io/styleguide](https://angular.io/styleguide)
- Great set of principles to follow

---

# Angular - Core constructs

- Modules
- Components
- Templates
- Metadata
- Data binding
- Directives
- Services
- Dependency Injection

---

# Modules
- Angular apps are modular
- Every angular app has only one **root module**
- App logic broken up into many **feature modules**
  - Organize modules by domain, workflow, or related capabilities
- Every module is a class with the `@NgModule` decorator

---

# Decorators
- Decorators are functions that modify a javascript class
- Used to attach metadata to classes

``` javascript
  function magic(target) {
    target.isMagic = true;
  }

  @magic
  class MyClass() {}

  console.log(MyClass.isMagic); //true;
```

---

# `@NgModule`
- Module is defined as class with `@NgModule` decorator function provided
- `@NgModule` property set
  - `declarations` - view classes that belong to module
  - `exports` - subset of declarations visible to __other__ modules
  - `imports` - dependent modules
  - `providers` -  services this module contributes to global collection accessible by app
  - `bootstrap` - the main application view; the root component. Only root module should set this

---

# Module example

``` javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
@NgModule({
  imports:      [ BrowserModule ],
  providers:    [ Logger ],
  declarations: [ AppComponent ],
  exports:      [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```
---

# Entry point

- bootstrapping and platform logic in `main.ts`
- Defines what module is the root module

``` javascript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';

platformBrowserDynamic().bootstrapModule(AppModule)
  .then(success => console.log(`Bootstrap success`))
  .catch(err => console.error(err));
```

---

# Components
- Controls a part of the screen - called a __view__
- Enables the user experience
- One to many components can be loaded at any time
- Example Components: Header, Footer, List, or Detail view

---

# Components
- Presents properties and methods for data binding
- Delegates everything else to services.
- Angular creates, updates, destroys components when needed.
- Use lifecycle hooks (like `ngOnInit()`) to take action

---

# Component example

``` javascript

export class ArticleListComponent implements OnInit {
  articles: Article[];
  selectedArticle: Article;

  constructor(private service: ArticleService) { }

  ngOnInit() {
    this.articles = this.service.getArticles();
  }

  selectArticle(article: Article) { this.selectedArticle = article; }
}

```

---

# Templates

- A template is the component's view.
- Tells angular how to render component
- All HTML syntax is valid
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

# Metadata
- Metadata is the glue that links components and views
- Use `@Component` director
  - `selector` - name of custom html tag assigned to component
  - `templateUrl` - file path to view defined for component
  - `providers` - dependencies that need injecting for this component

---

# Metadata example

```javascript
@Component({
  selector:    'article-list',
  templateUrl: './article-list.component.html',
  providers:  [ ArticleService ]
})
export class ArticleListComponent implements OnInit {
/* . . . */
}
```

---

# Data binding
- Mechanism for coordinating parts of a template with parts of a component
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

# Directives
- When templates are rendered, a directive transforms the DOM
- Directives are classes with `@Directive` decorator.
- Angular provides several, can write custom directives
- Two main directive types - structural and attribute.

---

# Directives - structural
- Structural directives alter layout.
- Add, remove, or replace elements in the DOM.

``` javascript
<li *ngFor="let article of articles"></li>
<article-detail *ngIf="selectedArticle"></article-detail>
```

- `*ngFor` adds an `<li>` tag for every article in article List
- `*ngIf` only includes `<article-detail>` if `selectedArticle` is non-null

---

# Directives - attribute
- Attribute directives alter the appearance or behavior of an existing element.
- `ngModel` is an attribute directive.
- `<input [(ngModel)]="article.title">`
- `ngModel` is modifying the `<input>` tags behavior by setting it's display value and responding to change events.

---

# Services
- A class with a narrow, well-defined purpose.
- Do one thing, do it well.
- Examples: Logging service, data service, etc..
- Nothing Angular specific about a service class

---

# Services
- Components are consumers of services
- Components should delegate logic to services.
- Services consume services.

---

# Services - basic logger

``` javascript
export class Logger {
  log(msg: any)   { console.log(msg); }
  error(msg: any) { console.error(msg); }
  warn(msg: any)  { console.warn(msg); }
}
```

---

# Services -

``` javascript
export class ArticleService {
  private articles: Article[] = [];

  constructor(
    private backend: BackendService,
    private logger: Logger) { }

  getArticles() {
    this.backend.getAll(Article).then( (articles: Article[]) => {
      this.logger.log(`Fetched ${articles.length} articles.`);
      this.articles.push(...articles); // fill cache
    });
    return this.articles;
  }
}
```

---

# Dependency Injection
- Supply a new instance of a class with fully-formed dependencies
- Most dependencies are services
- Dependency injection provides new components with services they need
- A components constructor determines what dependencies it needs
  - `constructor(private service: ArticleService) { }`

---

# Injector
- Angular asks injector for services a component requires.
- Injector maintains service instances previously created.
- If a requested service instance is not available, injector makes one and adds it
- Once all required services are resolved, calls component constructor with those services.

---

# Providers
- If the injector doesn't have a service, it uses a provider to create one.
- Providers can be registered in Modules or Components.
- In general, add providers to root module, so same instance is available everywhere
- Component level providers make a new instance for every instance of that component.

---

# Example Providers

``` typescript
@NgModule({
  imports:      [ BrowserModule ],
  providers:    [ Logger, ArticleService ],
  declarations: [ AppComponent ],
  exports:      [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
```
