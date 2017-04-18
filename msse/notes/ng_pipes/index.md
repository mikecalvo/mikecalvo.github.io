---
title: Web Application Development
layout: default
---

# Angular Pipes
## Adam Keyser

---

# Pipes
- Transform data only for display
- Format data: currency, dates, numbers
- Declared in html

---
# Example Built-in Pipes:
- currency
- date
- json
- number
- uppercase
- lowercase

---

# Examples

``` javascript
let person.firstName = 'fred';

{{person.firstName | uppercase}} -> 'FRED'

```

---

# Using a Pipe
- Within data bindings:

``` html
<td class="text-right">{{ p.price | currency }}</td>
<td class="date">{{ sale.date | date:"MM-dd-yy" }}</td>
<td class="date">{{ p.price | number:0 }}</td>
```

---

# Date Pipe
- Supports logical formats
  - medium MMM d, y h:mm:s a
  - short M/d/yy h:mm a
  - fullDateEEE, MMM d, y
  - longDate MMM d, y
  - mediumDate MMM d, y
  - shortDate M/d/yy
  - mediumTime h:mm:ss a
  - shortTime h:mm a

---
# JSON Filter
- Good debugging tool
- Converts the filtered value into JSON

``` html
<div>{{order | json}}</div>
```

---

# Parameters

- Pipes can take 0.. N arguments
- Seperate them all with a `:`

---
# Chaining Pipes
- Use the | to chain filters together

`{{ birthday | date | uppercase}}`

---
# Filtering Collections
- Used with directives that operate on arrays (ng-repeat)
- Example: slice
  - Constrains the array to only n items

``` html
<tr ng-repeat="p in products | slice:0:20">
```

---
# Creating Your Own Pipe
- Metadata `@Pipe` defines pipe name for template expressions
- Implement the `PipeTransform` interface
  - `tranform` method takes initial value plus any arguments
- Return a function that accepts a value and returns the modified result


---
#Custom Pipe Example
``` javascript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'labelCase'
})
export class LabelCasePipe implements PipeTransform {

  transform(firstName: string, lastName?: string): string {
    return LabelCasePipe.nameFormat(firstName)  +  (lastName ? ", " + LabelCasePipe.nameFormat(lastName) : "");
  }

  static nameFormat(value: string) : string {
    let upper = value.toUpperCase();
    let lower = value.toLowerCase();
    return upper[0] + lower.substr(1);
  }
}

```

---

#Pipe change detection

 - Pipe events don't react to every dom event
 - For example - mutating arrays
 - Can be resolved by replacing the array with a new array

---

#Pure vs impure Pipes

- Pure by default
  - Change detection only on primitive values or new objects
- Impure
  - Change detection on every component change detection cycle
  - Can cause performance issues

---

#Impure Pipes

```
@Pipe({
  name: 'myExpensivePipe',
  pure: false
})
```

---

#Async Pipes

- Used in conjuction with Observables, Promises
- When piped - subscribes to observable and updates for every value emitted
- Can be used for time interval user input - like debounce for typeahead or searches

---

#Testing pipes

- Can test output in component tests by using query selectors
- Custom pipes can be tested in isolation by testing the transform method

```javascript
it('transforms "abc" to "Abc"', () => {
    let pipe = new LabelCasePipe();
    expect(pipe.transform('NAME')).toBe('Name');
  });
```

---

# Summary
- Angular is strongly opinionated on where data transformation for display should happen
- Do it in pipes
- Pipes can accept arguments and use other pipes
- Pipes can operate on single values and collections
