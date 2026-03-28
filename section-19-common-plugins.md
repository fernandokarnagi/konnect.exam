# Section 19: Common Plugins — Authentication, Security, Traffic Control, Analytics, Transformations, and Logging

> **Exam Theme:** Knowing the key plugins in each category, what they do, their configuration, and when to use them.

---

## Q1. Which plugin enforces API key authentication on a route or service?

**A)** `jwt`
**B)** `basic-auth`
**C)** `key-auth`
**D)** `oauth2`

**Answer: C**
*The `key-auth` plugin requires clients to present an API key (in a header or query parameter). Keys are credentials stored on Consumer objects.*

---

## Q2. What does the JWT plugin validate?

**A)** Username and password in the Authorization header
**B)** A JSON Web Token's signature, expiry, and claims
**C)** An OAuth2 access token against an introspection endpoint
**D)** An HMAC-signed request body

**Answer: B**
*The `jwt` plugin validates the signature (using the secret/key stored on a Consumer credential), the expiry (`exp` claim), and optional custom claims.*

---

## Q3. Which plugin enables full OAuth 2.0 authorization flows (authorization code, client credentials, etc.) directly on Kong?

**A)** `key-auth`
**B)** `jwt`
**C)** `oauth2`
**D)** `openid-connect`

**Answer: C**
*The `oauth2` plugin implements OAuth 2.0 grant flows within Kong itself, issuing and validating access tokens without a separate authorization server.*

---

## Q4. The `openid-connect` (OIDC) plugin is used to:

**A)** Generate API keys for consumers
**B)** Delegate authentication to an external Identity Provider using OIDC/OAuth2
**C)** Validate API keys locally
**D)** Encrypt request payloads

**Answer: B**
*The `openid-connect` plugin integrates with external IdPs (Okta, Azure AD, Keycloak, etc.) for authentication and token introspection, offloading identity management from the API.*

---

## Q5. Which security plugin blocks requests from specific IP addresses or CIDR ranges?

**A)** `bot-detection`
**B)** `acl`
**C)** `ip-restriction`
**D)** `cors`

**Answer: C**
*The `ip-restriction` plugin allows or denies requests based on the client IP address using allow-lists and deny-lists, supporting individual IPs and CIDR notation.*

---

## Q6. What does the `cors` plugin do?

**A)** Encrypts cross-origin requests
**B)** Adds Cross-Origin Resource Sharing headers to responses, controlling browser cross-origin access
**C)** Blocks all cross-origin requests
**D)** Maps CORS preflight requests to upstream services

**Answer: B**
*The `cors` plugin adds the appropriate `Access-Control-Allow-*` response headers, enabling (or restricting) browser cross-origin requests per configuration.*

---

## Q7. The `acl` plugin controls access by:

**A)** Validating API keys per route
**B)** Restricting access to a route/service to Consumers belonging to specified groups
**C)** Blocking IPs in a deny-list
**D)** Enforcing JWT claims

**Answer: B**
*The `acl` plugin works with an authentication plugin. It checks whether the authenticated Consumer is a member of an allowed (or blocked) group.*

---

## Q8. Which plugin limits the number of requests a consumer or client can make within a time window?

**A)** `request-size-limiting`
**B)** `rate-limiting`
**C)** `proxy-cache`
**D)** `response-ratelimiting`

**Answer: B**
*The `rate-limiting` plugin enforces limits per second/minute/hour/day/month/year, per consumer, credential, IP, or globally. It returns `429 Too Many Requests` when exceeded.*

---

## Q9. What is the difference between `rate-limiting` and `rate-limiting-advanced`?

**A)** No difference — they are aliases
**B)** `rate-limiting-advanced` supports sliding window counters, Redis cluster, and consumer group overrides
**C)** `rate-limiting` is Enterprise-only
**D)** `rate-limiting-advanced` only works with OAuth2

**Answer: B**
*`rate-limiting-advanced` (Enterprise) adds sliding window algorithms, Redis Cluster support, consumer group tier overrides, and more granular controls than the OSS `rate-limiting` plugin.*

---

## Q10. Which plugin caches upstream responses to reduce backend load?

