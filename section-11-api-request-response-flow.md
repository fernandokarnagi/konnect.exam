# Section 11: API Request and Response Flow Through Kong Gateway

> **Exam Theme:** How a client request travels from ingress to upstream and back — phases, plugins, transformations, and error handling.

---

## Q1. In what order does Kong process phases for an incoming request?

**A)** Access → Rewrite → Header Filter → Body Filter → Log
**B)** Rewrite → Access → Header Filter → Body Filter → Log
**C)** Access → Header Filter → Body Filter → Rewrite → Log
**D)** Rewrite → Header Filter → Access → Log → Body Filter

**Answer: A**
*Kong's plugin execution order follows: rewrite (URL manipulation) → access (auth/rate-limit) → upstream proxying → header_filter → body_filter → log. Option A maps the post-proxy phases correctly.*

---

## Q2. A plugin needs to terminate a request before it reaches the upstream service. Which phase is the correct place to do this?

**A)** `header_filter`
**B)** `body_filter`
**C)** `access`
**D)** `log`

**Answer: C**
*The `access` phase runs before the request is forwarded upstream. Returning a response here (e.g., 401 Unauthorized) stops the request immediately.*

---

## Q3. Which component in Kong is responsible for actually forwarding the request to the upstream service?

**A)** The Router
**B)** The Balancer
**C)** The Proxy
**D)** The Admin API

**Answer: C**
*After routing and plugin processing, the Kong Proxy module opens a connection to the upstream and forwards the request.*

---

## Q4. What is the role of the Kong Router in the request flow?

**A)** It authenticates the incoming request
**B)** It maps the incoming request to a Service based on Routes
**C)** It applies rate-limiting rules
**D)** It stores request logs

**Answer: B**
*The Router evaluates configured Routes (host, path, method, headers) against the incoming request and selects the matching Service.*

---

## Q5. A request arrives at Kong with `Host: api.example.com` and path `/v1/users`. Two routes exist: one matching `/v1/users` on any host, and one matching `/v1/users` on `api.example.com`. Which route wins?

**A)** The first route created
**B)** The route with a higher priority number
**C)** The more specific route (host + path match)
**D)** Routes are selected randomly

**Answer: C**
*Kong's router uses a specificity-first algorithm. A route matching both host and path is more specific than one matching only path.*

---

## Q6. During which phase can a plugin modify response headers returned to the client?

**A)** `access`
**B)** `rewrite`
**C)** `header_filter`
**D)** `log`

**Answer: C**
*`header_filter` runs after the upstream response is received but before headers are sent to the client, allowing plugins to add, remove, or modify response headers.*

---

## Q7. A plugin reads the response body and conditionally transforms it. Which phase is used?

**A)** `access`
**B)** `header_filter`
**C)** `body_filter`
**D)** `rewrite`

**Answer: C**
*`body_filter` gives plugins access to the response body chunks as they stream from upstream to client.*

---

## Q8. Where in the request flow does Kong perform load balancing across upstream targets?

**A)** During the `rewrite` phase
**B)** After route matching, just before proxying
**C)** During the `header_filter` phase
**D)** During the `log` phase

**Answer: B**
*After the Router selects a Service and after plugin `access` phases complete, Kong's Balancer selects a target from the upstream and the Proxy establishes the connection.*

---

## Q9. Which HTTP response code does Kong return when no route matches an incoming request?

**A)** 400 Bad Request
**B)** 401 Unauthorized
**C)** 404 Not Found
**D)** 503 Service Unavailable

**Answer: C**
*When the Kong Router finds no matching route for an incoming request, it returns `404 Not Found` by default.*

---

## Q10. A plugin is applied at the Service level and another at the Route level for the same plugin type. How does Kong handle this?

**A)** Only the Route-level plugin executes
**B)** Only the Service-level plugin executes
**C)** Both execute; Route-level runs first
**D)** Both execute; Service-level runs first

**Answer: C**
*Kong applies plugins in order of scope specificity: Consumer → Route → Service → Global. Route-level plugins run before Service-level plugins of the same type.*

