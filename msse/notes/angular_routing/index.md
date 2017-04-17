---
title: Web Application Development
layout: default
---
slidenumbers: true

## Angular Routing

### Marc Kapke
#### kapkema@gmail.com

---
# Angular Router

- Enables navigation from one view to the next
- Router maps browser url to corresponding route definition
- Brings browser behavior to Single Page App

---
# Enables SPA Browser behavior

- Interpret URL and navigate to generated view
- Pass parameters to view component.
- Bind links in view to navigate.
- Back/ Forward browser button support

---

# Router configuration
- App should have `<base href="/">` in root `index.html`
- Router has no routes by default - just root.
- Each route maps a URL path to a component.
- Order of routes matter. First one to match wins.
- Register route list into RouterModule

---

# Route path
- relative path - no leading slash
- absolute path - leading slash
- Route path parameters start with `:`
- Example: `articles/:id`

---

# Component Map definition
- Define a path.
- Define corresponding component

``` javascript

{path: 'articles/:category/:time', component: ArticleListComponent}

```

---

# Route data
- Use data property to store data associated with route.
- Accessible from within activated route component.
- Store items like page tile, breadcrumb text, etc...

``` javascript

{
  path: 'articles/:category/:time',
  component: ArticleListComponent,
  data: {title: 'NYT Articles'}
}

```

---

# Handle redirects
- Map to another path instead of a component
- Specify pathMatch scheme. `full` or `prefix`
- full - results in match when the remaining URL matches entirely
- prefix - results in match when remaining URL begins with path

``` javascript

{ path: '',   redirectTo: '/articles', pathMatch: 'full' }

```

---

# Wildcard route
- Define failure case using wildcard route.
- Specified by path of `**`
- When the router finds no matches - routes to this definition
- Define a Page not Found component or redirect to different route

``` javascript

{ path: '**', component: PageNotFoundComponent }

```

---

# Routes definition

``` javascript

const routes: Routes = [
  {
    path: 'articles/:category/:time/:id',
    component: ArticleDetailComponent,
    resolve: {
      article: ArticleDetailResolverService
    }
  },
  {
    path: 'articles/:category/:time',
    component: ArticleListComponent,
    resolve: {
      articleList: ArticleListResolverService
    }
  },
  {
   path: 'about',
   component: AboutComponent
 },
 {
   path: '',
   redirectTo: 'articles/food/7',
   pathMatch: 'full'
 },
 { path: '**', component: PageNotFoundComponent }
];

```

---

# Import routes into app
- Can have routes defined in any module
- Route module uses `RootModule.forRoot(routes)` to import
- Sub-modules use `RouterModule.forChild(routes)`
- Export RouterModule so it can be consumed

---

# Root Module example

```
@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes)
    // other imports here
  ],
  ...
})
export class AppModule { }
```

---

# Sub-module Example

```
@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule],
  providers: [ArticleDetailResolverService, ArticleListResolverService]
})
export class ArticleRoutingModule {
}
```

---

# Router outlet
- Directive that marks the spot where router will inject routed component
- Specified with `<router-outlet></router-outlet>`
- Router will inject component immediately after `router-outlet`
- Usually `router-outlet` goes in root component template

---

# Named router outlets
- Can have multiple `router-outlet` tags in app
- Specify outlet by name in route definition

``` html
<router-outlet></router-outlet>
<router-outlet name="popup"></router-outlet>
```

``` javascript
{
  path: 'contact',
  component: ContactComponent,
  outlet: 'popup'
},
```

---

# Router navigation
- Use `routerLink` directive on anchor tags.
- `routerLinkActive` directive helps visually distinguish active route.
- `routerLinkActive` value is a space delimited list of CSS classes - `active` for Example
- `<a routerLink="/articles" routerLinkActive="active">Articles</a>`

---

# Simple Routable app template

``` html
<h1>Nyt Articles</h1>
  <nav>
    <a routerLink="/articles" routerLinkActive="active">Articles</a>
    <a routerLink="/about" routerLinkActive="active">About</a>
  </nav>
  <router-outlet></router-outlet>
```

---

# Router Navigation with parameters
- Use component to handle complex Routing

``` javascript
onSelect(artice: Article) {
    this.router.navigate(['/article', article.id]);
}
```
---

# ActivatedRoute
- ActivatedRoute is injectable service
- Contains all relevant route information
- `url`, `data`, `params`, `queryParams`, etc...

---

# Route guards
- Add guards to route configuration to handle:
  - Unauthorized Access
  - Authenticate first then continue
  - Fetch some data prior to displaying target component
  - Save or discard pending changes before leaving view

---

# Guard
- guard return value dictates router behavior
  - If `true`, navigation process continues
  - If `false`, navigation process stops
- can have multiple guards at entry
- defined as Injectable services

---

# Guard types
- `CanActivate` - mediate navigation to a route
- `CanActivateChild` - mediate navigation to a child route
- `CanDeactivate` - mediate navigation away from current route
- `Resolve` - perform route data retrieval before route activation
- `CanLoad` - to mediate navigation to a feature module loaded asynchronously

---

# Example Resolve guard

``` javascript
@Injectable()
export class ArticleDetailResolverService implements Resolve<Article>{
  resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<Article> {
    let id = route.params['id'];
    let category = route.params['category'];
    let time = route.params['time'];
    return this.articleService.getArticle(category, time, id).first()
  }

  constructor(private articleService: ArticleService) { }

}
```

---

# Map Resolver to route

``` javascript
{
    path: 'articles/:category/:time/:id',
    component: ArticleDetailComponent,
    resolve: {
      article: ArticleDetailResolverService
    }
  },
```

---

# Subscribe to resolved Observable

``` javascript
@Component({
  selector: 'app-article-detail',
  templateUrl: 'article-detail.component.html',
  styleUrls: ['article-detail.component.css']
})
export class ArticleDetailComponent implements OnInit {

  constructor(private route: ActivatedRoute) {
  }

  private article: Article;

  ngOnInit() {
    this.route.data
      .subscribe((data: {article: Article}) => {
        this.article = data.article;
      });
  }

}
```
