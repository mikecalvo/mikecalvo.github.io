---
title: Web Application Development
layout: default
---
# Typescript

---

# Typescript

- Developed and maintained by Microsoft
- First release: 2012
- Superset of Javascript

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

---

# Optional typing

- Can detect bad typing
- Can define strictness in compiler config for typescript (tsconfig.json)
  - Will still generate javascript - but will give IDE errors

---

# Defining types

- Colon (:) after type to be define

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
