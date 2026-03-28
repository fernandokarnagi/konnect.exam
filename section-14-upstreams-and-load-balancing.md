# Section 14: Upstreams and Load Balancing

> **Exam Theme:** Configuring Kong Upstreams, Targets, health checks, and load balancing algorithms.

---

## Q1. What is a Kong Upstream object?

**A)** A client that sends requests to Kong
**B)** A virtual hostname that groups multiple backend targets for load balancing
**C)** A plugin that inspects upstream responses
**D)** A DNS entry managed by Kong

**Answer: B**
*An Upstream is a named virtual host (e.g., `my-service-upstream`) that holds a pool of Targets. A Service's `host` field can point to an Upstream name to enable load balancing.*

---

## Q2. What is a Target in the context of Kong Upstreams?

**A)** A plugin configuration object
**B)** An individual backend address (host:port) within an Upstream pool
**C)** A health check endpoint
**D)** A consumer credential

**Answer: B**
*Each Target represents a single upstream instance (e.g., `10.0.0.1:8080`). The Upstream's load balancer distributes requests across its active Targets.*

---

## Q3. To use a Kong Upstream for load balancing, what must the Service's `host` field be set to?

**A)** The IP address of the first Target
**B)** The name of the Upstream object
**C)** A DNS CNAME for the upstream pool
**D)** `localhost`

**Answer: B**
*When the Service's `host` matches an Upstream's name, Kong routes traffic through the Upstream's load balancer instead of resolving the hostname via DNS.*

---

## Q4. Which load balancing algorithm distributes requests based on the number of active connections to each Target?

**A)** Round-Robin
**B)** Least-Connections
**C)** Hash (consistent hashing)
**D)** Random

**Answer: B**
*Least-Connections sends each new request to the Target with the fewest active connections, adapting to unequal processing times.*

---

## Q5. Which Kong load balancing algorithm is best for session stickiness (routing the same client to the same backend)?

**A)** Round-Robin
**B)** Least-Connections
**C)** Consistent Hashing
**D)** Random

**Answer: C**
*Consistent Hashing computes a hash of a request attribute (IP, header, cookie, or URI) and maps it to a Target. The same hash always maps to the same Target, enabling stickiness.*

---

## Q6. How is Target weight used in Kong load balancing?

**A)** It controls the Target's CPU allocation
**B)** It sets the proportion of requests sent to that Target relative to others
**C)** It determines the Target's priority for health checks
**D)** It sets the maximum connections to that Target

**Answer: B**
*A Target with `weight: 200` receives twice as many requests as a Target with `weight: 100`. Weight=0 effectively removes a Target from rotation.*

---

## Q7. What is the difference between active and passive health checking in Kong?

**A)** Active checks run on response; passive checks probe the Target proactively
**B)** Active checks send probe requests to Targets; passive checks monitor live traffic for failures
**C)** Active and passive are the same; the terms are interchangeable
**D)** Active checks apply to HTTP; passive checks apply to TCP

**Answer: B**
*Active health checks send periodic HTTP requests to a health endpoint. Passive (circuit-breaker) health checks infer health from the success/failure of proxied requests.*

---

## Q8. Which field on an Upstream defines the HTTP path used for active health checks?

**A)** `healthchecks.active.http_path`
**B)** `healthchecks.passive.health_endpoint`
**C)** `targets.health_path`
**D)** `health_check_url`

**Answer: A**
*`healthchecks.active.http_path` (default `/`) is the path Kong GETs on each Target to assess health during active health checking.*

---

## Q9. A Target has had 5 consecutive TCP failures. What does Kong do with this Target by default (with passive health checks enabled)?

**A)** It logs the failures but continues routing to the Target
**B)** It marks the Target as unhealthy and stops routing to it
**C)** It doubles the Target's weight
**D)** It removes the Target permanently

**Answer: B**
*When a Target exceeds the configured failure threshold (e.g., `unhealthy.tcp_failures: 5`), Kong marks it unhealthy. The Balancer stops sending traffic to it until it recovers.*

---

## Q10. How does Kong handle a request when ALL Targets in an Upstream are marked unhealthy?

**A)** It retries indefinitely
**B)** It returns 503 Service Unavailable
**C)** It bypasses health checks and picks any Target
**D)** It falls back to a default Service

