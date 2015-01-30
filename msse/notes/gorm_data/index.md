footer: © Citronella Software Ltd 2015
slidenumbers: true

# GORM Data
## Mike Calvo
### mike@citronellasoftware.com

---

# Bootstrap Considerations
- Running application should respond gracefully to requests
- Local development servers run with in-memory databases which are wiped clean on each run
- Consider what data needs to be loaded in the local app to make it functional

---

# Suggestions
- Check the size of key domain classes to see if they have no data
  - If not, run a populate sample data method
- When creating sample setup data, always use failOnError: true
  - This will give you errors at startup time that sample data was not generated
- Utilize environment metadata

---

# Environment Metadata
- Describes the current active grails environment (development, test, produnction)
- Bootstrap provides an environment block which will only run when the environment matches the block
- Alternatively check Environment.current to see which is currently running

---

# Environment Examples
```
class BootStrap {
  def init = { servletContext ->
    environments {
      development {
        createSampleData()
      }
    }

    if (Envrionment.current != Environment.DEVELOPMENT) {
      // do some non development stuff here
    }
  }
}
```
