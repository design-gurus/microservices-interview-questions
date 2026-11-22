# Observability: Interview Questions

Observability is the ability to explain what a system is doing from the data it emits. One user request may cross six services, so a failure can no longer be found by reading one log file. These questions test whether a candidate can find a fault in a distributed system.

## Questions and answers

### 1. What are the three pillars of observability?

The three pillars are logs, metrics, and traces. A log is a timestamped record of one event. A metric is a number measured over time, such as requests per second. A trace is the record of one request as it passes through every service. Metrics show that a problem exists, traces show where, and logs show why.

### 2. How is observability different from monitoring?

Monitoring checks conditions that were defined in advance, such as "error rate above 1 percent". Observability allows engineers to ask new questions about behavior that nobody predicted, without shipping new code. In practice this requires detailed, structured data that can be filtered by user, region, or version.

### 3. What is distributed tracing?

Distributed tracing assigns a trace identifier to each incoming request and passes it to every service the request reaches. Each service records a span, which is one timed unit of work with a start, an end, and a parent. The collected spans form a tree that shows which service consumed the time or returned the error.

### 4. What is a correlation identifier?

A correlation identifier is a unique value attached to a request at the entry point and copied into every log line and every outgoing call. Searching the logs for that one value returns the complete history of the request across all services. It must also travel inside message headers, or the history breaks at the first queue.

### 5. Why use structured logs?

A structured log is written as key-value data, usually JSON, instead of free text. A log system can then filter by fields, such as all errors for one order identifier in the last 10 minutes. Free-text logs require fragile text patterns for the same query, and those patterns break when a developer rewords a message.

### 6. Which metrics should every service expose?

The RED method covers request-driven services: Rate of requests, Errors, and Duration. The four golden signals from Google's site reliability practice add saturation, which measures how full a resource is. For a queue consumer, add queue depth and message age, because a consumer can be healthy and still far behind.

### 7. Why measure percentiles instead of the average?

An average hides slow requests. A service with an average of 80 milliseconds may still serve 1 percent of requests in 3 seconds. The 99th percentile, written p99, is the latency that 99 percent of requests stay under. When a page calls 10 services, most users experience at least one request near the p99.

### 8. What are SLI, SLO, and error budget?

A service level indicator (SLI) is a measured value, such as the share of requests that succeed within 300 milliseconds. A service level objective (SLO) is the target for that value, such as 99.9 percent over 30 days. The error budget is the allowed failure, here 0.1 percent, or about 43 minutes a month. Teams slow releases when the budget is spent.

### 9. What makes a good alert?

A good alert reports a symptom that users feel, requires a human action, and links to a runbook. "Checkout error rate above 2 percent for 5 minutes" meets that test, while "CPU above 80 percent" often does not. Alerts on causes produce noise, and engineers start ignoring a channel that pages them 20 times a week.

### 10. Checkout latency doubled an hour ago. How do you find the cause?

Start with the dashboard for the user-facing endpoint to confirm the time the change began. Open slow traces from that period and find the span that grew. Then compare that service's metrics with its deployments and its dependencies around the same minute. Finally read its logs, filtered by the trace identifiers of the slow requests.

### 11. How do you control the cost of tracing at high volume?

Record only a sample of traces. Head-based sampling decides at the entry point, for example keeping 1 percent of requests, which is cheap but may miss rare errors. Tail-based sampling decides after the request completes and keeps every trace that was slow or failed. Tail-based sampling costs more, because spans must be buffered first.

## What to say for each signal

| Signal | Answers | Example |
|---|---|---|
| Metric | Is something wrong? | p99 latency rose from 200 to 900 milliseconds |
| Trace | Where is it wrong? | The inventory span takes 700 milliseconds |
| Log | Why is it wrong? | "Connection pool exhausted, 50 of 50 in use" |

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-observability) covers logging, metrics, tracing, and debugging across services.
- Lesson: [Observability: Logs, Metrics, Traces](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/observability-logs-metrics-traces?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-observability), in Grokking Microservices for System Design Interviews
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-observability) shows where monitoring fits in a complete design answer.
- Next topic: [Deployment and testing](deployment-and-testing.md).
- Free article: [OpenTelemetry: A Beginner's Guide to Modern Observability](https://www.designgurus.io/blog/guide-to-modern-observability?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-observability), which covers traces, metrics, and logs.
