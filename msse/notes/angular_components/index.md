---
title: Web Application Development
layout: default
---
slidenumbers: true

## Angular Components

### Marc Kapke
#### kapkema@gmail.com

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

# Component lifecycle
![inline](https://angular.io/resources/images/devguide/lifecycle-hooks/hooks-in-sequence.png)

---

# Lifecycle hooks
- Angular creates, updates and destroys components
- Implement lifecycle hook interface to access these events
- One event per interface, implement multiple if need be

---

# ngOnChanges hook
- Called before init
- Also called on every data bind change
- Use to respond to data bind events

---

# ngOnInit hook
- Called once when component is initialized
- Called after the first ngOnChanges()
- Use this to initialize component state

---

# ngOnDestroy
- Cleanup before angular removes component from DOM
- Unsubscribe from observables, detach event handlers, etc..
- Use to detach and prevent memory leaks

---

# Metadata
- Metadata is the glue that links components and views
- Use `@Component` director
  - `selector` - name of custom html tag assigned to component
  - `templateUrl` - file path to view defined for component
  - `providers` - dependencies that need injecting for this component

---

# Component example

``` javascript
@Component({
  selector: 'app-article-detail',
  templateUrl: './article-detail-simple.component.html',
  styleUrls: ['./article-detail-simple.component.css']
})
export class ArticleComponent implements OnInit {

  title = "article page";

  constructor(
    private articleService: ArticleService,
    public article: Article) {
  }

  ngOnInit() {
    this.articleService.getArticle("food", 7, 1).subscribe(
      article => this.article = article
    )
  }
}

```

---

# Testing a component
- Mock out any dependencies
- Configure TestModule harness
- TestModule declarations should contain only component under test
- Setup TestModule providers if needed
- Create fixture and component
- Execute tests

---

# Example test setup

``` javascript
describe('ArticleDetailSimpleComponent', () => {
  let component: ArticleComponent;
  let fixture: ComponentFixture<ArticleComponent>;

  beforeEach(async(() => {
     let articleServiceStub = new ArticleService(null);
     TestBed.configureTestingModule({
       imports: [],
       declarations: [ArticleComponent],
       providers: [{provide: ArticleService, useValue: articleServiceStub}, Article],
     })
       .compileComponents();
   }));

   beforeEach(() => {
     fixture = TestBed.createComponent(ArticleComponent);

     let articleService = fixture.debugElement.injector.get(ArticleService);
     spyOn(articleService, 'getArticle').and.returnValue(Observable.of(new Article().id=500));

     component = fixture.componentInstance;
     fixture.detectChanges();
   });
  //tests go here  
});
```

---


# First `beforeEach(async())`
- The first beforeEach uses `async()`
- `async()` is test method provided by Angular to execute in test async 'zone'
- compileComponents needs to compile all components asynchronously

---

# Second 'beforeEach'
- Synchronous test Setup
- happens only after async setup completes
- This is where fixture and component are created.


---

# Example tests

``` javascript
it('should create', () => {
  expect(component).toBeTruthy();
});

it('should have article in model on init', () => {
  expect(component.article).toEqual(new Article().id = 500)
});

it('should have title in model ', () => {
  expect(component.title).toEqual("article page")
});

it('should have title in dom', () => {
  const compiled = fixture.debugElement.nativeElement;
  let title = compiled.querySelector("p");
  expect(title.textContent).toContain(component.title);
});
```
