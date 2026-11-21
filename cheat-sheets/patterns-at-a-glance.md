# Microservices Patterns at a Glance

A pattern is a named, reusable solution to a problem that appears in many systems. This sheet lists the 20 patterns that interviews ask about most, each with the problem it solves and the cost it adds. Naming the cost is what separates a strong answer from a memorized one.

## The table

| Pattern | Problem it solves | Cost |
|---|---|---|
| Database per service | Schema changes in one service break another | No joins or transactions across services |
| API gateway | Clients must know every service address | One more hop and a component that must stay highly available |
| Backend for Frontend | One API cannot fit mobile and web equally well | One gateway to maintain per client type |
| Service registry and discovery | Instance addresses change many times a day | A registry that must itself be highly available |
| Saga | One business operation spans several databases | No isolation, and every step needs a compensation |
| Transactional outbox | A crash between a database write and a message publish loses the message | A relay process, and consumers must accept duplicates |
| CQRS | Queries need data from several services or a different shape | Read data is slightly stale, and there are two models to maintain |
| Event sourcing | The system needs a full history of every change | Replays, snapshots, and versioned event formats |
| API composition | One screen needs data from three services | Latency of the slowest call, and partial failures to handle |
| Circuit breaker | Calls to a failing service use up threads | Thresholds to tune, and a fallback to design |
| Bulkhead | One slow dependency consumes all resources | Idle capacity in the separate pools |
| Retry with backoff and jitter | Temporary errors fail requests that would succeed a moment later | Extra load, and the operation must be idempotent |
| Timeout | A caller waits indefinitely for a reply | Some slow but valid requests are cut off |
| Idempotency key | A retried request performs the action twice | Storage for keys and results, with an expiry rule |
| Strangler fig | A monolith cannot be replaced in one step | Old and new systems run together for months |
| Anti-corruption layer | A legacy data model spreads into new services | A translation component to build and later delete |
| Sidecar and service mesh | Every service repeats retry, encryption, and metrics code | A proxy beside every instance, plus a control plane to operate |
| Consumer-driven contracts | A provider change breaks a consumer after release | Contracts to write and run in every provider build |
| Distributed tracing | Nobody can see where a request spent its time | Instrumentation in every service, and storage for spans |
| Blue-green and canary release | A bad release reaches every user at once | Spare capacity and traffic-splitting infrastructure |

## Patterns by the question that triggers them

| The interviewer asks | Reach for |
|---|---|
| "How do you keep data consistent across services?" | Saga, transactional outbox, idempotency key |
| "How do you query across services?" | API composition, CQRS |
| "What happens when the payment service is slow?" | Timeout, circuit breaker, bulkhead |
| "How do clients reach the services?" | API gateway, Backend for Frontend, discovery |
| "How would you break up this monolith?" | Strangler fig, anti-corruption layer |
| "How do you release safely?" | Canary release, consumer-driven contracts |
| "How would you debug a slow request?" | Distributed tracing |

## How to present a pattern in 30 seconds

A complete answer has four parts, in this order.

1. **Problem.** State the failure that exists without the pattern.
2. **Mechanism.** Describe how the pattern works in one or two sentences.
3. **Cost.** Name what the pattern adds or removes.
4. **Alternative.** Say when a simpler choice is correct.

For example: "A crash between the database write and the publish loses the event. The outbox pattern writes the event to a table in the same transaction, and a relay publishes it later. The cost is a relay to operate and duplicate deliveries, so consumers must be idempotent. If losing one event in a million is acceptable, a direct publish is simpler."

## Patterns that are often confused

| Pair | Difference |
|---|---|
| Circuit breaker and retry | A retry repeats a call, while a circuit breaker stops calls |
| CQRS and event sourcing | CQRS separates read and write models, while event sourcing changes how writes are stored |
| API gateway and service mesh | A gateway manages traffic entering the system, while a mesh manages traffic between services |
| Saga and two-phase commit | A saga commits each step and compensates later, while two-phase commit holds locks until all agree |

## Go deeper

- [Grokking Microservices Design Patterns](https://www.designgurus.io/course/grokking-microservices-design-patterns?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-patterns-at-a-glance) teaches each pattern with code and diagrams.
- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-patterns-at-a-glance) shows how to combine patterns in an interview answer.
- Reference: the [design-patterns repository](https://github.com/design-gurus/design-patterns) lists settings and failure modes for each pattern.
- Free article: [Microservices Design Patterns: 19 Essential Patterns](https://www.designgurus.io/blog/19-essential-microservices-patterns-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-patterns-at-a-glance), a longer description of each pattern in this table.
