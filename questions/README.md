# Questions by Topic

This folder holds 97 microservices interview questions across nine topics. Each question has a model answer of two to four sentences, and each page runs from basic questions to senior ones.

| Topic | Questions | What it tests |
|---|---|---|
| [Service boundaries](service-boundaries.md) | 10 | Where one service ends, data ownership, when to merge |
| [Communication](communication.md) | 10 | REST, gRPC, messaging, synchronous versus asynchronous |
| [Data consistency and sagas](data-consistency-and-sagas.md) | 11 | Eventual consistency, sagas, outbox, CQRS |
| [Resilience](resilience.md) | 11 | Timeouts, retries, circuit breaker, bulkhead, idempotency |
| [API gateway and service discovery](api-gateway-and-service-discovery.md) | 11 | Entry point, routing, registries, service mesh |
| [Observability](observability.md) | 11 | Logs, metrics, traces, service level objectives |
| [Deployment and testing](deployment-and-testing.md) | 11 | Release strategies, schema changes, contract tests |
| [Migrating from a monolith](migrating-from-a-monolith.md) | 11 | Strangler fig, database split, exit criteria |
| [Security](security.md) | 11 | Authentication, tokens, mutual TLS, least privilege |

## Suggested order

Read the first three topics in order, since boundaries decide communication and communication decides consistency. The remaining six can be read in any order.

For worked designs that combine these topics, see [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-readme).
