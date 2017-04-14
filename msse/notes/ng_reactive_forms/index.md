---
title: Web Application Development
layout: default
---

## Angular Reactive Forms & Validation
### Adam Keyser

---

# Reactive Forms

- Alternative to template driven forms
- Explicit data management
- Moves logic out of html, into the Component

---

# Benefits of Reactive Forms

- Synchronous validation
- Easier to test
- Separation of logic makes it easier to understand what's happening

---

# Components of Reactive Forms

- AbstractControl - abstract class for the following
- FormControl
- FormGroup
- FormArray
- FormBuilder

---

# FormControl

- Tracks value and validity of individual control

``` javascript
const ctrl = new FormControl('potential intital value', Validators.maxLength(100));
ctrl.reset() // Resets
ctrl.valid //Is the control valid
ctrl.pristine //Pristine?
ctrl.value //Current value
ctrl.setValue('new value') // Set new value
```

---

# FormGroup

- Tracks value and validity of a group of AbstractControl instances

``` javascript
const form = new FormGroup({
  first: new FormControl('Nancy', Validators.minLength(2)),
  last: new FormControl('Drew'),
});

form.valid //validity
form.status // "VALID or INVALID"
form.value //form data
form.reset() // Resets
```

---

# FormArray

- Tracks value and validity of an array of FormGroup instances
- Generally used for Lists of data (Phone Numbers, Addresses, Comments)
- Methods executed on the FormArray will propagate down to all child FormGroups

---

# FormBuilder

- Injectable Control meant to simplify form creation
- Reduces boilerplate of `new FormGroup()` `new FormControl()` ...
- Cleaned up code is much easier to realDeal

---

# Example

``` javascript

constructor(fb: FormBuilder, private userService: UserService, private router: Router) {
        this.fb = fb;
        this.registerForm = fb.group({
            firstName: ['', Validators.required],
            lastName: ['', Validators.required],
            birthday: ['', Validators.compose([birthdayValidator(), Validators.required])],
            addresses: this.fb.array([])
        });
    }

```

---

# Testing Reactive Forms

- Synchronous model
- Significantly easier to test
- Separation of concerns makes it easy to understand without significant poking around in html
