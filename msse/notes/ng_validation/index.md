---
title: Web Application Development
layout: default
---

# Angular Validation
## Mike Calvo
## mike@citronellasoftware.com

---
# Angular Validation
- Leverages HTML 5 input field types
- Examples:

```
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
# Example Form With Validation

``` html
<form name="userForm" novalidate ng-submit="addUser(newUser)">
  <div>
    <input name="userName" required ng-model="newUser.name" placeholder="Name">
  </div>
  <div>
    <input name="userEmail" type="email" ng-model="newUser.email" placeholder="Email">
  </div>
  <div>
    <input type="checkbox" required> I agree to the terms and conditions
  </div>
  <button type="submit" ng-disabled="userForm.$invalid">OK</button>
</form>
```

---
# Form Validation Variables
$prisine - true if user has not interacted
$dirty - true if user has interacted
$valid - true if valid
$invalid - true if not valid
$error - detailed errors

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
- ng-valid-[key]: for each valid key added by $setValidity
- ng-invalid-[key]: for each invalid key added by $setValidity
- ng-pristine: the control hasn't been interacted with yet
- ng-dirty: the control has been interacted with
- ng-touched: the control has been blurred
- ng-untouched: the control hasn't been blurred

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

--
# Displaying messages
- Use the automatically added validation properties on the form fields to conditionally show/hide error messages
- Each validation has it's own property on the $error object

---
# Validation Message Examples

``` html
Name:
<input type="text" ng-model="user.name" name="uName" required="" />
<br />
<div ng-show="form.$submitted || form.uName.$touched">
  <div ng-show="form.uName.$error.required">Tell us your name.</div>
</div>

E-mail:
<input type="email" ng-model="user.email" name="uEmail" required="" />
<br />
<div ng-show="form.$submitted || form.uEmail.$touched">
  <span ng-show="form.uEmail.$error.required">Tell us your email.</span>
  <span ng-show="form.uEmail.$error.email">This is not a valid email.</span>
</div>
```

---
# Resetting Form States
- Any named form in view is available in the scope
- Call $setPristine() and $setUntouched() to return form to orignal state

---
# More on Form Validation
- Custom model update triggers
- Custom validation
[https://docs.angularjs.org/guide/forms](https://docs.angularjs.org/guide/forms)

---
# Add Validation to Muzic
- Use a form and validation attributes on view
- Create some styles to provide feedback
- Use REST to search for artist
- Create a Factory for Artist Angular Resource
- Add searching to the ArtistRestController

---
# Summary
- Angular validation leverages HTML 5 field validations
- Use CSS to define feedback styles for validation failures
- Choose how soon you want to provide feedback
- Create your own validation if necessary
- As always: validate on the server as well
