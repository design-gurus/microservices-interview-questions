# Migrating From a Monolith: Interview Questions

A monolith is an application that is built and deployed as one unit. Most teams reach microservices by migrating an existing monolith, not by starting fresh. Interviewers use these questions to check whether a candidate can change a running system without stopping it.

## Questions and answers

### 1. When should a team stay with a monolith?

A team should stay when the product is new, the domain is still changing, or the team has fewer than about 20 engineers. A monolith has one deployment, local function calls, and real transactions. Microservices are worth their operational cost only when deployment contention or uneven scaling needs have become a measured problem.

### 2. What signs indicate that a monolith should be split?

Common signs are release queues in which teams wait days for each other, a build and test cycle longer than 30 minutes, and one module that needs ten times the capacity of the rest. Another sign is frequent incidents in which a fault in one module stops the whole application. Each sign should be measured before the migration begins.

### 3. What is the strangler fig pattern?

The strangler fig pattern replaces a monolith one function at a time. A routing layer is placed in front of the monolith. Each new service takes over one group of URLs, and the router sends those requests to it while everything else still reaches the monolith. The monolith shrinks until it can be retired.

### 4. Why not rewrite the whole system at once?

A full rewrite delivers no value until the last feature is complete, which often takes more than a year. During that time the old system keeps changing, so the target moves. An incremental migration returns value after each extracted service and can be stopped at any point without loss.

### 5. Which service should be extracted first?

Choose a capability with few inbound dependencies, a clear owner, and a real benefit from separation. Notifications or search are frequent first choices. The first extraction also builds the deployment pipeline, monitoring, and service template, so it should carry low business risk. Core order or payment logic should not be first.

### 6. What is a modular monolith?

A modular monolith is one deployable application divided into modules with enforced boundaries. Each module exposes a public interface and keeps its tables private. It gives clear ownership and local transactions without network calls. It is also a useful intermediate step, since a well-separated module can be extracted later with little change.

### 7. How do you split a shared database?

Split the data in stages. First make the monolith reach the tables only through one module's interface. Then move the tables to a separate schema that the new service owns. Replace joins across the boundary with API calls or copied data. Foreign keys across the boundary become identifiers that the application validates.

### 8. How do you keep data consistent while old and new run side by side?

Choose one system as the writer of record for each table at every stage. The other system receives changes through events or change data capture, which is a tool that reads the database log and publishes each row change. Compare the two copies with a scheduled reconciliation job, and switch the writer only after differences reach zero.

### 9. What is an anti-corruption layer?

An anti-corruption layer is a translation component between the new service and the monolith. It converts the monolith's data model and naming into the new service's model, and the reverse. The new service therefore keeps a clean design. When the monolith is retired, only the layer is deleted.

### 10. What is branch by abstraction?

Branch by abstraction changes an implementation inside a running codebase without a long-lived branch. An interface is placed in front of the old code, and a new implementation that calls the new service is added behind it. A feature flag selects the implementation per request. The old implementation is deleted once the new one serves all traffic.

### 11. How do you know when the migration is finished?

Define exit criteria before starting, in the same units as the original problem. Examples are deployment lead time under one day, zero cross-team release dependencies, and a 50 percent drop in incidents with wide effect. Many migrations stop correctly at 70 to 80 percent extracted, because the remaining core gains little from separation.

## A typical extraction order

| Stage | Action | Risk |
|---|---|---|
| 1 | Add a routing layer in front of the monolith | Low |
| 2 | Extract one edge capability, such as notifications | Low |
| 3 | Enforce module boundaries inside the monolith | Medium |
| 4 | Extract capabilities that own their data cleanly | Medium |
| 5 | Split the shared tables of the core domain | High |

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-migrating-from-a-monolith) walks through a migration from the first extraction to the database split.
- Lesson: [Splitting a Monolith: The Strangler Fig](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/splitting-a-monolith-the-strangler-fig?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-migrating-from-a-monolith), in Grokking Microservices for System Design Interviews
- Lesson: [Case Study: Migrating a Legacy Monolith](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/scenario-migrating-a-legacy-monolith?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-migrating-from-a-monolith), in Grokking Microservices for System Design Interviews
- Lesson: [The Strangler Pattern: A Solution](https://www.designgurus.io/course-play/grokking-microservices-design-patterns/doc/the-strangler-pattern-a-solution?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-migrating-from-a-monolith), in Grokking Microservices Design Patterns
- [Grokking Microservices Design Patterns](https://www.designgurus.io/course/grokking-microservices-design-patterns?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-migrating-from-a-monolith) covers strangler fig and anti-corruption layer as patterns.
- Cheat sheet: [Monolith vs microservices](../cheat-sheets/monolith-vs-microservices.md).
- Free article: [Decoupling Data: How to Split a Monolith Database](https://www.designgurus.io/blog/decoupling-data-splitting-monolith-database?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-migrating-from-a-monolith), the data side of a migration.
- Free answer: [What is the strangler pattern for migrating a monolithic application to microservices?](https://www.designgurus.io/answers/detail/what-is-the-strangler-pattern-for-migrating-a-monolithic-application-to-microservices-architecture?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-migrating-from-a-monolith)
