footer: © Citronella Software Ltd 2015
slidenumbers: true

# Grails URL Mappings
## Mike Calvo
### mike@citronellasoftware.com

---
# Grails URLs
- Convention-based
  - Default: /controller/action/id
- Overrides:
  - grails-app/conf/UrlMappings.groovy
  - HTTP Method Restrictons

---
# Basic URLMappings Syntax
'<url>'(configuration)

or

'<url>' {
  // configuration
}

---
# Examples

```
'product'(controller: 'product', action: 'list')
'product'(controller: 'product') // goes to default action
'product' {
  controller = 'product'
  action = 'list'
}
```

---
# URL Groups
- URLs that fall under the same path can be grouped:

```
group '/product', {
  'apple'(controller:'product', id:'apple')
  'htc'(controller:'product', id:'htc')
}
```

---
# URI rewriting
- Converts the reqeuested URI into another
- Can be used to integrate with other frameworks that use different extensions
  `'/hello'(uri: 'hello.dispatch')`

---
# REST Resources
- Standard REST Responses mapped to controller actions
- Two forms:
  plural: `'/books'(resource: 'book')`
  single: `'/book'(resource: 'book')`
- Optionally specify includes and excludes for actions:
  `'/books'(resources: 'book', includes:['index', 'show'])`

---
# REST Methods/URL/Action
- GET: /books -> index
- GET: /books/${id} -> show
- POST: /books -> save
- DELETE: /books/${id} -> delete
- PUT: /books/${id} -> update

---
# Nested Resources
- Allows for accessing resources within other resources

```
'/books'(resources:'book') {
  '/authors'(resources:'author')
}
```

GET /books/${bookId}/authors -> index

---
# Redirects
- Send the request to a Url to another request
- Can specify another:
  - Url
  - Controller/Action
  - View

---
# Embedded Variables
- $ in the configuration specifies a path variable
- Path variables are included in the params available to the controller action
- Example:
  `'/$blog/$year/$month/$day/$id'(controller: 'blog', show:)`
  /mikecalvo/2015/02/12/grails

---
# Dynamic Conroller and Action Names
- $controller and $action are special variables
- ? character means the path variable is optional
- Default Grails mapping use these:
'/$controller/$action?/$id?'
- Use ? to make an extension optional:
'/$controller/$action?/$id?(.$format)?'()

---
# Dynamically Resolved Variables
- Assign variables at runtime based on parameters, session values, etc:

```
'/holiday/win'() {
  id = { params.id }
  isEligible = { session.user != null }
}
```

---
# Response Codes
- Use the response code as the name matcher:

```
'403'(controller: 'errors', action: 'forbidden')
'404'(view: '/errors/missing')
'500'(controller: 'errors', action: 'serverError')
'500'(view: '/errors/npe', exception: NullPointerException)
```

---
# Control Mapping by HTTP Method

```
'/product/$id'(controller:'product', action:'update', method:'PUT')
```

---
# URL Pattern Wildcards
- * anything at one level
- ** anything at nested levels
- Can be used within excludes to prevent URLs from being handled by Grails:
```
class UrlMappings {
  static excludes = ['/images/**', '/css/**']
}
```

---
# URL Constraints
- Limit the variables processed during Url matching:

```
'/$blog/$year?/$month?/$day?/$id?' {
  controller = 'blog'
  action = 'show'
  constraints {
    year(matches:/\d{4}/)
    month(matches:/\d{2}/)
    day(matches:/\d{2}/)
  }
}
```

---
# Customizing Url Formats
- Default mapping uses camel cases
- Alternative option: hyphenation
- Set grails.web.url.converter = 'hyphenated' in Groovy.config

---
# Implementing A Custom Converter
  - Define class that implements grails.web.UrlConverter
  - Registering the bean with a prefix name of 'grails.web.UrlConverter.bean_name'
  - Assign grails.web.url.converter = bean_name
