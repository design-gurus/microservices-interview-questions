# Common Mistakes in Microservices Interviews

Most candidates know the definitions. They lose points by describing benefits with no costs, or by applying a pattern where a simpler design works. This sheet lists the 12 mistakes that appear most often, each with the correction an interviewer wants to hear.

## Mistakes about the architecture

| Mistake | Why it costs points | Say this instead |
|---|---|---|
| Proposing microservices for every system | It shows no judgment about cost | "For a team of 10, I would start with a modular monolith and extract on a measured trigger." |
| Drawing services by technical layer | A "database service" couples every feature | "I split by business capability: ordering, payment, shipping." |
| Sharing one database between services | A schema change breaks other teams | "Each service owns its tables, and others use its API or its events." |
| Creating very small services | Every request crosses many network calls | "A service should complete a business operation with at most a few calls." |

## Mistakes about data

| Mistake | Why it costs points | Say this instead |
|---|---|---|
| Assuming a transaction across services | No such transaction exists | "This spans three databases, so I use a saga with compensations." |
| Calling a compensation a rollback | The original change was already visible | "The compensation is a new transaction, for example a refund." |
| Publishing an event after the database commit | A crash between the two loses the event | "I write the event to an outbox table in the same transaction." |
| Ignoring duplicates | Brokers deliver at least once | "The consumer stores processed message identifiers and skips repeats." |

## Mistakes about failure

| Mistake | Why it costs points | Say this instead |
|---|---|---|
| No timeout on remote calls | One slow service stops its callers | "Each call has a timeout slightly above the dependency's p99 latency." |
| Retrying everything | Retries multiply load and repeat side effects | "I retry idempotent calls only, three times at most, with backoff and jitter." |
| Naming a circuit breaker with no fallback | The user still receives an error | "When the breaker is open, the page shows cached recommendations." |
| Long synchronous chains | Availability multiplies down the chain | "The user waits for payment only, and email goes through a queue." |

## Mistakes in how the answer is given

**Listing patterns without a problem.** A candidate who says "I would use CQRS, event sourcing, and a service mesh" has described a tool list. Name the problem first, and then name the pattern that removes it.

**Giving no numbers.** "It scales better" is weaker than "search receives 15 times the traffic of checkout, so it should scale alone". A number makes the reasoning checkable.

**Skipping operations.** A design with 15 services needs deployment, monitoring, and on-call ownership. One sentence on tracing and alerting shows production experience.

**Not asking questions.** The right design depends on team size, traffic, and consistency needs. Two clarifying questions at the start prevent a design that answers the wrong problem.

## A weak and a strong answer

The question is: "How would you keep the order and payment services consistent?"

> Weak: "I would use a distributed transaction so both commit together."

> Strong: "A transaction cannot span both databases, so I would use a saga. The order is created as PENDING, payment charges the card, and the order is then confirmed. If the charge fails, the order is canceled as a compensation. Each service publishes through an outbox, and consumers are idempotent because delivery is at least once. The cost is that an order is visible as PENDING for a second or two."

The strong answer names the constraint, the mechanism, the failure path, and the cost in about 20 seconds.

## A checklist before the interview

1. Can you explain each of the [20 patterns](patterns-at-a-glance.md) with its cost?
2. Can you draw one saga with its compensations?
3. Can you state when a monolith is the better choice?
4. Can you describe what happens when one dependency slows down?
5. Can you explain how you would find a slow request across six services?

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-common-mistakes) pairs each topic with worked designs and assessments that expose these mistakes.
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-common-mistakes) covers the structure of a full design answer.
- Practice: the [questions by topic](../questions/README.md).
