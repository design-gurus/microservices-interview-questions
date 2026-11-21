# Monolith vs Microservices

A monolith is one application built and deployed as a single unit. A microservices system is a set of small services that are deployed independently and own their data. Neither is better in general: a monolith is cheaper to build and operate, and microservices are cheaper to change once many teams work on one product.

## Side by side

| Dimension | Monolith | Microservices |
|---|---|---|
| Deployment | One unit, released together | Each service released separately |
| Calls between modules | In-process function calls | Network calls that can fail or time out |
| Data | One database, real transactions, joins | One database per service, sagas, no cross-service joins |
| Consistency | Immediate | Often eventual between services |
| Scaling | The whole application scales together | Each service scales by its own load |
| Fault isolation | One fault can stop everything | A fault can be contained in one service |
| Technology | One stack | A stack per service, if the team chooses |
| Testing | One application to start | Contract tests and test doubles needed |
| Debugging | One log, one stack trace | Tracing across services required |
| Operations | One pipeline, one runtime | Pipelines, discovery, and monitoring per service |
| Team fit | One to three teams | Many teams with clear ownership |

## What microservices cost

A candidate who lists only benefits appears inexperienced. Microservices add these costs from the first day.

- **Network failure.** Every call needs a timeout, a retry policy, and a fallback.
- **Lost transactions.** An operation across services needs a saga with compensations.
- **Operational load.** Twenty services mean 20 pipelines, 20 dashboards, and 20 on-call runbooks.
- **Harder debugging.** A single request must be traced across several services.
- **Slower local development.** A developer cannot always run the full system on one machine.

## When each one fits

| Situation | Better choice | Reason |
|---|---|---|
| New product, domain still changing | Monolith | Boundaries drawn early are usually wrong |
| Fewer than about 20 engineers | Monolith | Coordination cost is still low |
| Many teams waiting on one release | Microservices | Independent deployment removes the queue |
| One module needs ten times the capacity | Microservices | That module can scale alone |
| Strict transactions across most data | Monolith | Sagas would cover most operations |
| Parts with different availability targets | Microservices | Critical paths can be isolated |

## The middle option: a modular monolith

A modular monolith is one deployable application with enforced module boundaries. Each module has a public interface and private tables. It provides clear ownership and real transactions, with no network calls. Many teams should try it before moving to microservices, because a clean module can be extracted later in days.

## A worked decision

Consider an online store with 12 engineers in 2 teams. It deploys twice a week, and a full build takes 8 minutes. No module has a special scaling need. A monolith fits, because no measured problem exists that microservices would remove.

Two years later the same store has 90 engineers in 11 teams. Releases wait three days in a shared queue. Search receives 15 times the traffic of checkout. Now two measured problems exist: deployment contention and uneven load. Extracting search first addresses both, while most of the application can remain a modular monolith.

## What to say in an interview

When asked "monolith or microservices?", do not answer with one word. A strong answer follows three steps.

1. Ask about team size, release frequency, and scaling differences between modules.
2. State the default: start with a well-structured monolith.
3. Name the trigger that would justify extraction, with a number attached.

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-monolith-vs-microservices) covers this decision and the migration that follows it.
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-monolith-vs-microservices) shows how the choice appears inside a full design round.
- Questions: [Migrating from a monolith](../questions/migrating-from-a-monolith.md) and [Service boundaries](../questions/service-boundaries.md).
