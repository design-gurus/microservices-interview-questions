# Resilience: Interview Questions

Resilience is the ability of a system to keep serving requests when one of its parts fails or slows down. In a microservices system a slow dependency is more dangerous than a dead one, and most of these questions test whether the candidate understands why.

## Questions and answers

### 1. Why does every remote call need a timeout?

Without a timeout, a caller waits for a slow service indefinitely and holds a thread or connection the whole time. Once all threads are waiting, the caller stops serving every request, including those that never needed the slow service. Set the timeout slightly above the dependency's 99th percentile latency, for example 300 milliseconds for a service that normally answers in 200.

### 2. When is it safe to retry a failed call?

A retry is safe when the failure is temporary and the operation is idempotent. An idempotent operation produces the same result whether it runs once or several times. Timeouts and 503 responses are worth retrying, while a 400 response will fail again. Limit retries to two or three attempts.

### 3. What are exponential backoff and jitter?

Exponential backoff doubles the wait between retries, for example 100, 200, then 400 milliseconds, so a struggling service receives fewer requests. Jitter adds a random amount to each wait. Without jitter, thousands of clients that failed at the same moment retry at the same moment and overload the service again.

### 4. What is a retry storm?

A retry storm is a surge of load caused by retries at several layers. If three layers each retry three times, one user request can become 27 calls to the lowest service. The fixes are to retry at one layer only, to cap the share of traffic that may be retries at about 10 percent, and to use backoff with jitter.

### 5. How does a circuit breaker work?

A circuit breaker is a wrapper around a remote call that stops calling a failing service. It has three states. In the closed state calls pass through and failures are counted. When failures cross a threshold, such as 50 percent of the last 20 calls, it opens and rejects calls immediately. After a wait it becomes half-open and lets a few trial calls through to test recovery.

### 6. What is the bulkhead pattern?

The bulkhead pattern gives each dependency its own limited pool of threads or connections. If the recommendation service slows down, it can exhaust only its own pool of, for example, 20 threads. The remaining threads keep serving checkout and search. The name comes from the partitions that divide a ship's hull into separate compartments.

### 7. What is an idempotency key?

An idempotency key is a unique identifier that the client generates once and sends with every attempt of the same request. The server stores the key with the result of the first attempt. When a retry arrives with the same key, the server returns the stored result instead of repeating the work. This makes a payment request safe to retry.

### 8. What is graceful degradation?

Graceful degradation means returning a reduced but useful response when a dependency fails. A product page can show a cached price, hide the recommendations panel, or display a default list of popular items. The candidate should state which features are essential and which may be dropped, because a fallback for a payment is rarely acceptable.

### 9. How does a failure cascade, and how do you stop it?

A cascade starts when one slow service causes its callers to queue requests, which makes the callers slow for their own callers. The failure then spreads upward until the entry point stops responding. Timeouts, circuit breakers, bulkheads, and load shedding each interrupt the spread at a different point.

### 10. What are load shedding and backpressure?

Load shedding means a service rejects excess requests quickly, usually with a 429 or 503 status, to protect the requests it has already accepted. Backpressure means a consumer signals the producer to slow down, for example through a bounded queue that blocks when full. Both replace unbounded queues, which convert overload into long latency and then into failure.

### 11. How should timeouts be set across a chain of calls?

Each service should receive a deadline from its caller and pass the remaining time downstream. If the user-facing limit is 1 second and the first service spent 300 milliseconds, the next call receives 700. A downstream service that sees an expired deadline stops working, since no one is waiting for the result.

## The four protections side by side

| Protection | Stops | Typical setting |
|---|---|---|
| Timeout | Waiting indefinitely | Slightly above 99th percentile latency |
| Retry with backoff and jitter | Temporary errors | 2 or 3 attempts, idempotent calls only |
| Circuit breaker | Calling a failing service | Open at 50 percent failures, test after 30 seconds |
| Bulkhead | One dependency using all resources | A separate pool per dependency |

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-resilience) applies these protections to full designs and shows the failure each one prevents.
- [Grokking Microservices Design Patterns](https://www.designgurus.io/course/grokking-microservices-design-patterns?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-resilience) covers circuit breaker, bulkhead, and retry as separate patterns.
- Reference: [design-patterns repository](https://github.com/design-gurus/design-patterns) for settings and failure modes of each pattern.