---

## Q11. What is a "plugin chain" in the context of Kong's request processing?

**A)** A sequence of upstream services that handle a request
**B)** The ordered list of active plugins that execute for a given request
**C)** A linked list of routes attached to a service
**D)** The chain of certificates in mTLS

**Answer: B**
*For each request, Kong assembles the applicable plugins from all scopes (global, service, route, consumer) into an ordered chain and executes each phase.*

---

## Q12. At which point in the request flow does Kong add the `X-Forwarded-For` header?

**A)** Before routing
**B)** After routing, before forwarding upstream
**C)** After upstream responds
**D)** During the log phase

**Answer: B**
*Kong injects proxy headers (`X-Forwarded-For`, `X-Real-IP`, `X-Forwarded-Proto`) into the upstream request after routing and before forwarding.*

---

## Q13. A backend service returns a 502 error. Which Kong component surfaces this to the client?

**A)** The Router
**B)** The Admin API
**C)** The Proxy (with an error response)
**D)** The Balancer retries indefinitely

**Answer: C**
*The Proxy module receives the upstream 502 and propagates it (or a configured error page) back to the client. The Balancer may retry if retries are configured.*

---

## Q14. What does the `strip_path` Route setting do in the request flow?

**A)** Removes query parameters before forwarding
**B)** Removes the matched route path prefix before forwarding to the upstream
**C)** Strips authentication headers from the request
**D)** Removes trailing slashes from the upstream URL

**Answer: B**
*When `strip_path=true`, Kong removes the matched path prefix (e.g., `/api/v1`) before forwarding, so the upstream sees only the remainder of the path.*

---

## Q15. Which phase executes AFTER the upstream response is fully received?

**A)** `access`
**B)** `rewrite`
**C)** `log`
**D)** `body_filter`

**Answer: C**
*The `log` phase runs after the response has been sent to the client, making it suitable for asynchronous logging, metrics emission, and auditing.*

---

## Q16. A consumer's credential is rejected by an auth plugin. What HTTP status does Kong return by default?

**A)** 403 Forbidden
**B)** 401 Unauthorized
**C)** 429 Too Many Requests
**D)** 503 Service Unavailable

**Answer: B**
*Authentication plugins (key-auth, basic-auth, JWT, etc.) return `401 Unauthorized` when credentials are missing or invalid.*

---

## Q17. How does Kong handle a request when the upstream target is health-checked as unhealthy?

**A)** It passes the request through anyway
**B)** It returns 503 and removes the target from rotation
**C)** It queues the request until the target recovers
**D)** It redirects to the Admin API

**Answer: B**
*Kong's active/passive health checking marks unhealthy targets and the Balancer skips them, returning 503 if no healthy targets remain.*

---

## Q18. Which request property is typically used by Kong to match a Route when using path-based routing?

**A)** The request body
**B)** The `Authorization` header
**C)** The URI path
**D)** The response Content-Type

**Answer: C**
*Routes can be matched on `hosts`, `paths`, `methods`, `headers`, and `snis`. Path-based routing uses the URI path of the incoming request.*

---

## Q19. What is the purpose of the `preserve_host` setting on a Route?

**A)** It caches the upstream hostname for DNS resolution
**B)** It forwards the original `Host` header to the upstream instead of the upstream hostname
**C)** It prevents host-based routing from overriding path routing
**D)** It enables SNI matching on TLS connections

**Answer: B**
*With `preserve_host=true`, Kong sends the client's original `Host` header to the upstream. With `preserve_host=false` (default), Kong uses the upstream's configured hostname.*

---

## Q20. In Kong's request lifecycle, when does consumer identification occur relative to plugin execution?

**A)** Before any plugins run
**B)** During the `access` phase, typically by an auth plugin
**C)** After the upstream responds
**D)** During the `log` phase

**Answer: B**
*Authentication plugins running in the `access` phase identify and attach a Consumer context to the request, which downstream plugins can then use for consumer-specific policies.*

---

## Q21. What happens to in-flight requests when the Kong Data Plane loses connection to the Control Plane?

