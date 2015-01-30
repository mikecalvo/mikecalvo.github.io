footer: © Citronella Software Ltd 2015
slidenumbers: true

# Messaging
## Mike Calvo
### mike@citronellasoftware.com

---
# What is Messaging?
- Asynchronous communication approach
- System components send messages and don't wait for message to be handled
- Messages describe events, actions, and can contain data payloads
- Message-Oriented-Middleware (MOM) is infrastructure that supports sending and delivery of messages

---
# Internal and External Messages
- Messages can be sent between components within a system
  - Long running processes
  - Activities not requring an immediate response
- Messages can be sent inter-system as an alternative to synchronous forms of integration (like web services)

---
# Messaging Types
- Publish-Subscribe
  - Topics
  - 1 message delivered to many receivers
- Point-to-point
  - Queues
  - 1 message delivered to 1 receiver

---
# Guaranteed Delivery
- MOM typically will store messages until they can be delivered
- Durable subscriptions ensure that subscribers will still receive messages even if they are temporarily offline when the message is sent

---
# Message Piping and Filtering
- Middleware can apply transformations to messages between send and delivery
- Payloads can be transformed to
  - Protect/remove sensitive data
  - Transform data from one system view to another
- Messages can also have functionality applied to them in transit
  - Archive messages for auditing
  - Trigger secondary messages
  - Compression

---
# Enterprise Service Bus
- Modern MOM
- Large, enterprise, typically expensive products
- Examples:
  - Oracle Enterprise Service Bus (formerly Aqualogic)
  - Apache ServiceMix and Mule (OSS)

---
# Benefits of Messaging
- Can offer more flexibility and dynamic behavior
- Systems are less coupled
  - Receipients can be offline and not affect other components
- Long running tasks can run out of band with user or system requests
- Data transformation can be extracted and isolated in middleware tier

---
# Java Message Service (JMS)
- Vendor neutral Java API for sending and receiving messages
- Resource adapter (similar to JDBC)
- Includes API for both sending and receiving messages

---
# JMS Concepts
- ConnectionFactory -> Connection
- Connection -> Session
- Session -> MessageConsumer | MessageProducer
- Session creates Messages
- Messages sent to Destinations (Topic | Queue)

---
# JMS Implementations
- JEE Servers: JBoss, WebSphere, Oracle
- ActiveMQ: standalone, embeddable implementation

---
# Spring and JMS
- Spring provides a comprehensive set of libraries to make JMS programming easier
- POJO-based message receivers
  - More testable

---
# Grails and JMS
- JMS Grails plugin provides basic dependencies:
 `compile ":jms:1.3"`
- Provider required to implement JMS Spec

  - ActiveMQ is a popular choice
  - Install the plugin
  - `compile ":activemq:0.4.1"`

---
# JMS Grails Plugin
- Adds a jmsService component to Grails
- Available in controllers and services
- Methods for sending JMS messages to Queues and Topics
- Provides extension for services to be exposed as JMS endpoints (listeners)

---
# ActiveMQ
- Can also be downloaded and run seperately from Grails
- More typical configuration for non-development environments
- Easy to download and get running
- Add a bean to Groovy.config that connects to external instance of ActiveMQ

---
# JMS Listener example

```
class EmailService {
  static expose = ['jms']
  static distnation 'mailQueue'

  void onMessage(message) {
    // write email sedning logic here
  }
}
```

---
# Muzic: Add JMS Sender
- When a new song is played (Play instance saved) create a message to notify followers of the artist.
- Use ActiveMQ as JMS implementation
- Start ActiveMQ watch the messages arrive via the admin console

---
# Muzic: Add JMS Listener
- New domain concept: Follow (Profile following an artist)
- New domain concept: ProfileMessage (Message for a user's profile)
- Add a NotificationService that listens for messages
- It creates a new ProfileMessage for each follower of the artist

---
# Grails Platform Core
- Plugin utilities and glue code for 'advanced' Grails plugin development
- An attempt to extract features out of Grails core to prevent binding plugins to specific Grails versions
- Goal: allow plugins to remain compatable with Grails versions longer

---
# Grails Platform Core APIs
- Configuration: config merging, validation and defaults
- Security: basic security features most applications require
- UI Extensions: Tags and helper functions
- Navigation: Standard way to expose app menu information
- Events: Lightweight in-app messaging provider

---
# Platform Core Events API
- Install platform-core plugin:
  `compile ":platform-core:1.0.0"`
- Event raising functions added to Grails components
- Events are publish/subscribe event model
- Simpler alternative to JMS for in-app messaging

---
# Sending an Event
- Call the 'event' method within controller or service
- Specify the name of the topic the event is posted to
- Specify optional data sent to listeners
- Specify optional params
- Specify optional callback function

---
# Event Parameters
- timeout
- namespace: avoids channel naming collisions
- fork: force event to reuse calling thread
- gormSession: create new GORM session for new thread
- onReply
- onError

---
# EventReply
- The event method returns an EventReply
- Implements Future<Object>
- waitFor() allows event sending thread to block until event is processed
- The response from listeners of the event can be retrieved via getValue() or getValues() calls

---
# Listening for Events
- Static Registration
  - Annotate service methods with @Listener
  - Define topic, namespace (default is name of method)
- Dynamic Registration
  - Call 'on' method supplying a topic name and handler closure

---
# Replying from Listeners
- Just return an object
  - This will be available from the EventReply.getValue() call

---
# Message Send/Receive Example

```
class EmailService {
  Post sendEmail(String to, String body) {
    event 'sendEmail', [to: to, body:body]
  }
}

class DeliveryService {
  @grails.events.Listener
  def onSendEmail(def email) {
    log.info('Sending message: '+email)
    // actually send the mail
  }
}
```

---
# Listening for Grails-generated Events
- Grails produces events
- Uses namespaces to group events from subsystems
- Topic name can be specifed in the @Listener annoation
  - Wildcards can be used to listen for multiple topics

---
# GORM Events
- GORM generates events for before and after database writes
- onSaveOrUpdate can be specified with a domain class parameter
- Return false from before event handlers to prevent the write
- namespace: 'gorm'

---
# GORM Event Topics
- after/beforeInsert
- after/beforeUpdate
- after/beforeDelete
- beforeValidate
- onSaveOrUpdate

---
# GORM Event Examples

```
@grails.events.Listener(namespace = 'gorm')
def onSaveOrUpdate(User) {
  // doSomething
}

@grails.events.Listener(namespace = 'gorm')
def beforeUpdate(User user) {
  if (!user.loggedIn()) {
    return false
  }
}

@grails.events.Listener(topic = '*', namespace = 'gorm')
def BeforeEachGormEvent(EventMessage message) {
  // do something
}

```

---
# Muzic: Use Core Events
- Re-implement the Song Play->Notify Artist Followers logic using the Grails Core Events plugin instead of JMS