**A)** `response-transformer`
**B)** `proxy-cache`
**C)** `request-termination`
**D)** `acl`

**Answer: B**
*The `proxy-cache` plugin stores upstream responses in memory or Redis. Subsequent identical requests are served from cache without hitting the upstream, reducing latency and load.*

---

## Q11. The `request-termination` plugin is most useful for:

**A)** Terminating TLS connections
**B)** Immediately returning a configured response without forwarding to the upstream (maintenance mode, circuit breaker)
**C)** Terminating long-running upstream requests
**D)** Logging terminated requests

**Answer: B**
*`request-termination` returns a static response (status code + body) without proxying. Useful for maintenance pages, temporary service outages, or testing.*

---

## Q12. Which plugin restricts request body size to prevent large payload attacks?

**A)** `ip-restriction`
**B)** `bot-detection`
**C)** `request-size-limiting`
**D)** `acl`

**Answer: C**
*The `request-size-limiting` plugin rejects requests whose body exceeds a configured size (in MB), protecting upstream services from oversized payloads.*

---

## Q13. What does the `bot-detection` plugin do?

**A)** Blocks all non-browser user agents
**B)** Detects and optionally blocks or allows known bot User-Agent strings
**C)** Detects DDoS patterns and rate-limits bots
**D)** Integrates with an external bot management provider

**Answer: B**
*The `bot-detection` plugin matches the `User-Agent` header against lists of known bot signatures and takes configured action (allow or block) based on the match.*

---

## Q14. Which plugin category does `request-transformer` belong to?

**A)** Authentication
**B)** Security
**C)** Transformations
**D)** Analytics & Monitoring

**Answer: C**
*`request-transformer` is a Transformation plugin that modifies request headers, query strings, body parameters, or the URL before forwarding to the upstream.*

---

## Q15. What does `response-transformer` enable?

**A)** Rate limiting based on response codes
**B)** Adding, removing, or replacing headers and body content in the upstream response before sending to the client
**C)** Transforming authentication credentials in the response
**D)** Converting XML responses to JSON automatically

**Answer: B**
*`response-transformer` modifies response headers, adds/removes body fields, or replaces values in the response before the client receives it.*

---

## Q16. Which plugin collects and sends request/response analytics to a Prometheus endpoint?

**A)** `file-log`
**B)** `datadog`
**C)** `prometheus`
**D)** `statsd`

**Answer: C**
*The `prometheus` plugin exposes Kong metrics (request count, latency, upstream health, etc.) in Prometheus format at the `/metrics` endpoint on the Status API port.*

---

## Q17. The `datadog` plugin sends metrics to:

**A)** A local file
**B)** A Datadog Agent (UDP) for ingestion into Datadog's monitoring platform
**C)** An HTTP endpoint
**D)** A Kafka topic

**Answer: B**
*The `datadog` plugin emits StatsD-format metrics to a Datadog Agent over UDP. The agent forwards them to Datadog's cloud for monitoring and dashboards.*

---

## Q18. Which logging plugin writes request/response details to a local file on the Kong node?

**A)** `http-log`
**B)** `tcp-log`
**C)** `file-log`
**D)** `syslog`

**Answer: C**
*The `file-log` plugin writes JSON-formatted log entries to a file on the Kong node's filesystem, useful for local log aggregation.*

---

## Q19. What does the `http-log` plugin do?

**A)** Logs requests to a local file
**B)** Sends request/response logs as HTTP POST requests to a configured endpoint
**C)** Logs HTTP errors to syslog
**D)** Replays HTTP requests for debugging

**Answer: B**
*The `http-log` plugin sends log entries as JSON payloads via HTTP POST to an external log aggregator (Splunk, Elastic, custom webhook, etc.).*

---

## Q20. Which plugin is used to forward Kong logs to a syslog server?

**A)** `file-log`
**B)** `syslog`
**C)** `tcp-log`
**D)** `statsd`

**Answer: B**
*The `syslog` plugin sends log entries to the local syslog facility, which can forward to remote syslog servers (rsyslog, syslog-ng).*

---

## Q21. What is the primary purpose of the `statsd` plugin?