**A)** All in-flight requests are dropped immediately
**B)** In-flight and new requests continue processing using the last known configuration
**C)** The Data Plane pauses until the Control Plane reconnects
**D)** The Data Plane forwards requests directly to the Admin API

**Answer: B**
*Data Plane nodes cache configuration locally. A CP/DP disconnect does not interrupt traffic — requests continue using the cached config until reconnection.*

---

## Q22. Which Kong object determines the final URL to which a proxied request is sent?

**A)** Route
**B)** Consumer
**C)** Service
**D)** Plugin

**Answer: C**
*The Service object holds the `host`, `port`, `path`, and `protocol` for the upstream. The Route matches the request; the Service defines the destination.*

---

## Q23. A plugin at the global scope and a plugin at the consumer scope are both of type `rate-limiting`. Which runs first?

**A)** Global scope
**B)** Consumer scope
**C)** They run simultaneously
**D)** The one created first

**Answer: B**
*Plugin execution priority by scope (most specific first): Consumer > Route > Service > Global. Consumer-scoped plugins run before global ones.*

---

## Q24. What does Kong use to determine the client's real IP address when behind a load balancer?

**A)** The `X-Kong-Client-ID` header
**B)** The `X-Forwarded-For` or `X-Real-IP` headers, configured via `trusted_ips`
**C)** The upstream response headers
**D)** The Admin API `/consumers` endpoint

**Answer: B**
*Kong uses `trusted_ips` to decide which proxy IPs to trust. When trusted, Kong reads `X-Forwarded-For` to determine the real client IP.*

---

## Q25. Which response phase allows a plugin to buffer and replace the entire upstream response body?

**A)** `access`
**B)** `header_filter`
**C)** `body_filter`
**D)** `rewrite`

**Answer: C**
*`body_filter` receives chunks of the response body. A plugin can accumulate all chunks and replace the body before it reaches the client.*

---

## Q26. In a typical Kong request flow, what is the sequence after the `access` phase completes successfully?

**A)** Log → Upstream request → Header filter → Body filter
**B)** Upstream request → Header filter → Body filter → Log
**C)** Header filter → Upstream request → Body filter → Log
**D)** Upstream request → Body filter → Header filter → Log

**Answer: B**
*After `access`, Kong proxies the request upstream, then processes the response through `header_filter`, `body_filter`, and finally `log`.*

---

## Q27. What is the significance of plugin priority numbers in Kong?

**A)** Higher priority numbers execute later in the chain
**B)** Higher priority numbers execute earlier in the chain
**C)** Priority numbers only apply to global plugins
**D)** Priority numbers determine which scope a plugin applies to

**Answer: B**
*Plugins are sorted in descending priority order. A plugin with priority 1000 runs before one with priority 500 within the same phase.*

---

## Q28. Which Kong feature allows modifying the upstream request URL path at runtime?

**A)** Consumer credentials
**B)** Route `paths` with regex capture groups and Service `path`
**C)** The Analytics module
**D)** The Dev Portal

**Answer: B**
*Using regex capture groups in Route paths combined with the Service `path` field (or the Request Transformer plugin), Kong can rewrite paths dynamically.*

---

## Q29. A client sends a request with an invalid JSON body to an API that requires JSON. Which Kong plugin can reject this request in the `access` phase?

**A)** Rate Limiting
**B)** Request Validator
**C)** Response Transformer
**D)** File Log

**Answer: B**
*The Request Validator plugin inspects request bodies against a schema and rejects malformed requests during the `access` phase before they reach the upstream.*

---

## Q30. In the response flow, which header does Kong add to indicate the latency added by the gateway itself?

**A)** `X-Kong-Upstream-Latency`
**B)** `X-Kong-Proxy-Latency`
**C)** `X-Response-Time`
**D)** `Server-Timing`

**Answer: B**
*`X-Kong-Proxy-Latency` reflects the time Kong spent processing the request (excluding upstream time). `X-Kong-Upstream-Latency` reflects the upstream response time.*
