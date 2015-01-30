footer: © Citronella Software Ltd 2015
slidenumbers: true
# Groovy
## Mike Calvo
## mike@citronellasoftware.com

---

# What Is Groovy?
- A dynamic language for the Java Virtual Machine (JVM)
- Leverages investments in Java
  - Significantly reduces learning curve
  - Integrates with existing Java libraries
  - Can be integrated into existing Java projects

---

# Similarities with Java
- Syntactically correct Java will work in Groovy
  - Does not leverage full power of dynamic language
- Use your favorite class libraries directly as you would in Java
- Call libraries written in Groovy from Java
- Code can often be much more concise than the corresponding amount of Java

---

# Example Syntax Differences
- No semicolon required at the end of a statement
- Inherent support for properties
- No parenthesis required around method arguments
- Implied return

---

# More Syntax Differences
- == operator performs value equality
- Dynamic constructor arguments
- Null safe dereferencing
- String literals

---

# Groovy and Properties
- Java specification does not include inherent support for properties
- Groovy fixes this
- .NET developers understand the benefits
- Significantly reduced amounts of code
- No private members
- No do nothing getters and setters

---

# A Simple Example
```
class Person {
  String firstName
  String lastName
  Date dateOfBirth
}
```

---

# Groovy Strings
- Basic string literals are defined by single quote
- Double quoted Strings are called Gstrings and allow for variable substitution
  - “${firstName} ${lastName}”
- Triple double quotes allow for multi-line strings
“””Name: ${firstName} ${lastName}
          Date of Birth: ${dateOfBirth}”””

---

# Dynamically Typed
- Variables are not required to be declared with a type
- The type is given when the variable is assigned
- Use the def keyword
- Example: def s = ‘a string value’ // s is a string
- This applies to function return types as well
- Example: def foo() { return 0 } // foo defined to return an int

---

# Collections
- Groovy provides shorthand ways of creating collections
- Lists
def list = [‘a’, ‘b’, ‘c’]

- Maps
def map = [firstName: ‘Mike’, lastName: ‘Calvo’]

---

# Closures
- Groovy supports representing a function as an object
- They can be assigned to variables
- They can be passed as arguments to other functions
- They can be dynamically invoked

---

# Closure Arguments
- Specified via the -> operator
- Can be strongly or dynamically typed
- If no arguments are specified a default argument always exists: it
- Default values can be specified

---

# Closure Examples
```
def log = { String message -> println message }
log('hi')

log = { println it }
log('bye')

def adder = { a, b -> a + b }
println adder(1,2)

```

---

# _Groovy Style_
When defined inline closures are usually passed as arguments outside the parenthesis
```
// Closure is argument to eachLine
new File('foo.txt').eachLine { println it }

// Closure is argument to constructor
new Thread { while(true) { sleep(500); check() } }
```

---

# Truth and Falsehood
- Null is false
- Empty collection is false
- Empty string is false
- Zero is false
- Pretty much everything else is true

---

# Duck Typing
- Dynamic typing concept that uses an object’s set of methods and properties to determine semantics rather than inheritance or implementation of an interface

"If something walks like a duck and quacks like a duck then I can refer to that thing as a duck"

---

# Maps as Duck Types
- A map can be used as a duck type for an interface
- Key is method name
- Value is closure with method implementation
- Not all interface methods need to be defined
- Handy for unit test mocks

---

# Parsing Structured Data
- Groovy excels at parsing and converting hierarchical data into object instances
- JSON and XML data can easily be converted into Groovy Maps
- Standard "Slurper" Libraries for reading
- Standard "Builder" Libraries for static object -> data conversion

---

# Example Slurping/Building
```
def response = JsonSlurper.parse('{"name": "Mike", "dob": {"month": 7 }}')
assert response.name == 'Mike'
assert response.dob.month == 7

def builder = new JsonBuilder()
def root = builder.people {
  person {
    name 'Mike'
    dob(month: 7)
  }
}
assert builder.toString() == '{"people":{"name":"mike", "dob":{"month": 7}}}'
```

---

# Differences from Java
- http://groovy.codehaus.org/Differences+from+Java
- Default imports java.io, java.lang, java.net, java.util, groovy.lang, and groovy.util
- in is a keyword used to define ranges
  - for (x in 0..5)
- Arrays cannot be declared as int[] a = {1,2,3}
  - Must be: int[] a = [1,2,3]

---

# _Groovy Style_
- Don't use semicolons
- Use the save navigation operator (?)
  - Example: bean?.property?.value?.method()
- Use operator overloads
- Use map-based constructors
  - Example: new Person(firstName: 'mike', dob: new Date())

---

# Example Operator Overloads
```
def list = ['a', 'b']
list += 'c'
list << 'd'

list = [new Person(name: 'mike'), new Person(name: 'matt')]
assert list*.name == ['mike', 'matt']

def map = [name: 'Mike'] + [dob: new Date()]
assert map.dob
assert map.name == 'Mike'
```

---

# Groovy Meta Programming
- Enabling the dynamic elements of Groovy
- Methods & properties can be added to classes at runtime
- Methods that don't exist at compile time can be invoked at runtime

---

# Meta Programming Tools
- metaClass
  - Describes class (methods, properties)
- invokeMethod
  - Each method invocation calls this method on each instance
- methodMissing
  - Invoked when a method is not found on instance

---

# Groovy Console
- Use this tool to help you test out simple groovy syntax trickiness
