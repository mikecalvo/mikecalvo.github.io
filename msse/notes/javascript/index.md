---
title: Web Application Development
layout: default
---

# Javascript &amp; TypeScript

## Mike Calvo

## mike@citronellasoftware.com

---

# Javascript
- The language of the web
- Written in two weeks by Brendan Eich (Netscape)
- Original name: LiveScript (1995)
- Once only for browsers - now found on servers (NodeJS)

---

# Language Features
- C-style syntax
- Dynamic typing (var instead of def)
- Objects: associative arrays + prototypes
- Functional: functions are objects

---

# Example Syntax: Variables

``` javascript
var x;
var y = 2;

x = 3.4;
x = false;

y = 'dynamic... ohhh.';
y = "Strings can be single or double quoted";

console.log("This is Javascript's version of println");
```

---

# Conditions, Arrays and Loops

``` javascript
var a = [1, 2, 3, 4, 5];

var sum = 0;
for (var i = 0; i < a.length; i++) {
  if (a[i] % 2) {
    sum += a[i];
  }
}
console.log('sum = '+sum);

```

---

# JavaScript Functions Syntax
- Functions are created with the 'function' literal
- Functions can be named - aids with debugging/stacks
- Functions can be assigned to a variable
- Functions can be passed as arguments (closures)

---

# Example Function Syntax

``` javascript
function print(parameter) {
  console.log('parameter = '+parameter);
}
print('yo');

// Optional function name assists debugging
var log = function log(words) {
  console.log('logging: '+words);
};

var forEach = function(array, fn) {
  for (var i = 0; i < array.length; i++) {
    fn(array[i]);
  }
};

forEach(['one', 'two', 'three'], log);

```

---

# Objects
- Name-value pairs
- Values can be functions
- Values can be other objects

---

# Object Examples

``` javascript
var emptyObject = {};
var person = { name: 'Mike', email: 'mjcalvo@gmail.com'};
console.log('Name: '+person.name);
console.log('Email: '+person.email);

person.address = { street: '123 Main', city: 'Lexington' };

console.log('City: '+person.address.city);

person['address']['city'] = 'London';

person.zip // !!! undefined !!!
```

---

# What are Prototypes?
- A base set of properties that are cloned/copied to form a new type
- It is not inheritance in the same way Groovy/Java/C# classes inherit from each other
- Every object has a prototype
- Prototypes can be modified at run time
- Use `new` or Object.create to extend another object

---

# Example Javascript Inheritance

``` javascript
var person = { name: 'Mike', age: 43 };
person.speak = function(words) { console.log('speaking: '+words); };

var employee = Object.create(person)
employee.work = function() { this.speak("I'm working..."); };

employee.work();
```

---

# Javascript Reflection

``` javascript
typeof 3           // 'number'
typeof 'status'    // 'string'
typeof {}          // 'object'
typeof {}.name     // 'undefined'
typeof {}.toString // 'function'

var obj = {name: 'name', count: 3};
obj.hasOwnProperty('name') // true
obj.hasOwnProperty('color') // false
```

---

# Looping Over Properties

``` javascript
var object = { name: 'a', date: new Date(), scores: [1, 2, 3] };
for (var p in object) {
  if (object.hasOwnProperty(p)) {
    console.log(p+': '+object[p]);
  }
}
```

---

# Deleting Properties

``` javascript
var object = [name: 'Mike', type: 'employee'];
delete object.type; // Now object == [name: 'Mike']

var array = ['a', 'b', 'c'];
delete array[1]; // ['a', undefined, 'c']
```

---

# Method Invocation Model
- Calling a function stored on an object
- Inside function 'this' refers to the instance on whom the method is called

``` javascript
var obj = {
  name: 'Billy',
  go: function() {
    this.name += ' going... ';
  }
};
```

---

# Plain Function Invocation
- Calling a function that is assigned to a variable or simply defined
- Global object is bound to this (boo!)

``` javascript
var obj = {
  go: function() {
    var that  = this;

    var helper = function() {
      that.value = 'nice hack!';
    }

    helper();
  }
};
```

---

# Constructor Invocation
- Calling a function with the 'new' keyword
- Not the same as calling a constructor in classic OO languages
- Returns a new Object which is a clone of the function's prototype
- Convention: constructor functions are Capitalized

---

# Example Constructor

``` javascript
var Person = function(name) {
  this.name = name;
}
Person.prototype.get_name = function() { return this.name; };

new Person('John').get_name();

```

---

# Apply Invocation
- Dynamically invoke a function
- Supply the arguments
- Supply the 'this'

---

# Apply Example

``` javascript
var add = function(a, b) {
  return a + b;
};

add.apply(null, [5, 6]);

```

---

# Arguments
- Javascript has flexible argument rules
- Callers of a function don't have to match the argument parameters exactly
- Non-supplied arguments are undefined
- Extra arguments are ignored
- All functions can access the complete set of arguments via the arguments value which is an array

---

# Arguments Example

``` javascript
var addAll = function() {
  var sum = 0;
  for (var i = 0; i < arguments.length; i++) {
    sum += arguments[i];
  }

  return sum;
}

addAll(2, 5, 6, 7, 10, 22);

```
---

