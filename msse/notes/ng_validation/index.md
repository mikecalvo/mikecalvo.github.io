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
