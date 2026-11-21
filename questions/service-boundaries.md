# Service Boundaries: Interview Questions

A service boundary is the line that decides which data and behavior one service owns. Interviewers ask about boundaries first, because a wrong boundary cannot be repaired by any pattern applied later. The ten questions below run from basic to senior.

## Questions and answers

### 1. What is a microservice?

A microservice is a small application that owns one business capability, owns its data, and is deployed independently of other services. The word "small" refers to scope, not to lines of code. A useful test is whether one team of five to eight engineers can understand, change, and release the service without coordinating with another team.

### 2. How do you decide where one service ends and another begins?

Start from business capabilities such as ordering, payment, and shipping, not from technical layers such as "database service". Group data that must change together in one transaction inside one service. Then check the boundary against change history: two modules that change in the same pull request most weeks belong together.

### 3. What is a bounded context?

A bounded context is a term from domain-driven design, a method that models software around the business domain. It names an area in which each word has exactly one meaning. "Customer" in billing holds a tax identifier and a payment method, while "customer" in support holds a ticket history. Each context becomes a candidate service with its own model.

### 4. What are cohesion and coupling, and which one do you want?

Cohesion measures how closely the parts inside one service relate to a single purpose. Coupling measures how much one service must know about another in order to work. A good boundary produces high cohesion and low coupling, so most changes touch one service only.

### 5. Why should each service own its database?

A shared database couples services through the schema, so one team's column change can break another team's queries. With one database per service, other services reach the data only through the owning service's API. The cost is that joins across services disappear and must be replaced by API composition or by copied, read-only data.

### 6. How small should a service be?

A service should be small enough for one team to own and large enough to complete a business operation without calling five other services. If a single user request crosses more than three or four services synchronously, the boundaries are probably too fine. Latency and failure probability both grow with each added network call.

### 7. What is a distributed monolith?

A distributed monolith is a set of services that must still be deployed together, because they share a database, share libraries with business logic, or call each other in long synchronous chains. It carries the operational cost of microservices and the change cost of a monolith. The usual sign is a release that requires three teams to deploy in a fixed order.

### 8. How does team structure affect service boundaries?

Conway's law, named after Melvin Conway, observes that a system's structure tends to copy the communication structure of the organization that builds it. A boundary that cuts across two teams produces constant coordination, while a boundary that matches one team's ownership produces independent releases. Senior candidates therefore ask who owns a service before drawing it.

### 9. Two services need the same data. What are the options?

There are three options. The first is a synchronous API call to the owner at read time, which is simple but adds latency and a runtime dependency. The second is to subscribe to the owner's events and keep a local read-only copy, which removes the dependency but makes the copy slightly stale. The third is to merge the two services, which is correct when they need the data in the same transaction.

### 10. When would you merge two services back together?

Merge when the two services change together most of the time, when every request to one calls the other, or when they need a transaction that spans both. For example, an order service and an order-items service with 95 percent of changes shared are one service drawn as two. Merging removes a network call, a saga, and a deployment dependency at once.

## What a strong answer adds

| Level | What the answer contains |
|---|---|
| Basic | The definition and one benefit |
| Solid | The cost of the boundary: lost joins, extra calls, eventual consistency |
| Senior | A test for the boundary (change history, transaction needs, team ownership) and a case for merging |

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-service-boundaries) teaches decomposition with worked designs and diagrams.
- [Grokking Microservices Design Patterns](https://www.designgurus.io/course/grokking-microservices-design-patterns?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-service-boundaries) covers the patterns that follow from a boundary decision.
- Next topic: [Communication](communication.md).
