---
title: Web Application Development
layout: default
---

# Angular Bootstrap

## Marc Kapke

## kapkema@gmail.com

---

# Angular UI Bootstrap Module
- Angular Bootstrap provides a collection of rich web UI controls
  - Examples: carousel, accordion, typeahead, modals
- Controls built with Twitter bootstrap styling
- [https://ng-bootstrap.github.io/#/home](https://ng-bootstrap.github.io/#/home)
- The Angular UI module makes these available in an Angular friendly way


---

# Adding Angular UI Bootstrap To Project
1. Use npm to install ng-bootstrap, bootstrap@4.0.0-alpha.6, jquery, tether
1. Update `.angular-cli.json` to include bootstrap
1. Import `NgbModule.forRoot()` in root module
1. Import `NbgModule` into sub-modules

---

# Install Latest Version of Bootstrap and Angular UI
- From your root directory
- `./gradlew npm_install_--save_@ng-bootstrap/ng-bootstrap`
- `./gradlew npm_install_--save_bootstrap@4.0.0-alpha.6`
- `./gradlew npm_install_--save_jquery`
- `./gradlew npm_install_--save_tether`


---

# Update `.angular-cli` styles block

``` json
"styles": [
  "styles.css", "../../../node_modules/bootstrap/dist/css/bootstrap.min.css"
],

```

---

# Update App Module Imports

``` javascript
@NgModule({
  declarations: [
    AppComponent,
    AboutComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    AppRoutingModule,
    NgbModule.forRoot(),
    ArticleModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

---

# Carousel control
- Slideshow for cycling through a series of content
- Built with CSS 3D transforms and JavaScript.
- Can be a series of images, text, or custom markup.
- Includes support for previous/next controls and indicators.

---

# Carousel control

``` html
<div class="container" style="margin-top:5em;">
  <div class="row">
        <ngb-carousel class="col-12">
          <template ngbSlide *ngFor="let article of articles">
            <img class="carousel-image" [src]="getArticleImageUrl(article)" [alt]="getArticleImageCaption(article)">
            <div class="carousel-caption">
              <h3>{{article.title}}</h3>
              <p>{{article.abstract}}</p>
            </div>
          </template>
        </ngb-carousel>
  </div>
</div>
```

---

# Accordion control
- Displays collapsible content panels
- Used to present information in a limited amount of space.
- Click headers to expand/collapse content

---

# Accordion control

``` html
<div class="container" style="margin-top:5em;">
  <div class="row">
    <ngb-accordion [closeOthers]="true" activeIds="1" class="col-12">
      <ngb-panel [id]="i" [title]="article.title" *ngFor="let article of articles, let i=index">
        <template ngbPanelContent>
          <h3>{{article.byline}}</h3>
          {{article.abstract}}
          <img class="accordion-image" [src]="getArticleImageUrl(article)"
          [alt]="getArticleImageCaption(article)" (click)="open(article)">
        </template>
      </ngb-panel>
    </ngb-accordion>
  </div>
</div>
```

---

# Modals
- Modals are dialog prompts
- A number of use cases from user notification to completely custom content
- Helpful subcomponents, sizes, and more.

---

# Modal - view

``` html
<div class="modal-header">
  <h4 class="modal-title">{{article?.title}}</h4>
  <button type="button" class="close" aria-label="Close" (click)="activeModal.dismiss('Cross click')">
    <span aria-hidden="true">&times;</span>
  </button>
</div>
<div class="modal-body">
 <app-article-detail [article]="article" [showContent]="false"
 [showTitle]="false" style="margin-top:-5em;"></app-article-detail>
</div>
<div class="modal-footer">
  <button type="button" class="btn btn-secondary" (click)="activeModal.close('Close click')">Close</button>
</div>
```

---

# Modal - component

``` javascript
@Component({
  selector: 'app-article-detail-modal',
  templateUrl: 'article-detail-modal.component.html',
  styleUrls: ['article-detail-modal.component.css']
})
export class ArticleDetailModalComponent {

  @Input() article: Article;
  constructor(private activeModal: NgbActiveModal){

  }

}
```

---

# Modal - open

``` javascript
open(article: Article) {
  const modalRef = this.modalService.open(ArticleDetailModalComponent, {size:'lg'});
  modalRef.componentInstance.article = article;
}
```
---

# Typeahead
- AutoComplete behavior on input element
- Type to narrow down results
- Select from list of matching results

---

# Typeahead - view


``` html
<div class="container" style="margin-top:5em;">
  <div class="row">
    <div class="col-12">
      <label>Search for Article:</label>
      <input type="text" class="form-control" [(ngModel)]="article" [ngbTypeahead]="search" [resultFormatter]="formatter" [inputFormatter]="formatter"/>
      <hr>
    </div>
  </div>
  <div class="row">
    <app-article-detail [article]="article" *ngIf="article?.title != null" style="margin-top:-5em;"></app-article-detail>
  </div>
</div>

```

---

# Typeahead - component

``` javascript
export class ArticleListSearchComponent extends ArticleListComponent {
  article: Article;

  formatter = (result: Article) => result.title;
  search = (text$: Observable<string>) =>
    text$
      .debounceTime(200)
      .distinctUntilChanged()
      .map(term => term.length < 2 ? []
        : this.articles.filter(v => new RegExp(term, 'gi').test(v.title)).splice(0, 10));
}
```

---

# Dropdown
- Menu for displaying lists of links.
- Toggle menu open and closed

---

# Dropdown - view

``` html
<div ngbDropdown class="d-inline-block">
 <label>Category: </label>
 <button class="btn btn-outline-primary" id="dropdownMenu1"
 ngbDropdownToggle>{{selectedCategory}}</button>
 <div class="dropdown-menu" aria-labelledby="dropdownMenu1">
   <button *ngFor="let category of categories"
   class="dropdown-item" (click)="onCategoryChange(category)">{{category}}</button>
 </div>
</div>
```
