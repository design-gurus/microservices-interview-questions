# API Gateway and Service Discovery: Interview Questions

An API gateway is the single entry point that clients call instead of calling services directly. Service discovery is the mechanism by which one service finds the current network addresses of another. Both exist because service instances start, stop, and move many times a day.

## Questions and answers

### 1. What does an API gateway do?

A gateway receives every external request and routes it to the correct service. It also handles concerns that every service would otherwise repeat: authentication, rate limiting, TLS termination, and request logging. Clients therefore see one host name and one security model, while the services behind the gateway can be split or merged freely.

### 2. What should not go into the gateway?

Business logic should stay out of the gateway. A gateway that validates orders or computes prices becomes a shared component that every team must change, which recreates the monolith at the edge. A practical rule is that the gateway may inspect a request but should not understand the business meaning of its body.

### 3. What is the Backend for Frontend pattern?

Backend for Frontend (BFF) creates one gateway per client type, such as one for the mobile app and one for the web app. Each gateway returns exactly the data that its client needs, in the shape it needs. The team that owns the client usually owns its BFF, which removes a shared gateway as a coordination point.

### 4. What is API composition at the gateway?

API composition means the gateway calls several services and merges their responses into one. A product page may need data from the catalog, pricing, and review services. One composed call replaces three client calls, which matters on mobile networks. The gateway should call the services in parallel and decide which parts are optional if one fails.

### 5. Why is service discovery needed?

Instances receive new IP addresses whenever they restart or scale, so a fixed address list becomes wrong within hours. A service registry is a database of the healthy instances of each service. Instances register on startup and are removed when their health checks fail, so callers always find a current address.

### 6. What is the difference between client-side and server-side discovery?

In client-side discovery the caller queries the registry and selects an instance itself, which removes a network hop but requires a discovery library in every language. In server-side discovery the caller sends the request to a load balancer or platform address, and that component selects the instance. Kubernetes Services are a common example of the server-side form.

### 7. How does a registry know an instance is unhealthy?

Each instance either sends a heartbeat every few seconds or exposes a health endpoint that the registry polls. After a set number of missed checks, such as three in a row, the registry removes the instance. A good health endpoint reports whether the instance can serve traffic, not only whether the process is running.

### 8. How does a gateway differ from a load balancer and a reverse proxy?

A load balancer distributes requests across identical instances of one service. A reverse proxy forwards requests to servers behind it and can add caching or TLS. An API gateway is a reverse proxy with API-specific features, such as routing by path to different services, authentication, quotas per client, and response composition.

### 9. What is a service mesh, and how does it relate to a gateway?

A service mesh places a small proxy, called a sidecar, beside every service instance. The proxies handle retries, timeouts, encryption, and metrics for calls between services, with no change to application code. A gateway manages traffic entering the system, often called north-south traffic. A mesh manages traffic between services, often called east-west traffic.

### 10. Is the gateway a single point of failure?

It is a single logical entry point, so it must not be a single instance. Run at least three stateless gateway instances across availability zones behind a load balancer. Keep the gateway's own dependencies few, and cache authentication keys locally so that a slow identity service does not stop all traffic.

### 11. Where do you apply rate limiting?

Apply a coarse limit per client at the gateway, for example 100 requests per minute per API key, to protect the whole system. Apply finer limits inside individual services for expensive operations, since only the service knows their cost. The counters need a shared store when several gateway instances enforce one limit.

## Request path in one view

| Hop | Component | Responsibility |
|---|---|---|
| 1 | Load balancer | Spread traffic across gateway instances |
| 2 | API gateway | Authenticate, limit, route by path |
| 3 | Discovery | Resolve "orders" to a healthy instance |
| 4 | Service | Authorize and run the business logic |

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-api-gateway-and-service-discovery) covers gateways, BFF, discovery, and the service mesh with diagrams.
- [Grokking Modern API Design Interview](https://www.designgurus.io/course/grokking-modern-api-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-api-gateway-and-service-discovery) covers the API contract that the gateway exposes.
- Related repository: [api-design-interview](https://github.com/design-gurus/api-design-interview).
