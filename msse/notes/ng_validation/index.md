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
<input required>
```

---
# Validation Requires a Form
- Even though the form won't get used to post the data
- Used to attach validation errors to the form
- Angular will watch the model backing the inputs
- Angular will apply styles to each input based on:
  - Validity
  - Interaction characteristics

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
- ng-pending: any $asyncValidators are unfulfilled

---
# Define Styles
- Report errors when you want how you want
- Only after they've touched a field?
- Always until it's valid?
- Red border?
- Red background?
- Alert icon?
- As they type?
