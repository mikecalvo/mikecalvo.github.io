footer: © Citronella Software Ltd 2015
slidenumbers: true

# Grails Filters
## Mike Calvo
### mike@citronellasoftware.com

---
# Servlet Filters
- Components that wrap HTTP requests
  - Apply logic before or after a Servlet request is handled
- Can be applied chained
  - Wrapped around each other
- Usages: Compression, Security, Logging/Auditing
- Similar to Aspects

---
# Grails Filters
- Simplifyies the creation and configuration of Servlet Filters
- Live in grails-app/confg
- Files end in the word 'Filter'
  - Class contains a code block called 'filters'
- More than one can be defined within a class

---
# Filter Matching
- Can be based on similar parameters to UrlMappings:
  - controller, action, uri
- Supports:
  - Wildcards,
  - Regex Patterns
  - Inversions
  - Exclusions

---
# Filter Types
- before : before the action is excuted.  Return false to not run the action
  - Request can be redirected preventing it from getting to destination
- after : after the action is executed.  Argument is the view model
- afterView : after the view is rendered

---
# Filter Variables
- Similar to those available to controller actions
- request, response, session, servletContext, flash, params
- Grails-specific: actionName, controllerName, grailsApplication
- Spring-specific: applicationContext

---
# Altering Request Flow
- If the filter does nothing, request proceeds as normal to UrlMapping responder
- redirect: call to route the request elsewhere (login page)
- render: call to render a response immediately (system outage)

---
# Example Filter

```
class AppFilters {
  def filters = {
    loggingFilter(controller:'*', action:'*') {
      before = {
        request['start_time'] = System.currentTimeMillis()
      }
      after = {
        if (!log.debugEnabled) {
          return true
        }

        def start = request['start_time']
        def end = System.currentTimeMillis()
        log.debug("Request ${controllerName}.${actionName} time: ${end - start}ms")
      }
    }
  }
}
```

---
Filter Dependencies
- Filters can be applied in order
  - Add the 'dependsOn' field to mark filters to be applied first

```
class SecurityFilter {
  def dependsOn = [AppFilter]

  def filters = {
    before {
      if (!session.user) {
        redirect(controller: 'login')
        return false
      }
    }
  }
}
```
