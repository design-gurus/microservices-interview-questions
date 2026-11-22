# Saga: Choreography vs Orchestration

A saga is a sequence of local transactions across services, with a compensating transaction to undo each step if a later one fails. A saga can be coordinated in two ways. In choreography, services react to each other's events with no central controller. In orchestration, one component directs every step.

## How each one works

**Choreography.** The order service publishes "order created". The inventory service hears it, reserves stock, and publishes "stock reserved". The payment service hears that, charges the card, and publishes "payment captured". Each service knows only which events it consumes and which it publishes.

**Orchestration.** An orchestrator receives the order. It sends "reserve stock" to inventory and waits for the reply. It then sends "charge card" to payment and waits again. The orchestrator stores the saga's current step, so it knows what to compensate if a reply reports failure.

## Side by side

| Dimension | Choreography | Orchestration |
|---|---|---|
| Control | Spread across the participants | Held by one orchestrator |
| Coupling | Services know event formats only | The orchestrator knows every participant |
| Where the flow is visible | Nowhere in one place | In the orchestrator's code or definition |
| Adding a step | Add a new subscriber | Change the orchestrator |
| Failure handling | Each service listens for failure events | The orchestrator issues compensations in reverse order |
| Debugging | Requires tracing across all events | Read the saga state in one store |
| Risk | Cyclic event dependencies | The orchestrator collects business logic |
| Best fit | 2 to 4 steps, stable flow | 5 or more steps, branches, timeouts |

## Worked example: an order with three steps

The order costs 40 dollars and contains 2 items.

| Step | Choreography | Orchestration |
|---|---|---|
| 1 | Order service publishes "order created" | Orchestrator creates the saga record |
| 2 | Inventory reserves 2 items, publishes "stock reserved" | Orchestrator sends "reserve stock", receives success |
| 3 | Payment fails, publishes "payment failed" | Orchestrator sends "charge card", receives failure |
| 4 | Inventory hears "payment failed", releases 2 items | Orchestrator sends "release stock" |
| 5 | Order service hears "payment failed", cancels the order | Orchestrator sends "cancel order", marks the saga failed |

Both versions reach the same end state. The difference is where the knowledge of the sequence lives. In choreography, the inventory service must know that a payment failure concerns it. In orchestration, only the orchestrator knows.

## How to choose

Choose choreography when the flow has few steps and each service's reaction is simple. It needs no extra component, and services stay independent.

Choose orchestration when any of these conditions holds:

- The flow has five or more steps or includes branches.
- A step has a timeout, such as "cancel if payment is not confirmed in 15 minutes".
- The business needs to see the status of each running saga.
- Compensations must run in a strict order.

## Rules that apply to both

1. **Every step is idempotent.** Messages arrive at least once, so a repeated "reserve stock" must not reserve twice.
2. **Every step before the pivot has a compensation.** The pivot is the last step that can fail.
3. **Messages are published with the outbox pattern.** The state change and the message commit together.
4. **Intermediate state is visible.** Mark the order PENDING so other requests do not treat it as complete.

## Mistakes interviewers listen for

| Mistake | Correction |
|---|---|
| Describing compensation as a rollback | A compensation is a new transaction, such as a refund |
| Placing an irreversible step first | Put steps that cannot be undone after the pivot |
| An orchestrator that holds pricing or stock rules | Keep business rules in the participants |
| Forgetting what happens when a compensation fails | Retry it until it succeeds, and alert after a set number of attempts |

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-saga-choreography-vs-orchestration) builds a saga both ways and walks through each failure path.
- Lesson: [Choreography vs. Orchestration](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/choreography-vs-orchestration?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-saga-choreography-vs-orchestration), in Grokking Microservices for System Design Interviews
- Lesson: [The Saga Pattern by Example](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/the-saga-pattern-by-example?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-saga-choreography-vs-orchestration), in Grokking Microservices for System Design Interviews
- Lesson: [Sagas on Kafka](https://www.designgurus.io/course-play/grokking-kafka-and-event-driven-architecture/doc/sagas-on-kafka?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-saga-choreography-vs-orchestration), in Grokking Kafka and Event-Driven Architecture
- [Grokking Kafka and Event-Driven Architecture](https://www.designgurus.io/course/grokking-kafka-and-event-driven-architecture?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-saga-choreography-vs-orchestration) covers the event delivery that choreography depends on.
- Questions: [Data consistency and sagas](../questions/data-consistency-and-sagas.md).
- Free article: [Saga Pattern vs Two-Phase Commit](https://www.designgurus.io/blog/saga-pattern-vs-two-phase-commit?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-saga-choreography-vs-orchestration), the two approaches to a distributed transaction compared.
- Free answer: [What is the Saga pattern and when should you use it?](https://www.designgurus.io/answers/detail/what-is-the-saga-pattern-for-managing-distributed-transactions-and-when-should-you-use-it?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-saga-choreography-vs-orchestration)
