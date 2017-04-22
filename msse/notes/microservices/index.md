---
title: Web Application Development
layout: default
---
theme: Next, 3

## Microservices
### MSSE 2017

---
# Goals
- Understanding of what Microservices are
- How to manage Microservices
- Tools and strategies for monitoring and implementing Microservices

---
# History - Single Server Application
- Application architecture where almost all logic is bundled together
- Shared database
- Multiple endpoints
- Tightly coupled
- Single deploy

---
# Benefits Of Monolith
- Easy to maintain
- Easy to navigate
- Easy to test
- Simple to develop/test/deploy

---
# Drawbacks
- Single point of failure
- Codebase and repository grow unbounded
- Generally locked into one language

---
# Slow Moving
- Lacks any separation of concern
- Can impact release schedules
- Bugs can and will have system wide outages
- Poor scaling

---

# Microservices
- Materialized in the early 2010's by numerous people
- Instant buzzword
- Still gaining momentum

---
# Microservices - Definition
- An application of loosely coupled services, each with subset of domain and specific function

- There is currently no agreed standard for Microservices

---
# Microservices - Properties
- Services are loosely coupled
- Each service has it's own specific domain or function
- Services are composable
- Services can be tested in isolation from one another
- Can have their own database

---
# Polyglot
- Services only need to understand how to communicate with one another
- Can be written in many languages
- Can choose different database implementations - NoSql vs Standard - etc.

---
# Deployment

- Can be deployed and scaled independently
- Deployments don't affect other services
- Ability to release much faster
- Will require some kind of service discovery mechanism

---
# Other benefits
- Service isolation allows for failure in some services while the rest of the system remains up
- Granularity can help with understanding domain
- Independent scalability offers greater flexibility
  - Harder hit services can have more instances

---
# Microservice Challenges

- Introduction of communication mechanism (generally http) introduces a new opportunity for failure
- Have to handle service failure
- Services need to be resilient and redundant - can still take down an entire system
- Versioning
- Deployment complexity
- With poor design complication can grow exponentially

---

# Shared libraries
- Sharing code now requires versioning to not break other services
- Other teams may implement changes that break your code
- Keep it isolated to logging/error handling
- Don't build a distributed monolith

---

# Strategies for creating resilient and effective Microservices

- Bulkheading
- Circuit Breaking
- Back Pressure (If Applicable)
- Monitoring

---
# Bulkheading
- Services should be designed for redundancy (failures don't impact others)
- Don't share databases
- Separate on basis of criticality
  - User features vs user requirements

---
# Circuit Breaking
- Expect services to fail - how to handle?
- Calling a failing service repeatedly can't help the situation
- Takes significant resources on the caller
- Could interrupt the services ability to become healthy again

---
# Circuit Breaking Flow
- Given a poorly performing backend
- Stop calling for a period of time (open a circuit)
- After a specified period of time, attempt a small volume of requests
  - If success, close the circuit - otherwise keep it open and try again
- Criteria mostly based on timeouts, 5xx

---
# Backpressure
- Unexpected volume can cause slow downs which cascade through the system
- Having the entire system go down is probably the worst case scenario
- In this scenario handling a subset of the entire request volume can be better than handling none
- A service under duress should report it's status
- Other systems respond by reducing load temporarily
- Should involve some kind of auto scaling to eventually resolve

---
# Caching
- Not all data needs to be real time
- Having stale data for some period of time can ease strain on backends
- Possible to deliver data from cache in the event of a backend failure

---

# Health Checks
- Should have a simple ping that can tell if things are good or back
- Can be used to trigger alerts
- Can also be used to kill that process and bring up a new one

---
# Monitoring
- Need to be able to understand a complex system
- Log aggregation key
- Metrics should be pumped to some centralized space
- Alerting on failure crucial

---

# Popular Log Aggregation frameworks
- Splunk
  - Costs money
  - Very user friendly
- Elk (Elasticsearch, Logstash, Kibana)
  - Open source
  - Slightly higher learning curve

---
# Useful frameworks/tools for Microservices
- Zipkin
- Tracing for http
- Understand which requests take so long

---
# Zipkin

![inline](web-screenshot.png)

---

# GraphQL - Query API
- Rest supplement/replacement
- Clients can specify only which values they want to get back
- Can potentially reduce number of Microservices you deploy
- Makes client interaction easier to understand

---
# Hystrix
- Fault tolerant framework for building apis
- Configuration for circuit breaking and timeouts
- Metrics and dashboard view
- Supports backup strategies in case of failure

---

# Visceral
- Visual library for seeing how your services interact
- Dots show relative throughput compared to other services
- Red dots are failures, red services are circuit broken

---

# Do you always need Microservices?
- Most times, the cost of Microservices may not be worth it
- Small applications are less complicated and can survive just fine as a monolith


---
# Current development trends (JVM centric)
- Streaming technology (JavaRx, Akka)
- Messaging (Kafka)
- Kubernetes with Docker

---
# Future
- Technology catching up to the hype
- Streaming technology could be alternative to http
- Datasets becoming mobile - pushing real time data to the edge getting easier
  - Kafka Streams
  - Netflix Hollow
  - Serverless
