# Deployment and Testing: Interview Questions

Independent deployment is the main benefit of microservices: one team releases one service without waiting for any other team. These questions test whether a candidate knows how to release safely and how to test a service whose dependencies belong to other teams.

## Questions and answers

### 1. What does independent deployability mean?

A service is independently deployable when its team can release a new version at any time without a matching release from another service. This requires backward-compatible APIs and no shared database. If two services must always be released together, they behave as one deployment unit and should be treated as one.

### 2. Why are containers common in microservices?

A container packages a service with its runtime and libraries into one image that runs the same way on a laptop and in production. Services written in different languages can then be built, shipped, and started through one process. Containers also start in seconds, which makes scaling out and replacing failed instances fast.

### 3. What does an orchestrator such as Kubernetes provide?

An orchestrator schedules containers onto machines and keeps the requested number of instances running. It restarts failed instances, replaces instances gradually during a release, scales by load, and gives each service a stable name. Without one, a team would write and maintain these functions itself for every service.

### 4. What is the difference between rolling, blue-green, and canary deployment?

A rolling deployment replaces instances a few at a time. A blue-green deployment runs the new version beside the old one and switches all traffic at once, which allows instant rollback at double the capacity cost. A canary deployment sends a small share of traffic, such as 5 percent, to the new version and increases it while error rates stay normal.

### 5. What is a feature flag, and how does it change a release?

A feature flag is a configuration switch that turns a code path on or off at run time. It separates deployment, which puts code on servers, from release, which exposes behavior to users. A team can deploy unfinished code safely, enable it for 1 percent of users, and disable it in seconds without a rollback.

### 6. How do you change a database schema without downtime?

Use the expand and contract method. First expand: add the new column or table while the old code continues to work. Then deploy code that writes to both and backfill existing rows. Next move reads to the new structure. Finally contract: remove the old column once no deployed version uses it. Each step is backward compatible.

### 7. What does the test pyramid look like for microservices?

The base holds many fast unit tests of business logic. Above them are integration tests of the service with its own database and broker, and contract tests of its API. The top holds a small number of end-to-end tests across services. End-to-end tests are slow and fail for unrelated reasons, so 10 to 20 critical paths are usually enough.

### 8. What is consumer-driven contract testing?

Each consumer writes down the requests it sends and the response fields it reads, and this record is the contract. The provider runs every consumer's contract in its own build pipeline. A provider change that would break a consumer fails the provider's build before release. This finds integration errors without starting both services together.

### 9. How do you test a service that depends on five others?

Replace the five dependencies with test doubles, which are simple stand-ins that return prepared responses. Contract tests keep the doubles honest, since they verify that the real providers still return those responses. The service can then be tested alone in minutes. A shared staging environment remains useful only for a few end-to-end checks.

### 10. How do you roll back a release that included a data change?

Code can be rolled back in minutes, but data written in the new format cannot be removed safely. The expand and contract method avoids the problem, because the previous code version still works with the expanded schema. When that is not possible, roll forward with a fix instead of rolling back.

### 11. How do you keep configuration and secrets out of the image?

Store configuration outside the image, in environment variables or a configuration service, so one image runs in every environment. Store secrets such as database passwords in a secrets manager that encrypts them and records every access. Inject them at startup, and rotate them on a schedule such as every 90 days.

## Deployment strategies compared

| Strategy | Rollback speed | Extra capacity | Exposure to a bad release |
|---|---|---|---|
| Rolling | Minutes | About 10 to 25 percent | Grows as instances are replaced |
| Blue-green | Seconds | 100 percent | All users at the switch |
| Canary | Seconds | About 5 to 10 percent | Only the canary share |

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-deployment-and-testing) covers deployment, testing, and schema changes with worked examples.
- [Grokking Microservices Design Patterns](https://www.designgurus.io/course/grokking-microservices-design-patterns?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-deployment-and-testing) covers the deployment and migration patterns one at a time.
- Next topic: [Migrating from a monolith](migrating-from-a-monolith.md).
