# Microservices Interview Questions

> The free, open companion to **Grokking Microservices for System Design Interviews** by [DesignGurus.io](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme), created by Arslan Ahmad.

[![GitHub stars](https://img.shields.io/github/stars/design-gurus/microservices-interview-questions?style=social)](https://github.com/design-gurus/microservices-interview-questions/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/design-gurus/microservices-interview-questions)](https://github.com/design-gurus/microservices-interview-questions/commits/main)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](LICENSE)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

This repository holds 97 microservices interview questions with short model answers, grouped into nine topics, plus six cheat sheets. Every answer states the cost of the technique it describes, because interviewers grade the trade-off and not the definition. For worked designs, diagrams, and assessments, see the full course.

## Start here

| Your situation | Go to |
|---|---|
| The interview is tomorrow | [Patterns at a glance](cheat-sheets/patterns-at-a-glance.md), then [Common mistakes](cheat-sheets/common-mistakes.md) |
| You have two weeks | [Two-week plan](cheat-sheets/two-week-plan.md) |
| You are new to microservices | [Monolith vs microservices](cheat-sheets/monolith-vs-microservices.md), then [Service boundaries](questions/service-boundaries.md) |
| Sagas and consistency confuse you | [Data consistency and sagas](questions/data-consistency-and-sagas.md) |
| You are interviewing for a senior role | [Resilience](questions/resilience.md) and [Migrating from a monolith](questions/migrating-from-a-monolith.md) |

## Questions by topic

| Topic | Questions | Sample question |
|---|---|---|
| [Service boundaries](questions/service-boundaries.md) | 10 | How do you decide where one service ends and another begins? |
| [Communication](questions/communication.md) | 10 | When would you choose gRPC over REST? |
| [Data consistency and sagas](questions/data-consistency-and-sagas.md) | 11 | What is the dual-write problem, and how does the outbox pattern solve it? |
| [Resilience](questions/resilience.md) | 11 | How does a circuit breaker work? |
| [API gateway and service discovery](questions/api-gateway-and-service-discovery.md) | 11 | What should not go into the gateway? |
| [Observability](questions/observability.md) | 11 | Checkout latency doubled an hour ago. How do you find the cause? |
| [Deployment and testing](questions/deployment-and-testing.md) | 11 | How do you change a database schema without downtime? |
| [Migrating from a monolith](questions/migrating-from-a-monolith.md) | 11 | Which service should be extracted first? |
| [Security](questions/security.md) | 11 | How do services authenticate each other? |

## Cheat sheets

| Sheet | What it gives you |
|---|---|
| [Patterns at a glance](cheat-sheets/patterns-at-a-glance.md) | 20 patterns, the problem each solves, and its cost |
| [Monolith vs microservices](cheat-sheets/monolith-vs-microservices.md) | An 11-row comparison and the triggers for extraction |
| [Saga: choreography vs orchestration](cheat-sheets/saga-choreography-vs-orchestration.md) | Both styles applied to one order |
| [Synchronous vs asynchronous communication](cheat-sheets/synchronous-vs-asynchronous-communication.md) | A decision table and the availability arithmetic |
| [Common mistakes](cheat-sheets/common-mistakes.md) | 12 mistakes, each with the sentence to say instead |
| [Two-week plan](cheat-sheets/two-week-plan.md) | A daily schedule of about one hour |

## The ten questions asked most often

1. [What is a microservice?](questions/service-boundaries.md)
2. [Why should each service own its database?](questions/service-boundaries.md)
3. [What is the difference between synchronous and asynchronous communication?](questions/communication.md)
4. [What is a saga?](questions/data-consistency-and-sagas.md)
5. [What is the difference between choreography and orchestration?](cheat-sheets/saga-choreography-vs-orchestration.md)
6. [How does a circuit breaker work?](questions/resilience.md)
7. [What does an API gateway do?](questions/api-gateway-and-service-discovery.md)
8. [What is distributed tracing?](questions/observability.md)
9. [What is the strangler fig pattern?](questions/migrating-from-a-monolith.md)
10. [How do services authenticate each other?](questions/security.md)

## FAQ

### What are the most common microservices interview questions?

The most common questions cover five areas: how to draw service boundaries, how services communicate, how data stays consistent without a shared transaction, how the system survives a failing dependency, and how to migrate from a monolith. The list of ten above is a good first set. Sagas and circuit breakers appear in most senior interviews.

### How do I prepare for a microservices interview?

Learn each concept as a problem, a mechanism, and a cost. Then practice aloud, since interviews are spoken. The [two-week plan](cheat-sheets/two-week-plan.md) schedules one topic a day and ends with a timed 40-minute design.

### What is the difference between a monolith and microservices?

A monolith is one application deployed as a single unit with one database. Microservices are small services that are deployed independently and own their data. The monolith is simpler to build and operate, and microservices allow many teams to release without waiting for each other. See the [full comparison](cheat-sheets/monolith-vs-microservices.md).

### What is the saga pattern in microservices?

A saga is a sequence of local transactions, one per service, that together complete a business operation. If one step fails, compensating transactions undo the steps that already committed. It replaces a database transaction that cannot span several services. See [Data consistency and sagas](questions/data-consistency-and-sagas.md).

### Are microservices asked in system design interviews?

Yes. Most system design questions for backend roles lead to services, queues, and consistency decisions, even when the word "microservices" is not used. Interviewers commonly ask how the design handles a failed service and how data stays consistent across services. Both topics are covered here.

### What microservices questions are asked for senior engineers?

Senior questions ask for judgment instead of definitions. Examples are when to merge two services, how to split a shared database with no downtime, how to prevent a retry storm, and how to decide that a migration is finished. The later questions on each topic page are written at this level.

## Course lessons to start with

- [What Microservices Actually Solve](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/what-microservices-actually-solve?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): the problems that justify the architecture, in Grokking Microservices for System Design Interviews
- [Drawing Service Boundaries](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/drawing-service-boundaries?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): how to decide where one service ends and the next begins, in Grokking Microservices for System Design Interviews
- [Sync vs. Async](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/sync-vs-async?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): when a service should call and when it should send a message, in Grokking Microservices for System Design Interviews
- [Circuit Breakers and Bulkheads](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/circuit-breakers-and-bulkheads?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): how to stop one failing service from causing failures in the others, in Grokking Microservices for System Design Interviews
- [The Saga Pattern by Example](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/the-saga-pattern-by-example?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): one order carried through each step and each compensation, in Grokking Microservices for System Design Interviews
- [The Dual-Write Problem and the Outbox](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/the-dualwrite-problem-and-the-outbox?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): how to update a database and publish an event without losing either, in Grokking Microservices for System Design Interviews
- [A Framework for Microservices Design Answers](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/a-framework-for-microservices-design-answers?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): a fixed order of steps for the design round, in Grokking Microservices for System Design Interviews
- [The Strangler Pattern: A Solution](https://www.designgurus.io/course-play/grokking-microservices-design-patterns/doc/the-strangler-pattern-a-solution?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): how to replace a monolith one route at a time, in Grokking Microservices Design Patterns
- [Queues and Logs](https://www.designgurus.io/course-play/grokking-kafka-and-event-driven-architecture/doc/queues-and-logs?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): the difference between a message queue and an event log, in Grokking Kafka and Event-Driven Architecture

## Free reading on DesignGurus.io

- Free article: [The Microservices System Design Interview: A Six-Step Framework](https://www.designgurus.io/blog/microservices-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): a method for the design round.
- Free article: [Microservices Design Patterns: 19 Essential Patterns](https://www.designgurus.io/blog/19-essential-microservices-patterns-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): each pattern with the problem it solves.
- Free article: [10 Common Microservices Anti-Patterns](https://www.designgurus.io/blog/10-common-microservices-anti-patterns?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): the designs to avoid and their fixes.
- Free article: [Saga Pattern vs Two-Phase Commit](https://www.designgurus.io/blog/saga-pattern-vs-two-phase-commit?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): how to keep data consistent across services.
- Free article: [API Gateway Interview Questions and Answers](https://www.designgurus.io/blog/api-gateway-interview-questions?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): gateway questions with trade-offs.
- Free article: [Circuit Breaker Pattern in System Design](https://www.designgurus.io/blog/circuit-breaker-pattern-in-distributed-systems?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): how to stop a cascading failure.
- Free article: [Transactional Outbox Pattern](https://www.designgurus.io/blog/transactional-outbox-pattern?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): the solution to the dual-write problem.
- Free article: [Decoupling Data: How to Split a Monolith Database](https://www.designgurus.io/blog/decoupling-data-splitting-monolith-database?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): the data side of a migration.

## Go deeper

This repository gives the short answers. The courses below give the full lessons, diagrams, and practice.

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): the companion course, with worked designs and chapter assessments.
- [Grokking Microservices Design Patterns](https://www.designgurus.io/course/grokking-microservices-design-patterns?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): each pattern taught separately, with code.
- [Grokking Kafka and Event-Driven Architecture](https://www.designgurus.io/course/grokking-kafka-and-event-driven-architecture?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): messaging, ordering, and delivery guarantees.
- [Grokking Modern API Design Interview](https://www.designgurus.io/course/grokking-modern-api-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): contracts, versioning, and gateways.
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=readme): the general design round.

Related free repositories: [design-patterns](https://github.com/design-gurus/design-patterns), [grokking-system-design](https://github.com/design-gurus/grokking-system-design), and [api-design-interview](https://github.com/design-gurus/api-design-interview).

For the system design round, see [Grokking System Design](https://github.com/design-gurus/grokking-system-design), the free companion guide with 30 patterns, 59 question walkthroughs, and 35 cheat sheets.

## Contributing

Corrections and new questions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) first.

## License

The content is licensed under [CC BY 4.0](LICENSE). See [NOTICE.md](NOTICE.md) for what the license covers and how to attribute.