**Answer: B**
*When no healthy Targets are available, Kong returns `503 Service Unavailable` to the client.*

---

## Q11. What is the default load balancing algorithm in Kong?

**A)** Least-Connections
**B)** Random
**C)** Round-Robin
**D)** Consistent Hashing

**Answer: C**
*Kong defaults to Round-Robin, which distributes requests evenly across Targets in sequence.*

---

## Q12. You add a Target with `weight: 0` to an Upstream. What is the effect?

**A)** The Target receives all traffic (highest weight)
**B)** The Target is excluded from the load balancer rotation
**C)** The Target becomes the health check endpoint
**D)** The Target is promoted to primary

**Answer: B**
*Weight 0 is a graceful way to drain a Target. It remains in the configuration but receives no traffic, equivalent to disabling it without deleting.*

---

## Q13. How many Targets can be added to a single Upstream?

**A)** Maximum 10
**B)** Maximum 100
**C)** Unlimited
**D)** Limited by the Kong license tier

**Answer: C**
*There is no hard limit on the number of Targets per Upstream. The practical limit is system resources and DNS resolution capacity.*

---

## Q14. Which Admin API endpoint adds a Target to an Upstream?

**A)** `POST /upstreams/{upstream-name}/targets`
**B)** `POST /targets/{upstream-id}`
**C)** `PUT /upstreams/{upstream-name}`
**D)** `PATCH /services/{service-id}/targets`

**Answer: A**
*Targets are children of Upstreams. `POST /upstreams/{name-or-id}/targets` with `{"target":"host:port","weight":100}` adds a Target.*

---

## Q15. What does the `slots` field on an Upstream control?

**A)** The number of concurrent connections
**B)** The number of slots in the load balancer ring, affecting weight granularity
**C)** The maximum number of Targets
**D)** The number of health check workers

**Answer: B**
*`slots` (default 10000) sets the size of the consistent hash ring or round-robin wheel. More slots = finer weight granularity and smoother distribution.*

---

## Q16. Which Upstream setting controls how many successful responses are needed before a Target is marked healthy again?

**A)** `healthchecks.active.healthy.successes`
**B)** `healthchecks.passive.healthy.successes`
**C)** `healthchecks.threshold.recover`
**D)** `targets.recovery_count`

**Answer: B**
*For passive health checks, `healthchecks.passive.healthy.successes` specifies how many consecutive successes are required to move a Target from unhealthy back to healthy.*

---

## Q17. A Kong Upstream uses consistent hashing on the `consumer` attribute. What does this ensure?

**A)** All requests from the same Consumer go to the same Target
**B)** All requests with the same URI go to the same Target
**C)** Requests are distributed based on consumer weight
**D)** Consumers are load balanced in round-robin order

**Answer: A**
*When hashing on `consumer`, Kong hashes the authenticated Consumer ID. All requests from that Consumer always route to the same Target — providing consumer-level stickiness.*

---

## Q18. What is the purpose of the `host_header` field on an Upstream?

**A)** It overrides the Host header sent to all Targets
**B)** It configures the health check Host header
**C)** It sets the header used for consistent hashing
**D)** It defines the upstream's DNS hostname

**Answer: A**
*`host_header` forces Kong to use a specific `Host` header value when proxying to Targets, useful when Targets require a specific virtual host name.*

---

## Q19. Which health check type is suitable when you want to detect failures without sending additional probe traffic?

**A)** Active health checks
**B)** Passive (circuit breaker) health checks
**C)** DNS health checks
**D)** TCP port health checks only

**Answer: B**
*Passive health checks observe the result of real proxied requests. They add no extra traffic but react to failures after they happen (circuit breaker pattern).*

---

## Q20. You have three Targets with weights 100, 100, and 200. What percentage of traffic does the third Target receive?

**A)** 33%
**B)** 40%
**C)** 50%
**D)** 25%

**Answer: C**
*Total weight = 400. Third Target weight = 200. 200/400 = 50%. The other two each receive 25%.*

---

## Q21. What does a Kong Upstream's `hash_on` field accept as values?

**A)** `ip`, `header`, `cookie`, `consumer`, `path`
**B)** `roundrobin`, `leastconn`, `random`
**C)** `http`, `tcp`, `grpc`
**D)** `active`, `passive`, `none`

