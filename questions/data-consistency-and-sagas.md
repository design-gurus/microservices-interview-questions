# Data Consistency and Sagas: Interview Questions

When each service owns its database, one business operation can no longer run inside one database transaction. This topic produces the hardest microservices interview questions, because every answer trades immediate consistency for availability and independence.

## Questions and answers

### 1. Why can you not use one transaction across services?

A database transaction guarantees all-or-nothing changes inside one database. Placing an order may change the order database, the payment database, and the inventory database, and each belongs to a different service. No single transaction manager covers all three, so the operation needs a different coordination method.

### 2. What is eventual consistency?

Eventual consistency means that the copies of data in different services may disagree for a short time, and they agree once all pending updates have been applied. For example, an order may show "paid" two seconds before the inventory count drops. The design task is to decide which reads may be stale and for how long.

### 3. What is two-phase commit, and why is it rare between microservices?

Two-phase commit is a protocol in which a coordinator first asks every participant to prepare and then tells all of them to commit. Participants hold locks between the two phases, so one slow service blocks the others. If the coordinator fails in the middle, participants stay locked until it recovers. These costs conflict with independent, highly available services.

### 4. What is a saga?

A saga is a sequence of local transactions, one per service, in which each step publishes a result that triggers the next step. If a step fails, the saga runs compensating transactions that undo the earlier steps. The saga gives up isolation: other requests can see the intermediate state while the saga is running.

### 5. What is a compensating transaction?

A compensating transaction is a new operation that reverses the business effect of an earlier committed step. Refunding a payment compensates a charge, and releasing stock compensates a reservation. It is not a database rollback, because the original change was already committed and may have been seen by other users.

### 6. What is the difference between choreography and orchestration?

In choreography each service listens for events and decides by itself what to do next, so no central component exists. In orchestration one service, the orchestrator, sends a command to each participant and tracks the state of the saga. Choreography suits two or three steps, while orchestration keeps longer flows readable in one place. See the [comparison sheet](../cheat-sheets/saga-choreography-vs-orchestration.md).

### 7. What is the dual-write problem, and how does the outbox pattern solve it?

A dual write happens when a service updates its database and publishes a message as two separate actions. A crash between them leaves the database changed and the message unsent. The transactional outbox pattern writes the message to an outbox table in the same local transaction as the data. A separate relay process then reads the table and publishes each message at least once.

### 8. How do you handle a saga step that cannot be compensated?

Some actions, such as sending an email or shipping a parcel, cannot be undone. Order the saga so that these steps come last, after every step that might fail. The last step that can still fail is called the pivot. Steps after the pivot must be retried until they succeed, so they must be idempotent.

### 9. What are CQRS and event sourcing?

Command Query Responsibility Segregation (CQRS) uses one model for writes and a separate, denormalized model for reads, which is updated from events. Event sourcing stores every state change as an appended event and derives the current state by replaying events. The two are often combined, but each can be used alone, and both add eventual consistency to reads.

### 10. How do you query data that lives in several services?

There are two main methods. API composition calls each owning service and joins the results in memory, which is simple but slow for large result sets. A CQRS read model subscribes to events from each service and keeps a prejoined view in its own store, which is fast to read and slightly stale.

### 11. A user sees stale data right after an update. What do you do?

First decide whether the staleness matters for that screen. If it does, read the user's own writes from the write model or the owning service for a few seconds after the update. Alternatively, return the new state in the response to the write, so the client does not need to query the read model at all.

## Worked example: placing an order

| Step | Service | Local transaction | Compensation |
|---|---|---|---|
| 1 | Order | Create order as PENDING | Mark order CANCELED |
| 2 | Inventory | Reserve 2 items | Release 2 items |
| 3 | Payment | Charge 40 dollars (pivot) | Refund 40 dollars |
| 4 | Order | Mark order CONFIRMED | None, retried until it succeeds |

If the charge in step 3 fails, the saga releases the stock and cancels the order. Steps 1 and 2 are placed before the pivot because both are cheap to reverse.

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-data-consistency-and-sagas) walks through sagas step by step, including failure paths.
- [Grokking Microservices Design Patterns](https://www.designgurus.io/course/grokking-microservices-design-patterns?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-data-consistency-and-sagas) covers saga, outbox, CQRS, and event sourcing as separate patterns.
- [Grokking Kafka and Event-Driven Architecture](https://www.designgurus.io/course/grokking-kafka-and-event-driven-architecture?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-data-consistency-and-sagas) covers the event log that these patterns rely on.
- Free article: [Transactional Outbox Pattern: How to Solve the Dual-Write Problem](https://www.designgurus.io/blog/transactional-outbox-pattern?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-data-consistency-and-sagas)
- Free article: [Saga Pattern vs Two-Phase Commit](https://www.designgurus.io/blog/saga-pattern-vs-two-phase-commit?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-data-consistency-and-sagas), when each one fits a distributed transaction.
