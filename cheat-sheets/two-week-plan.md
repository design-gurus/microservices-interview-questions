# Two-Week Preparation Plan

This plan covers every topic in this repository in 14 days at about one hour a day. It assumes working knowledge of HTTP, databases, and one backend language. Each day has one reading task and one task that produces output, because recall improves when an answer is spoken or drawn.

## Week 1: structure and data

| Day | Read | Produce |
|---|---|---|
| 1 | [Monolith vs microservices](monolith-vs-microservices.md) | A 60-second spoken answer to "when would you not use microservices?" |
| 2 | [Service boundaries](../questions/service-boundaries.md) | Boundaries for an online store, with the data each service owns |
| 3 | [Communication](../questions/communication.md) | A table of every call in your store design, marked synchronous or asynchronous |
| 4 | [Synchronous vs asynchronous communication](synchronous-vs-asynchronous-communication.md) | The availability of your longest call chain, calculated |
| 5 | [Data consistency and sagas](../questions/data-consistency-and-sagas.md) | One saga drawn with a compensation for each step |
| 6 | [Saga: choreography vs orchestration](saga-choreography-vs-orchestration.md) | The same saga drawn the other way |
| 7 | Review | Answer 10 questions aloud from days 2 to 5 without notes |

## Week 2: operation and practice

| Day | Read | Produce |
|---|---|---|
| 8 | [Resilience](../questions/resilience.md) | Timeout, retry, and breaker settings for one dependency, with reasons |
| 9 | [API gateway and service discovery](../questions/api-gateway-and-service-discovery.md) | The path of one request from the client to the database, hop by hop |
| 10 | [Observability](../questions/observability.md) | Three metrics and one alert for your payment service |
| 11 | [Deployment and testing](../questions/deployment-and-testing.md) | A plan to rename one database column with no downtime |
| 12 | [Migrating from a monolith](../questions/migrating-from-a-monolith.md) and [Security](../questions/security.md) | An extraction order for a monolith you know |
| 13 | [Patterns at a glance](patterns-at-a-glance.md) | Each of the 20 patterns explained in 30 seconds |
| 14 | [Common mistakes](common-mistakes.md) | One full design, timed at 40 minutes |

## How to use each hour

| Minutes | Activity |
|---|---|
| 0 to 20 | Read the page for the day |
| 20 to 40 | Cover the answers and answer each question aloud |
| 40 to 55 | Complete the output task on paper |
| 55 to 60 | Write down the two questions you answered worst |

Start the next day by answering those two questions again.

## The day 14 design

Pick one of these systems and design it with services in 40 minutes: a food delivery platform, a ticket booking site, or a ride-hailing service. Use this time split.

| Minutes | Step |
|---|---|
| 0 to 5 | Clarify requirements and scale |
| 5 to 12 | Draw service boundaries and data ownership |
| 12 to 20 | Mark each call synchronous or asynchronous |
| 20 to 30 | Design the saga for the main write path |
| 30 to 36 | Add timeouts, breakers, and fallbacks |
| 36 to 40 | State how you would deploy and observe it |

Record yourself if possible. Then check the recording against the [common mistakes](common-mistakes.md) sheet.

## If you have less time

| Time available | Cover |
|---|---|
| 3 days | Days 1, 5, and 8, then the patterns table |
| 1 week | Week 1, then days 8 and 13 |
| 1 evening | [Patterns at a glance](patterns-at-a-glance.md) and [Common mistakes](common-mistakes.md) |

## How to measure readiness

You are ready when you can do these four things without notes.

1. Answer 8 of 10 randomly chosen questions from this repository with the cost included.
2. Draw a saga with compensations in under 5 minutes.
3. Explain what happens to callers when one service becomes slow.
4. Give a reasoned answer for when a monolith is the better choice.

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-two-week-plan) gives the full lessons, diagrams, and chapter assessments behind each day of this plan.
- [Grokking Microservices Design Patterns](https://www.designgurus.io/course/grokking-microservices-design-patterns?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-two-week-plan) is the follow-up for day 13.
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-two-week-plan) prepares the general design round that day 14 imitates.
- Free article: [The Microservices System Design Interview: A Six-Step Framework](https://www.designgurus.io/blog/microservices-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=cheat-sheets-two-week-plan), a method to practice during the design days of the plan.
