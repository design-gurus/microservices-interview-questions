# Communication Between Services: Interview Questions

Services communicate in two ways: a synchronous call, where the caller waits for a reply, and asynchronous messaging, where the caller hands a message to a broker and continues. Most interview questions on this topic test whether the candidate can choose between the two and state the cost of the choice.

## Questions and answers

### 1. What is the difference between synchronous and asynchronous communication?

In synchronous communication the caller blocks until the other service replies, so both services must be available at the same moment. In asynchronous communication the caller writes a message to a broker, which is a server that stores messages until a consumer reads them. The caller does not wait, so the receiver can be slow or briefly unavailable without failing the caller.

### 2. When is REST the right choice?

REST is an API style that models data as resources addressed by URLs and uses the standard HTTP verbs. It suits public APIs and browser clients, because every language and tool supports HTTP and JSON. Its costs are larger text payloads and no built-in contract, so a schema such as OpenAPI must be maintained separately.

### 3. When would you choose gRPC over REST?

gRPC is a remote procedure call framework that sends binary Protocol Buffers messages over HTTP/2. It suits internal calls between services, where a strict contract, generated client code, and lower latency matter more than human-readable payloads. It also supports streaming in both directions. Browsers cannot call it directly without a proxy, which is why it is rare at the public edge.

### 4. What is the difference between a message queue and a publish-subscribe topic?

A queue delivers each message to exactly one consumer from a group of competing consumers, which suits work distribution such as "resize this image". A publish-subscribe topic delivers each message to every subscribed consumer group, which suits events such as "order placed" that several services care about. Many brokers support both models.

### 5. What is the difference between a command and an event?

A command asks one specific service to do something, such as "charge this card", and the sender expects it to happen. An event states that something already happened, such as "payment captured", and the sender does not know who is listening. Commands couple the sender to the receiver, while events invert that dependency.

### 6. What do at-most-once, at-least-once, and exactly-once delivery mean?

At-most-once delivery may lose a message but never repeats it. At-least-once delivery never loses a message but may repeat it, and it is the default in most brokers. Exactly-once delivery across a network is not achievable in general, so systems combine at-least-once delivery with idempotent consumers, which are consumers that produce the same result when a message arrives twice.

### 7. Why are long synchronous call chains a problem?

Availability multiplies along a chain. Five services at 99.9 percent availability each give about 99.5 percent for the chain, which is five times the downtime of one service. Latency adds up as well, and one slow service holds threads in every caller above it. The usual fixes are fewer hops, parallel calls, and asynchronous steps for work the user does not wait for.

### 8. How do you change an API or event without breaking consumers?

Make only additive changes within a version: add optional fields, and never rename or remove a field or change its meaning. Consumers must ignore fields they do not recognize. A breaking change requires a new version that runs beside the old one until every consumer has moved, with a published removal date.

### 9. How do you get a reply over asynchronous messaging?

The requester sends a message that carries a correlation identifier and the name of a reply queue. The responder processes the message and publishes the result to that reply queue with the same identifier. The requester matches the reply to the original request by the identifier. This keeps the services decoupled in time while still returning a result.

### 10. How do you preserve message order?

Total order across a whole topic prevents parallel consumption, so systems preserve order per key instead. All messages for one order identifier go to the same partition, which is an ordered sub-log of the topic, and one consumer reads that partition. Order across different keys is not guaranteed, and a design should not depend on it.

## Choosing in one table

| Need | Choice | Cost |
|---|---|---|
| The user is waiting for the answer | Synchronous call | Both services must be available |
| Work can finish seconds later | Queue | The result is not immediate |
| Several services react to one fact | Publish-subscribe event | Harder to trace the whole flow |
| Low-latency internal call with a strict contract | gRPC | Needs a proxy for browsers |

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-communication) compares REST, gRPC, and messaging inside full designs.
- [Grokking Kafka and Event-Driven Architecture](https://www.designgurus.io/course/grokking-kafka-and-event-driven-architecture?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-communication) covers partitions, ordering, and delivery guarantees in depth.
- [Grokking Modern API Design Interview](https://www.designgurus.io/course/grokking-modern-api-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-communication) covers contracts and versioning.
- Cheat sheet: [Synchronous vs asynchronous communication](../cheat-sheets/synchronous-vs-asynchronous-communication.md).
- Free article: [Messaging Patterns: Queues, Pub/Sub, and Event Streams](https://www.designgurus.io/blog/message-patterns?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-communication), the three shapes of asynchronous messaging.
- Free answer: [How do you handle communication between microservices?](https://www.designgurus.io/answers/detail/how-do-you-handle-communication-between-microservices?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-communication)
