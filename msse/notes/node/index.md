---
title: Web Application Development
layout: default
---

# Node.js
## Mike Calvo
## mike@citronellasoftware.com

---

# Node.Js
- Platform based on Chrome's JS Runtime
- Write JavaScript code as applications (outside browser)
- Examples: server code and build tools

---

# Installing Node
- [http://nodejs.org/download/](http://nodejs.org/download/)
- Installers for Mac/Windows/Linux
- Confirm at command line: `node --version`

---

# Interactive Console
- Type `node` at command line
- Interactive JavaScript console
- Good for testing simple syntax, etc.

---

# Node Basics
- Node supports modular JavaScript application development
- Designed to be a self-standing (non-browser) platform
- Require - bring in other packages or files into your program
- Single-threaded event loop
- Core set of libraries

---

# Node is Callback/Promised-based
- Most functions allow for specifying a function to call when action is completed
- Callback - single function, usually passed directly into another function
- Promise
  - Interface returned from a function
  - then() takes a success callback and error callback
  - When a promise is fulfilled, the then success callback is invoked
  - When a promise fails, the error callback is invoked

---

# Example Node Libraries
- http: serving up and HTTP server and making HTTP requests
- fs: interacting with the file system
- os: operating system-specific functionality
- crypto: cryptography
- events: publish/subscribe

---

# Example Node Web Server

``` javascript
var http = require('http');

http.createServer(function(request, response) {
  response.writeHead(200, {'Content-Type': 'text/plain'});
  response.end('Hello World\n');
}).listen(8124);

console.log('Server running at http://127.0.0.1:8124/');
```

---

# Running a Node Application
`node <program_file.js>`

- Required dependencies must live relative to the program
- Node dependencies: Packages

---

# Node Package Manager (NPM)
- Node supports a package model for installing utilities and Node-based tools
- Part of node installation
- Usage: `npm install <package_name>`
- Global versus Local installation
  - Global: added to system
  - Local: creates a directory called node_modules in current directory

---

# Package
- Node module
- Directory with one or more files
- package.json file: list dependencies
- See all packages at [npmjs.com](http://www.npmjs.com)

---

# package.json
- Define which node modules you depend on
- Initialize the file:
  `npm init -f`
- When installing packages:
  `npm install bower --save`

---

# Example package.json file

``` javascript
{
  "dependencies": {
    "lodash": "^2.4.1",
    "tap": "*"
  }
}

```
---

# Bower
- Node-based package manager for web projects
- Manages external JavaScript and CSS dependencies
- Installing Bower:
  - In your web-app folder: `npm install bower --save`

---

# Use Bower
`bower install jquery`
`bower install git://github.com/user/package.git`
`bower install bootsrap`

---

# bower.json
- Much like Node - bower can use a configuration file to list dependencies
`bower init` -> bower.json

``` javascript

{
  "dependencies": {
    "angular": "~1.3.11"
  }
}

```

---

# Bower Packages

- Bootstrap
- Angular
- jQuery
- Less
- [http://bower.io/search/](http://bower.io/search/)

---

# Using the local Bower
- By default npm installs packages into node_modules/package_name
- Node packages that contain scripts are placed into node_modules/.bin
- Run bower from here to get installed version for project
`node_modules/.bin/bower install angular`
