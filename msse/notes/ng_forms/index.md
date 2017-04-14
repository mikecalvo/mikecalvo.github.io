---
title: Web Application Development
layout: default
---

## Angular Template Forms & Validation
### Adam Keyser

---

# Forms

- Main mechanism for data entry on the web
- User Registration
- Contact Us
- Facebook

---

# Main Components

- Input
- Data binding
- Change tracking
- Validation
- Error handling

---

# Implementation

- Html5 for inputs/errors/validation
- CSS + Angular for errors/success
- Angular for data binding

---

# Angular Validation
- Leverages HTML 5 input field types
- Examples:

``` html
<input type="date">
<input type="number">
<input type="email">
<input type="url">
<input type="checkbox" required>
```

---

# Other Angular Validation Directives
- pattern
- minlength
- maxlength
- min
- max

---

# Validation Requires a Form
- Even though the form won't get used to post the data
- Used to attach validation errors to the form
- Angular will watch the model backing the inputs
- Angular will apply styles to each input based on:
  - Validity
  - Interaction characteristics
  - Must turn off browser validation (novalidate)

---

# Template Reference Variables (`#value`)

- Can refer to an element, component, or directive - depending on context
- Useful in forms for referencing specific input fields and for referencing the form
- Don't declare more than one with the same name - global to the template

---

# Examples

``` html

<input type="text" id="firstName"  #firstName="ngModel"/>

<form name="form" #f="ngForm">
  ...
</form>

```
---

# Two Way Binding [(x)] (banana-box syntax)

- Property binding and event change combined into one

``` groovy
  [ngModel]="model.firstName"
  (ngModelChange)="model.firstName=$event">
```

- Becomes:
`[(ngModel)]="model.firstName"`

---

# NgModel

- Directive that binds a FormControl instance to a domain model
- Tracks value, user interaction, and validation status
- When in the context of `<form>` will register as a child of the form using the name of the `name` attribute

---

# Example Form With Validation

``` html

<form name="form" (ngSubmit)="f.form.valid && saveUser()" #f="ngForm">
  <div class="form-group">
    <label for="firstName">First Name</label>
    <input type="text" id="firstName" class="form-control" name="firstName"
      [(ngModel)]="model.firstName" #firstName="ngModel" required/>
  </div>
  <div class="form-group">
    <button class="btn btn-primary">Register</button>
    <a [routerLink]="['/users']" class="btn btn-link">Cancel</a>
  </div>
</form>

```

---

# Notes about NgModel and Binding

- Banana box binding not required if submitting the form values

```javascript
@Component({
  selector: 'example-app',
  template: `
    <form #f="ngForm" (ngSubmit)="onSubmit(f)" novalidate>
      <input name="first" ngModel required #first="ngModel">
      <input name="last" ngModel>
      <button>Submit</button>
    </form>
    <p>First name value: {{ first.value }}</p>
    <p>First name valid: {{ first.valid }}</p>
    <p>Form value: {{ f.value | json }}</p>
    <p>Form valid: {{ f.valid }}</p>
  `,
})
export class SimpleFormComp {
  onSubmit(f: NgForm) {
    console.log(f.value);  // { first: '', last: '' }
    console.log(f.valid);  // false
  }
}
```

---

# Form Validation Variables
- prisine - true if user has not interacted
- dirty - true if user has interacted
- valid - true if valid
- invalid - true if not valid
- errors - detailed errors

---

# Providing Feedback For Validation
- Angular automatically applies CSS classes based on the status of the form inputs
- Match concepts in the form
  - valid/invalid & dirty/pristene
- Define CSS to match these classes based on the feedback desired

---

# Angular Validation CSS Classes
- ng-valid: the model is valid
- ng-invalid: the model is invalid
- ng-pristine: the control hasn't been interacted with yet
- ng-dirty: the control has been interacted with
- ng-touched: the control has been blurred
- ng-untouched: the control hasn't been blurred

---

# Debugging Classes

- Useful to use interpolation to print the current classes associated with an input

``` html
<input [(ngModel)]="model.firstName" name="firstName" #firstNameClass>
{{firstNameClass.className}}
```

---

# Define Styles
- Report errors when you want how you want
- Only after they've touched a field?
- Always until it's valid?
- Red border?
- Red background?
- Alert icon?
- As they type?

---

# Example Styles

``` css
form .ng-invalid.ng-dirty { background-color: lightpink; }
form .ng-valid.ng-dirty { background-color: lightgreen; }
span.summary.ng-invalid { color: red; font-weight: bold; }
span.summary.ng-valid { color: green; }
```

---

# Displaying messages
- Use the automatically added validation properties on the form fields to conditionally show/hide error messages
- Each validation has it's own property on the template reference

---

# Validation Message Examples

``` html

<div class="form-group" [ngClass]="{ 'has-error': f.submitted && !firstName.valid }">
      <label for="firstName">First Name</label>
      <input type="text" id="firstName" class="form-control"
        name="firstName" [(ngModel)]="model.firstName" #firstName="ngModel" required/>
      <div [hidden]="!f.submitted && firstName.valid"
           class="alert alert-danger">
        First name is required
      </div>
</div>

```

---

# Resetting Form State

- Any form can be reset by calling reset() (using template reference)
- Individual form controls can be reset by calling markAsPristine on control

---

# Custom Validation
- Define a directive
- Implement the Validator interface
- Add a new selector matching the name of the validator used on the input field
- Create a validation function that returns null if the control is valid, or errors if not

---

# Custom Validation Example

``` javascript

export function BirthdayValidator(): ValidatorFn {
  return (control: AbstractControl): {[key: string]: any} => {
    const regex = /[0-2][0-9][0-9][0-9]-[0-1][0-9]-[0-3][0-9]/g;
    const date = control.value;
    if (!date) {
      return null
    }
    const worked = regex.test(date);


    return worked ? null : {'birthday' : 'value must be in yyyy-mm-dd'}
  };
}

```

---

# Notes about the template form approach

- Asynchronous change model
- Exceedingly hard to unit test
- Opportunity for mistakes
- Not recommended?

---

# Summary
- Angular validation leverages HTML 5 field validations
- Use CSS to define feedback styles for validation failures
- Choose how soon you want to provide feedback
- Create your own validation if necessary
- As always: validate on the server as well
