# Security: Interview Questions

A monolith has one entry point to protect. A microservices system has dozens of services that call each other over a network, and each call is a possible point of attack. These questions test whether a candidate can secure both the edge and the internal traffic.

## Questions and answers

### 1. What is the difference between authentication and authorization?

Authentication establishes who the caller is, for example by checking a password or a signed token. Authorization decides what that caller is allowed to do, such as reading order 42. Authentication usually happens once at the edge. Authorization must also happen inside each service, because only the service knows its own data rules.

### 2. Where should authentication happen?

The API gateway should authenticate every external request and reject invalid ones before they reach a service. The gateway then passes the verified identity to the services, usually as a signed token. Services must still verify that token themselves. A service that trusts any internal caller is exposed as soon as one other service is compromised.

### 3. What is a JSON Web Token, and why is it used?

A JSON Web Token (JWT) is a signed piece of JSON that carries claims such as the user identifier, roles, and an expiry time. Any service can verify the signature locally with the issuer's public key, with no call to the identity service. This removes a network dependency from every request.

### 4. What is the weakness of a JWT, and how do you limit it?

A JWT remains valid until it expires, so it cannot be withdrawn easily after a user logs out or an account is disabled. Keep access tokens short-lived, for example 5 to 15 minutes, and issue new ones with a refresh token that the identity service can revoke. A denylist of revoked token identifiers is a second option at higher cost.

### 5. How do OAuth 2.0 and OpenID Connect fit in?

OAuth 2.0 is a standard for delegated authorization: it lets an application obtain an access token to call an API on a user's behalf. OpenID Connect adds an identity layer on top, which returns an ID token that describes the authenticated user. Together they let one identity provider handle login for every service.

### 6. How do services authenticate each other?

Mutual TLS (mTLS) is the common method. In ordinary TLS only the server presents a certificate, while in mutual TLS both sides present one, so each proves its identity to the other. A service mesh can issue, rotate, and check these certificates automatically. A second method is a signed service token with a narrow scope.

### 7. What is zero trust?

Zero trust is a security model in which no request is trusted because of its network location. Every call is authenticated, authorized, and encrypted, even inside the private network. The model assumes that an attacker may already be inside. It limits the damage one compromised service can do to the permissions of that one service.

### 8. How do you pass the user's identity through a chain of services?

Forward the user's token, or a token exchanged for a narrower one, with each downstream call. Each service then authorizes against the original user and not against the calling service. Without this, a low-privilege user could reach data through a high-privilege internal service, a flaw known as the confused deputy problem.

### 9. How should secrets be managed?

Keep secrets such as database passwords and API keys in a secrets manager, never in source code or container images. Give each service its own credentials with the minimum permissions it needs. Rotate them on a schedule, for example every 90 days, and immediately after any suspected exposure. Short-lived credentials issued at startup reduce the value of a stolen secret.

### 10. How do you protect an API from abuse?

Apply rate limits per client and per user at the gateway. Validate the size and format of every input, and reject request bodies above a set limit such as 1 megabyte. Require pagination on list endpoints. Log failed authentication attempts and alert on unusual volumes, such as 50 failures from one address within a minute.

### 11. How do you limit the damage if one service is compromised?

Apply least privilege at every layer. The service's database account can reach only its own schema, and its network policy allows calls only to the services it needs. Its credentials are short-lived and scoped. With these limits, an attacker who controls the notification service cannot read the payment database.

## Where each control sits

| Layer | Control |
|---|---|
| Edge | TLS, authentication, rate limiting, input size limits |
| Between services | Mutual TLS, scoped tokens, network policies |
| Inside a service | Authorization on every operation, input validation |
| Data | Encryption at rest, separate credentials per service |

## Go deeper

- [Grokking Microservices for System Design Interviews](https://www.designgurus.io/course/grokking-microservices-for-system-design-interviews?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-security) covers authentication, token design, and service-to-service security in full designs.
- Lesson: [Securing Service-to-Service Calls](https://www.designgurus.io/course-play/grokking-microservices-for-system-design-interviews/doc/securing-servicetoservice-calls?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-security), in Grokking Microservices for System Design Interviews
- [Grokking Modern API Design Interview](https://www.designgurus.io/course/grokking-modern-api-design-interview?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-security) covers API authentication and authorization design.
- Back to the [question index](README.md).
- Free answer: [Principles for securing microservices architectures](https://www.designgurus.io/answers/detail/principles-for-securing-microservices-architectures?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-security)
- Free answer: [What is mTLS and when to use it?](https://www.designgurus.io/answers/detail/what-is-mtls-and-when-to-use-it?utm_source=github&utm_medium=repo&utm_campaign=microservices-interview-questions&utm_content=questions-security)
