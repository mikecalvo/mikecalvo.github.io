---
title: Web Application Development
layout: default
---

# Alternatives

## Mike Calvo

## mike@citronellasoftware.com

---

# Server Side

---

# Defining the Monolith
- Development teams bundle many API endpoints into a single deployable artifact
- Typically defined by the struture of the team (one deploy per team)
- Simplified deployment and runtime service management
- Java: WAR file

---

# Problems with the Monolith
- Some endpoints called more frequently than others
- Some endpoints have different external constraints and dependencies
- Changes to a large system may cause regressions in unrelated areas
- A problem with one endpoint may bring down all others

---

# Micro Services

## [fit] The Cure For the Common Monolith
---

# Microservices

- Small, single-purpose API endpoints
- Develop quickly and deploy frequently
- Often used to support high traffic situations
- Often implemented using Asynchronous or Reactive (RX) programming styles

---

# Benefits of Microservices

- Technology choices can be made on smaller scales

---

# Server Options

- Java/Groovy
- JavaScript
- Ruby
- Others


---

# Java/Groovy Options

- Jersey
- Spring Boot
- REST-Easy (JBoss)
- Dropwizard
- Netty-based
-- Ratpack
-- Vert.x

---

# Ratpack Example

``` groovy
import static ratpack.groovy.Groovy.ratpack

ratpack {
    handlers {
        get {
            render "Hello World!"
        }
        get("/person/:name") {
            render "Hello $pathTokens.name!"
        }
    }
}
```

---

# Vert.x Example

``` groovy
vertx.createHttpServer().requestHandler({ req ->
  req.response()
    .putHeader("content-type", "text/plain")
    .end("Hello from Vert.x!")
}).listen(8080)
```

---

# JavaScript Options

- All NodeJS based
- Express
- KOA
- Hapi
- Meteor
- Total.js
- Sails

---

# Express Example

``` javascript
var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

`npm install express --save `
`$ node app.js`

---

# Ruby

- Ruby on Rails

---

# Other Options

- Microsoft
-- WebAPI (very similar to Grails)
- Python
-- Flask
-- Falcon
-- Bottle

---

# Falcon Example

``` python

# sample.py
import falcon
import json

class QuoteResource:
    def on_get(self, req, resp):
        """Handles GET requests"""
        quote = { 'quote': 'Do less.  Better.', 'author': 'Marcus Aurelius' }
        resp.body = json.dumps(quote)

api = falcon.API()
api.add_route('/quote', QuoteResource())

```

---

# Client Options

- Backbone.js
-- Marionette.js
- Ember.js
- Vue.js