**A)** Logging full request/response payloads
**B)** Sending metrics in StatsD format to a StatsD server (Graphite, InfluxDB, etc.)
**C)** Rate limiting based on StatsD counters
**D)** Generating synthetic traffic for monitoring

**Answer: B**
*The `statsd` plugin emits request metrics (latency, request count, status codes) to a StatsD-compatible metrics server for aggregation and graphing.*

---

## Q22. Which plugin would you use to block SQL injection and other OWASP Top 10 attacks at the gateway level?

**A)** `ip-restriction`
**B)** `modsecurity` / `openappsec` (WAF plugin)
**C)** `acl`
**D)** `request-size-limiting`

**Answer: B**
*WAF (Web Application Firewall) plugins like `openappsec` or ModSecurity integration inspect request content for injection patterns and other OWASP-class attacks.*

---

## Q23. The `correlation-id` plugin adds which header to requests?

**A)** `X-Kong-Consumer-ID`
**B)** A unique identifier header (e.g., `Kong-Request-ID`) for distributed tracing correlation
**C)** `X-Forwarded-For`
**D)** `Authorization`

**Answer: B**
*The `correlation-id` plugin injects a UUID (or other format) into a configurable request header, enabling end-to-end request tracing across services and log aggregators.*

---

## Q24. Which plugin validates incoming requests against an OpenAPI schema?

**A)** `jwt`
**B)** `request-validator`
**C)** `oas-validation`
**D)** `openid-connect`

**Answer: C**
*The `oas-validation` plugin (Enterprise) validates requests and responses against a linked OpenAPI spec, rejecting malformed requests before they reach the upstream.*

---

## Q25. What does the `response-ratelimiting` plugin rate-limit on, compared to `rate-limiting`?

**A)** It rate-limits based on response body size
**B)** It rate-limits based on a custom counter header in the upstream response (e.g., credits consumed)
**C)** It rate-limits response body streaming
**D)** It is identical to `rate-limiting`

**Answer: B**
*`response-ratelimiting` reads a header (configurable, e.g., `X-Kong-Limit`) set by the upstream in its response and decrements a quota accordingly — useful for metered APIs.*

---

## Q26. Which plugin adds mutual TLS (mTLS) client certificate authentication to a service?

**A)** `jwt`
**B)** `mtls-auth`
**C)** `tls-handshake-modifier`
**D)** `ssl`

**Answer: B**
*The `mtls-auth` plugin requires clients to present a valid TLS client certificate signed by a trusted CA, authenticating them at the TLS layer.*

---

## Q27. The `kafka-log` plugin sends request logs to:

**A)** A Kafka consumer group
**B)** A Kafka topic via a Kafka broker
**C)** A Kafka REST Proxy
**D)** A Kafka Streams application

**Answer: B**
*The `kafka-log` plugin publishes log entries as messages to a Kafka topic, enabling high-throughput event-driven log processing pipelines.*

---

## Q28. When should you use the `proxy-cache-advanced` plugin over `proxy-cache`?

**A)** When caching gRPC responses
**B)** When you need Redis Cluster support, cache warming, or fine-grained TTL per status code
**C)** When caching is only needed for GET requests
**D)** When using Kong OSS

**Answer: B**
*`proxy-cache-advanced` (Enterprise) extends the OSS plugin with Redis Cluster/Sentinel support, configurable TTLs per status code, and Vary header support.*

---

## Q29. Which plugin enforces that requests contain specific headers or query parameters before forwarding?

**A)** `request-termination`
**B)** `request-validator`
**C)** `acl`
**D)** `cors`

**Answer: B**
*The `request-validator` plugin validates request headers, query parameters, and body against a schema (JSON Schema or OAS), rejecting invalid requests with `400 Bad Request`.*

---

## Q30. For which use case would you combine `key-auth` AND `acl` plugins on the same route?

**A)** To require both an API key and a client certificate
**B)** To authenticate the client with an API key AND restrict access to only consumers in specific ACL groups
**C)** To rate-limit authenticated consumers only
**D)** To log requests only from authenticated consumers

**Answer: B**
*`key-auth` identifies and authenticates the Consumer. `acl` then checks that the Consumer belongs to an allowed group, providing both authentication and authorization.*