# Augmenting Types
- Prototypes - even of system types - can be changed at run time
- String is missing the trim function...

``` javascript
String.prototype['trim'] = function() {
  // use a regular expression to remove whitespace
  return this.replace(/^\s+|\s+$/g, '');
};
"   hello    ".trim();
```

---

# Global Scope
- By default, Javascript puts variables in global scope
- Need tricks to avoid this:
- Trick 1: put all your variables within a single global variable
- Trick 2: declare variables within functions

---

# Function To Protect Scope

``` javascript
var object = (function() {
  var value = 5;  // Shhhh - this is private

  return {
    increment: function(amount) { value += amount; },
    getValue: function() { return value; }
  }
})();

object.increment(10);
object.getValue();
object.value; // !!! Undefined !!!
```

---

# Callbacks
- Common pattern in JavaScript
- Functions that run asynchronously get a function parameter
- This parameter is a callback
- When the asychronous execution completes, the callback is called

---

# Callback Example

``` javascript
sendMyRequest(url, function(status) {
  console.log('Request completed.  Status: '+status);
});
```

---

# Regular Expressions
- Core type in JavaScript
- /regex/
- Use test() to check for match

---

# RegEx Example

``` javascript
var parse_number = /^-?\d+(?:\.\d*)?(?:e[+\-]?\d+)?$/i

parse_number.test('1');           // true
parse_number.test('98.6');        // true
parse_number.test('123.45E-67');  // true
parse_number.test('192.168.1.1'); // false

```

---

# JavaScript in HTML
- Javascript code can exist within `<script>` tags
- Externally defined Javascript files can be included using `src` attribute
- Script tags can live in the head and the body
- Script tags are executed in order within the page
- JavaScript code has access to the HTML document (DOM)

---

# Example HTML Scripts

``` html
<!DOCTYPE html>
<html><head>
  <script src="angular.js"></script>
  <script>
    var app = {};
  </script>
  </head>
  <body>
    <script>
      document.body.innerHTML += '<div>hi</div>';
    </script>
  </body>
</html>
```

---

# Excellent JavaScript Books
- 'JavaScript: The Good Parts' by Crockford
- 'Eloquent JavaScript' by Haverbeke
- 'Functional JavaScript' by Fogus
- 'JavaScript Design Patterns' by Osmani

---

# Typescript

- Developed and maintained by Microsoft
- First release: 2012
- Superset of Javascript
- Created by Anders Hejlsberg (C#)

---

# Typescript is a Transpiler

- All Typescript is turned into Javascript to be run in the browser
  - Java -> bytecode
  - Typescript -> Javascript
- Allows functionality to be developed without waiting for browser support
  - Typescript already supports some EMCA 7 features

---

# Features

- Support for optional typing
  - Compile time type checking
- Inheritance
- Generics
- Classes
- Modules

---

# Optional typing

- Can detect bad typing
- Can define strictness in compiler config for typescript (tsconfig.json)
  - Will still generate javascript - but will give IDE errors

---

# Basic TypeScript types
- Boolean
- Number
- String
- Array
- Tuple
- Enum
- Any

---

# Type Assertions
- Casting or forcing a variable to a type

``` TypeScript
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
let anotherLength: number = (someValue as string).length;
```

---

# Defining types

- Variable level

```javascript
let greeting : string;
```

- Function level

```javascript
function sum(x : number, y : number) : number {
  return x + y;
};
```

---

# Type inference

```javascript
function sum(x, y : number) : number {
  return x + y;
};
```

- Typescript determines `x` has to be a number
- No error unless `"noImplicitAny": true`
- Changing to `x : string` produces an error

---
# Interfaces
- Abstract types
- Can have properties
  - Optional and readonly
- Can describe function signatures
  - Return types and parameters

---

# Interface Example

``` TypeScript

interface Person {
  firstName: string;
  middleName?: string;
  lastName: string;
  readonly age: number;
}

function greeter(person: Person) {
  return "Hello, "+person.firstName+" "+person.lastName;
}

document.body.innerHTML = greeter({firstName: 'Joe', lastName: 'Smith'});
```

---
# Function Type Example

``` TypeScript
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    let result = source.search(subString);
    return result > -1;
}
```

---
# TypeScript Classes
- User defined types
- Properties
- Methods
- Constructors
- Visibility: private, public, protected
- Inheritance
- Static

---

# Class Example

``` TypeScript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
```

---
# Class Accessors

``` TypeScript
class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}
```

---

# Modules
- Proper include functionality

greet.ts:

``` TypeScript
export function sayHello(name: string) {
    return `Hello from ${name}`;
}
```

main.ts:

``` TypeScript
import { sayHello } from "./greet";

console.log(sayHello("TypeScript"));
```

---

# Let, Var and Const
- Declare variables using `let`, `var`, and `const`
- var: Define a global variable
- let: block-scoped variable
- const: block-scoped constant variable

---

# Spread Operator
- Extract the values of an array

``` TypeScript
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];
// bothPlus = [0, 1, 2, 3, 4, 5]
```
