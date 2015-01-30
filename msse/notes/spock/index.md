footer: © Citronella Software Ltd 2015
slidenumbers: true

# Spock Testing Framework
## Mike Calvo
### mike@citronellasoftware.com

---

# What is Spock?
- Testing and specification framework
- Groovy and Java

---

# Why Spock?
- Simple
- Supports data-driven tests
- Great mocking support
- Integrates with tooling

---

# Simple Rules
- Extend Specification
- Fixture methods: setup, cleanup, setupSpec, cleanupSpec
- Define feature methods: `def "pop items from stack"() { ... }`
- Blocks: setup (given), when, then, expect, where, cleanup

---

# Feature Methods
- Provide a description of what feature does
- Four phases:
  - Setup the fixture
  - Provide a stimulus
  - Describe the expected response
  - Cleanup the fixture

---

# Blocks
- Define phases within a feature method
- Label is used to define the start of a block
  - All code before first defined block is a default setup
- Feature methods require at least one defined block

---

# Setup Blocks
- setup:
  - Alias for setup is given:
- Usually reserved for creating objects required by test

``` groovy
setup:
def stack = new Stack()
def elem = 'push me'
```

---

# When and Then Blocks
- Describe the stimulus and response for the test
- Always appear together (when before then)
- Multiple can appear in a feature method

``` groovy
when:
stack.push(value)

then:
stack.size() == originalSize+1
```

---

# Conditions
- Then blocks are a collection of assertions that must evaluate to true to pass
- No 'assert' is required within a then block
- An exception during the when block can be asserted via thrown(ExceptionType)
- Verifying an exception was not thrown via notThrown(ExceptionType)

---

# Interactions
- Then blocks can also contain expectations regarding interactions between components
- Typically used with mock objects

``` groovy
when:
publisher.fire('event')

then:
1 * subscriber1.receive('event')
1 * subscriber2.receive('event')
```

---

# Mocks
- Spock supports mocking of all non-final Java and Groovy classes
- Creating a mock: `def s = Mock(TypeToBeMocked)`
- Calling a method on a mock instance signifies that it is expected to be invoked
- Interactions can be required or optional (cardinality)
- Constraints can be placed on arguments

---

# Cardinalities
- Place expectations on the number of times an interaction happens:

``` groovy
n * subscriber.receive(event)      // exactly n times
(n.._) * subscriber.receive(event) // at least n times
(_..n) * subscriber.receive(event) // at most n times
```

---

# Argument Constraints
- No arguments: m.method()
- Any argument: m.method(_)
- Any non-null argument: m.method(!null)
- Any argument of a specific type: m.method(_ as Type)
- A specific argument: m.method(value)
- A custom constraint: m.method({ it.name == 'oi!'})

---

# Return values
- Returning a single value:
`subscriber.isAlive() >> true`
- Returning a different value per invocation:
`subscriber.isAlive() >>> [true, false, true]`
- Defining a new implementation:
`subscriber.isAlive() >> { random.nextBoolean() }`

---

# Mock Example
```
setup:
def event = new Event()
def subscriber = Mock(Subscriber)
subscriber.isAlive() >> true // global interaction

when:
publisher.send(event)

then:
1 * subscriber.receive(event) // local interaction
```

---

# Full Example
``` groovy
class MySpec extends Specification {

  def 'simple test of something silly'() {
    setup:
    def input = 10

    when:
    def result = service.add(input)

    then:
    result == input + 10
  }
}
```

---

# Testing Expected Exceptions
- Use the thrown method with exception type in the then block

``` groovy
def 'bad things happen'() {
  when:
  service.provide(null)

  then:
  thrown(IllegalArgumentException)
}
```

---

# Data-driven Tests
- Use the where block
- Two forms: arrays or tables
- Must come last in method and can't be repeated
- Values can be used in all other blocks as inputs and expected values
- @Unroll to display individual data results

---

# Data-driven Example

``` groovy
@Unroll('#description')
def 'supports count parameter'() {
  given:
  for (int i = 1; i <= count; i++) {
    new Artist(name: "Artist #${i}").save()
  }
  params.count = count

  when:
  controller.index()
  def returned = parser.parseText(response.text)

  then:
  returned.size() == expectedReturned

  where:
  description  | count | expectedReturned
  'Zero'       | 0     | 0
  'Two'        | 2     | 2
  'Ten'        | 10    | 10
  'Cap at ten' | 30    | 10
  }
}
```

---

# Fixture Methods
- Setup/cleanup code shared by all methods
- def setup() {}
- def cleanup() {}
- def setupSpec() {}
- def cleanupSpec() {}

---

# Helper Methods
- Often common to pull frequently used validations into their own methods
- These can be added as regular methods to specification
- Implicit asserts must be converted to explicit asserts

---

# Optional Block Descriptions
- Block labels can be given a descriptive text
- The 'and' block provides some syntactic sugar to separate expected results

``` groovy
given: 'an empty bank account'
// ...
when: 'the account is credited $10'
// ...
then: "the account's balance ins $10"
// ...
```

# Spock Extensions
- @Timeout - provide a timeout for spec or fixture
- @Ignore - prevents running of a feature method
- @IgnoreRest - helps quickly run a single test
- @FailsWith - Alternative to thrown check

---

# Try Spock Out
- Web Console

http://meetspock.appspot.com/?id=9001

---

# Spock with Grails
- Version 2.4.4 comes with Spock support built in

http://grails.org/plugin/spock
