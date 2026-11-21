# Synchronous vs Asynchronous Communication

In synchronous communication the caller waits for the reply before it continues. In asynchronous communication the caller sends a message to a broker and continues at once, and the receiver processes the message later. The choice decides how failures spread, so it deserves one explicit sentence in every design answer.

## Side by side

| Dimension | Synchronous (REST, gRPC) | Asynchronous (queue, topic) |
|---|---|---|
| Caller waits | Yes | No |
| Both sides available at once | Required | Not required |
| Result | Immediate | Later, through an event or a status query |
| Failure effect | The caller fails or degrades | The message waits in the broker |
| Load spikes | Passed directly to the receiver | Absorbed by the queue |
| Consistency | Easier to keep immediate | Eventual |
| Tracing a request | One call chain | Events across several consumers |
| Extra infrastructure | None | A broker to operate |
| Duplicate handling | Needed only for retries | Always needed |

## The availability arithmetic

Synchronous availability multiplies. If a request passes through 4 services that are each available 99.9 percent of the time, the chain is available about 99.6 percent of the time. That is roughly 35 hours of downtime a year, against 9 hours for one service.

An asynchronous step is removed from that calculation. If the order service only needs to publish "order placed", it succeeds whenever the broker is available. The email service can be unavailable for an hour, and its messages are processed when it returns.

## How to choose

| Question | If yes |
|---|---|
| Does the user need the result to continue? | Synchronous |
| Can the work finish seconds or minutes later? | Asynchronous |
| Do several services react to the same fact? | Asynchronous, with a publish-subscribe topic |
| Does the step take longer than about 1 second? | Asynchronous, with a status the client can query |
| Is the call a simple read of another service's data? | Synchronous, with a timeout and a cache |
| Must the caller survive the receiver being unavailable? | Asynchronous |

## Worked example: checkout

A user presses "Pay". Five things must happen.

| Step | Mode | Reason |
|---|---|---|
| Validate the cart | Synchronous | The user must see errors immediately |
| Charge the card | Synchronous | The user is waiting for success or failure |
| Reserve stock | Synchronous or saga step | The order is invalid without stock |
| Send the confirmation email | Asynchronous | A 30-second delay is acceptable |
| Update the recommendation model | Asynchronous | The user never waits for this |

The synchronous path now contains three services instead of five. Latency drops, and an email service failure no longer fails checkout.

## Combining both

Many designs accept a request synchronously and complete it asynchronously. The service validates the input, stores the request, and returns status 202 with a request identifier. A worker processes the request from a queue. The client learns the outcome through a status endpoint, a webhook, or a push message.

## What asynchronous messaging requires

1. **Idempotent consumers.** Brokers deliver at least once, so a message can arrive twice.
2. **A dead-letter queue.** This is a separate queue that holds messages that failed repeatedly, for example after 5 attempts.
3. **An ordering decision.** Order is kept only per key, such as per order identifier.
4. **Monitoring of queue depth and message age.** A consumer can be running and still be an hour behind.
5. **The outbox pattern.** The state change and the message must commit together.

## Mistakes interviewers listen for

| Mistake | Correction |
|---|---|
| "Make everything asynchronous" | Users still need immediate answers for some steps |
| A synchronous chain of six services | Shorten the chain or move steps to events |
| A queue with no plan for duplicates | State the idempotency method |
| Assuming global message order | Promise order per key only |

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-synchronous-vs-asynchronous-communication) applies this choice step by step in full designs.
- [Grokking Kafka and Event-Driven Architecture](https://www.designgurus.io/course/grokking-kafka-and-event-driven-architecture?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-synchronous-vs-asynchronous-communication) covers brokers, partitions, and consumer groups.
- Questions: [Communication](../questions/communication.md).
- Free article: [Synchronous vs Asynchronous Communication in System Design Interviews](https://www.designgurus.io/blog/sync-vs-async?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-synchronous-vs-asynchronous-communication), the same choice explained at full length.
- Free article: [Event-Driven Architecture: Patterns, Diagrams, and When to Use It](https://www.designgurus.io/blog/event-driven-architecture?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-synchronous-vs-asynchronous-communication), a guide to the asynchronous side.
