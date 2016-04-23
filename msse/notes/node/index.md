---
title: Web Application Development
layout: default
---

# Node and Bower

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
- http: serving up an HTTP server and making HTTP requests
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
  `npm install lodash --save`

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

# Example Setup Project with Node
1. In root project folder run:
  `npm init -f`
2. Answer all the questions
3. Install bower
  `npm install bower --save`
4. Verify your install
  `ls node_modules/.bin/bower`

---

# Bower
- Node-based package manager for web projects
- Manages external JavaScript and CSS dependencies
- Installing Bower:
  - In your grails-app folder: `npm install bower --save`

---

# Use Bower

``` bash
bower install jquery
bower install git://github.com/user/package.git
bower install bootstrap
```

---

# bower.json
- Much like Node - bower can use a configuration file to list dependencies
- `bower init` will produce a bower.json file

``` javascript

{
  "dependencies": {
    "angular": "~1.3.11"
  }
}

```

---

# Initialize Bower in Your Grails Project
1. From your project root:
  `node_modules/.bin/bower init`
2. Answer the questions
  - If you don't understand a question just press enter
  - Answer 'n' to add common ignores question

---

# Configure Bower
- Tell bower where to install dependencies
- Create a file called .bowerrc in your root project folder:

``` json
{
  "directory" : "grails-app/assets/bower"
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

# Use Bower to install jQuery and Angular
1. In your root project directory run:
  `node_modules/.bin/bower install jquery --save`
  `node_modules/.bin/bower install angular --save`
1. Verify your installs:
  `ls grails-app/assets/bower-components`
  `angular jquery`
1. Check the bower.json file to be sure in includes jquery and angular