**Answer: A**
*`hash_on` can be set to `none` (round-robin), `consumer`, `ip`, `header`, `cookie`, `uri`, or `path`. This selects what attribute is hashed for sticky routing.*

---

## Q22. Which Admin API call manually marks a Target as healthy?

**A)** `PATCH /upstreams/{upstream}/targets/{target}/healthy`
**B)** `PUT /targets/{id}/status`
**C)** `POST /upstreams/{upstream}/targets/{target}/health`
**D)** `DELETE /upstreams/{upstream}/targets/{target}/unhealthy`

**Answer: A**
*`PUT /upstreams/{upstream}/targets/{target}/healthy` (or the equivalent `PATCH`) manually overrides the health status to healthy.*

---

## Q23. Which load balancing algorithm is most appropriate for backends with variable response times?

**A)** Round-Robin
**B)** Random
**C)** Least-Connections
**D)** Consistent Hashing

**Answer: C**
*Least-Connections dynamically adapts by sending requests to the least-loaded backend, which is beneficial when response times vary significantly across Targets.*

---

## Q24. How can you perform a zero-downtime rollout of a new backend version using Kong Upstreams?

**A)** Delete all Targets and recreate them with the new version
**B)** Add the new Target with full weight, then reduce the old Target's weight to 0
**C)** Change the Service host to the new backend directly
**D)** Enable the `version_routing` flag on the Upstream

**Answer: B**
*Gradual traffic shifting: add the new Target at low weight, verify, increase its weight, then drain the old Target by reducing its weight to 0.*

---

## Q25. What happens to existing connections to a Target when it is marked unhealthy?

**A)** They are immediately terminated
**B)** They complete normally; only new connections avoid the unhealthy Target
**C)** They are transferred to a healthy Target mid-flight
**D)** They are queued until the Target recovers

**Answer: B**
*Health marking affects the load balancer's selection of new connections. In-flight requests to a Target that just became unhealthy are not interrupted.*

---

## Q26. Which Upstream field controls the interval (in seconds) for active health checks?

**A)** `healthchecks.active.interval`
**B)** `healthchecks.active.check_interval`
**C)** `healthchecks.interval`
**D)** `health_check_frequency`

**Answer: A**
*`healthchecks.active.healthy.interval` and `healthchecks.active.unhealthy.interval` control how often Kong probes healthy vs. unhealthy Targets.*

---

## Q27. A Service has `host: my-upstream` and the Upstream `my-upstream` has two Targets. You delete one Target. What happens to the Service configuration?

**A)** The Service breaks until a new Target is added
**B)** The Service automatically routes all traffic to the remaining Target
**C)** The Service caches the deleted Target and retries
**D)** The Service reverts to DNS resolution

**Answer: B**
*Removing a Target from an Upstream is seamless. Kong's balancer dynamically adjusts and all traffic shifts to the remaining Targets without changing the Service.*

---

## Q28. What is the role of `healthchecks.active.https_verify_certificate`?

**A)** It specifies the client certificate for mTLS health checks
**B)** It controls whether Kong validates the Target's TLS certificate during HTTPS health checks
**C)** It enables certificate pinning for upstream connections
**D)** It rotates the health check certificates automatically

**Answer: B**
*When set to `false`, Kong skips TLS certificate validation for health check probes (useful for self-signed certs). Default is `true`.*

---

## Q29. A Target is added with address `backend.internal:8080`. Kong cannot resolve `backend.internal`. What occurs?

**A)** The Target is marked healthy and used immediately
**B)** The Target is marked unhealthy due to DNS resolution failure
**C)** Kong logs a warning but continues using the Target
**D)** The Upstream is deleted

**Answer: B**
*If DNS resolution fails for a Target, Kong treats it as unreachable and marks it unhealthy (or skips it during balancing), preventing traffic from being sent to an unresolvable host.*

---

## Q30. How does Kong handle an Upstream with `algorithm: least-connections` when all Targets have the same number of active connections?

**A)** It picks the Target with the lowest IP address
**B)** It falls back to random selection among tied Targets
**C)** It returns an error
**D)** It adds a new Target automatically

**Answer: B**
*When multiple Targets are tied in active connection count, Kong falls back to a round-robin or random tiebreaker to distribute evenly.*
