# Section 12: Troubleshooting & Observability

> **Exam Theme:** Diagnosing and resolving common Konnect and Kong Gateway problems — CP/DP connectivity, plugin errors, traffic anomalies, health checks, and using observability tools to find root causes.

---

## Q1. A Data Plane node is not appearing in Konnect's Gateway Manager under "Data Plane Nodes." What should you check first?

**A)** Whether the Dev Portal is published
**B)** Whether `cluster_control_plane`, `cluster_cert`, `cluster_cert_key`, and `cluster_server_name` are correctly configured on the DP node, and whether the DP can reach the CP endpoint on port 443
**C)** Whether the Analytics ingestion is enabled for the Control Plane
**D)** Whether the Konnect Terraform provider is up to date

**Answer: B**
*A DP not appearing in Konnect means the CP/DP mTLS channel never established. Check: correct CP endpoint address, valid cluster cert/key pair from this CP, port 443 reachable from DP network, and matching `cluster_server_name`.*

---

## Q2. A Data Plane node shows "Disconnected" in Konnect Gateway Manager. Traffic is still being served. What explains this?

**A)** The DP is in maintenance mode and serving cached responses
**B)** The DP cached its last known configuration and continues proxying traffic — the disconnect only prevents new config changes from propagating
**C)** The DP is running a different version of Kong and has failed over
**D)** The DP is disconnected and serving 503 errors to all clients

**Answer: B**
*CP/DP separation resilience: DP continues operating from cached config during a disconnect. "Disconnected" means management-plane loss only — data-plane traffic is unaffected.*

---

## Q3. After connecting a new Data Plane node, it shows as "Connected" in Konnect but requests are returning 404. What is the most likely cause?

**A)** The DP is using a wrong cluster certificate
**B)** The Control Plane has no routes or services configured yet — the DP is connected but the gateway configuration is empty
**C)** The DP version is incompatible with the CP
**D)** Analytics ingestion is disabled

**Answer: B**
*"Connected but 404" means the DP is working correctly — it's just serving an empty (or partial) configuration. Add services and routes to the CP; they will propagate to the DP.*

---

## Q4. Which Konnect Admin API endpoint confirms that a DP node is connected to a Control Plane?

**A)** `GET /status`
**B)** `GET /clustering/data-planes`
**C)** `GET /nodes`
**D)** `GET /health`

**Answer: B**
*`GET /clustering/data-planes` returns all DP nodes registered to a CP, their connection status, Kong version, and last configuration sync timestamp.*

---

## Q5. A client receives `HTTP 401 Unauthorized` consistently. The developer is certain they are sending the correct API key. What are the top three things to verify?

**A)** The upstream service logs, the DP version, and the CP geo
**B)** (1) The header/query param name the plugin expects matches what the client sends, (2) the Consumer credential is enabled and not expired, (3) the key-auth plugin is actually applied to the matched route/service
**C)** The rate limiting counter, the Data Plane memory usage, and the Analytics retention period
**D)** The CMEK configuration, the SSO provider, and the cluster certificate

**Answer: B**
*401 checklist: Is the key being sent in the right header/param? Is the Consumer and its credential active? Is the auth plugin present on the correct route or service? Any one of these being wrong causes 401.*

---

## Q6. A client receives `HTTP 403 Forbidden` after successfully authenticating. What is the most likely cause?

**A)** The API key has expired
**B)** The Consumer is authenticated but is not a member of the ACL group allowed by the `acl` plugin on that route
**C)** The rate limit has been exceeded
**D)** The upstream service is returning 403 to Kong

**Answer: B**
*403 after successful auth = authorization failure. The `acl` plugin checks Consumer group membership — authenticated but not authorized. Verify the Consumer's `acl` group assignments.*

---

## Q7. A route is returning `HTTP 404` for requests that should match it. After checking the route exists, what are the next diagnostic steps?

**A)** Restart the Control Plane and retry
**B)** Check that (1) the route's `hosts`, `paths`, and `methods` fields precisely match the incoming request, (2) `strip_path` and `preserve_host` settings are correct, and (3) the CP has propagated the config to the DP
**C)** Check the upstream health check status
**D)** Verify the Analytics ingestion is enabled

**Answer: B**
*404 from Kong = no route matched. Inspect: exact path/host/method match, regex correctness, protocols. Also check that the config change has propagated — a new route may not be on the DP yet if the CP/DP sync is delayed.*

---

## Q8. An API is returning `HTTP 502 Bad Gateway`. Kong logs show "upstream connect error." What does this indicate?

**A)** Kong's Admin API is misconfigured
**B)** Kong established routing correctly but failed to connect to the upstream service — check that the upstream host is reachable, the correct port is configured, and the service is running
**C)** The client sent a malformed request
**D)** The JWT token has expired

**Answer: B**
*502 = Kong reached the upstream but got an invalid response (or no connection). Common causes: wrong upstream host/port in the Service config, upstream service down, TLS mismatch between Kong and upstream.*

---

## Q9. You see a large spike in `HTTP 503` errors in Konnect Analytics. What should you investigate?

**A)** Client-side authentication configuration
**B)** Upstream health — check whether all targets in the upstream pool are marked unhealthy, and review active/passive health check results
**C)** The Control Plane geo setting
**D)** The decK state file for syntax errors

**Answer: B**
*503 = no healthy upstream targets available. Check the Upstream's target health via `GET /upstreams/{name}/health`. Investigate whether the backend services are down, overloaded, or misconfigured.*

---

## Q10. Which response headers can you inspect on a Kong proxied response to understand where latency is being introduced?

**A)** `X-Response-Time` and `Server-Timing`
**B)** `X-Kong-Proxy-Latency` (Kong processing time) and `X-Kong-Upstream-Latency` (time waiting for upstream response)
**C)** `X-Kong-Gateway-Version` and `X-Kong-Plugin-Chain`
**D)** `X-Forwarded-For` and `X-Real-IP`

**Answer: B**
*`X-Kong-Proxy-Latency` = Kong overhead. `X-Kong-Upstream-Latency` = how long the upstream took. High upstream latency → backend problem. High proxy latency → Kong plugin or config problem.*

---

## Q11. A plugin is throwing a Lua error in production. Where do you find the full stack trace?

**A)** In the Konnect Analytics dashboard
**B)** In Kong's `error.log` file — Lua runtime errors with stack traces are written there at `error` log level
**C)** In the Dev Portal application logs
**D)** In the decK state file diff output

**Answer: B**
*`error.log` is the primary diagnostic log for plugin execution errors. Set `log_level = debug` for more context, but `error` level is where Lua exceptions and plugin failures always appear.*

---

## Q12. After a `deck sync`, several existing routes disappeared from the gateway. What happened?

**A)** A Konnect outage deleted the routes
**B)** The synced state file did not include those routes — decK deleted them to reconcile Kong's running state with the desired state in the file
**C)** The routes were moved to a different workspace automatically
**D)** The CP propagation was interrupted mid-sync

**Answer: B**
*`deck sync` enforces desired state. Routes absent from the state file are "extra" entities — decK removes them. Always run `deck diff` first to review what will be deleted before running `deck sync`.*

---

## Q13. How do you prevent `deck sync` from deleting entities not present in the state file?

**A)** Use `deck validate` instead of `deck sync`
**B)** Add `--skip-delete` flag — decK will only create and update entities, never delete them
**C)** Tag all entities with `protected: true`
**D)** Run `deck diff` first — it prevents subsequent deletions

**Answer: B**
*`--skip-delete` is the safety net for additive-only syncs. It prevents any deletions — useful during migrations or when managing partial configuration slices.*

---

## Q14. A rate-limiting plugin is configured with `policy: redis` but requests are not being counted accurately across multiple DP nodes. What should you investigate?

**A)** Whether the route has `strip_path` enabled
**B)** Redis connectivity from all DP nodes — verify `redis_host`, `redis_port`, and `redis_password` in the plugin config, and test that every DP can reach the Redis instance
**C)** Whether the Analytics ingestion is capturing rate-limit events
**D)** Whether the Control Plane is synchronized

**Answer: B**
*With Redis policy, all DP nodes share rate-limit counters via Redis. If some DPs can't reach Redis, they fall back to local (node-level) counting — leading to inaccurate distributed rate limiting.*

---

## Q15. What does the `deck diff` output `~ updating service "payments-service"` followed by `+ creating plugin "rate-limiting"` indicate?

**A)** The state file has a syntax error
**B)** decK will modify the payments-service configuration AND add a new rate-limiting plugin that doesn't currently exist in Kong
**C)** decK detected a conflict between two state files
**D)** The sync will fail because updates and creates cannot be mixed

**Answer: B**
*`~` = update (entity exists, changing fields). `+` = create (entity does not exist yet). Reading the diff output helps reviewers understand the exact changes before approving the sync.*

---

## Q16. A consumer's application consistently hits rate limits even though the limit appears set high enough. What could cause this?

**A)** The upstream service is enforcing its own rate limits
**B)** The `limit_by` field is set to `ip` instead of `consumer` — if multiple consumers share an IP (e.g., behind a NAT), they share the same counter and collectively exhaust the limit
**C)** The Analytics ingestion is causing false positives
**D)** The Control Plane is applying rate limits before the DP

**Answer: B**
*`limit_by: ip` with shared NAT IPs is a classic misconfiguration. Multiple consumers behind a corporate NAT all count as the same IP — they collectively hit the limit. Switch to `limit_by: consumer` for per-consumer accuracy.*

---

## Q17. What does `deck ping` return when the Konnect token has expired or is invalid?

**A)** "Ping successful — connection established"
**B)** An authentication error (401 Unauthorized) — indicating the token is expired, revoked, or does not have sufficient permissions
**C)** A timeout error — the CP is unreachable
**D)** "Ping failed — state file is invalid"

**Answer: B**
*`deck ping` authenticates to the Konnect CP. A 401 response means the token is bad (expired, wrong, or revoked). Regenerate or rotate the system account token in Konnect.*

---

## Q18. How do you determine which Control Plane version and how many DP nodes are running in a Konnect deployment?

**A)** Check the decK state file header
**B)** Use the Konnect Gateway Manager UI — the Control Plane overview shows CP version, connected DP node count, and each node's Kong version and last sync time
**C)** Run `kong version` on each node individually
**D)** Check the Konnect Analytics summary dashboard

**Answer: B**
*Gateway Manager is the operational dashboard for CP/DP status. It shows connectivity, versions, and sync states — the first place to check for deployment health at a glance.*

---

## Q19. A DP node's `X-Kong-Proxy-Latency` values are consistently high (100ms+) even for simple requests with minimal plugins. What should you investigate?

**A)** The upstream service response time
**B)** Kong node resources (CPU, memory) and the number/complexity of plugins executing — high proxy latency without upstream delay suggests Kong itself is overloaded or a plugin is computationally expensive
**C)** The Analytics retention period
**D)** The cluster certificate expiry

**Answer: B**
*High proxy latency (not upstream) = Kong is the bottleneck. Check: CPU utilization on the DP node, RAM, number of plugins in the chain, and whether any plugin (e.g., OPA, custom Lua) has expensive logic.*

---

## Q20. After enabling the `opentelemetry` plugin, traces are not appearing in the configured collector. What are the most likely causes?

**A)** OpenTelemetry only works with Dedicated Cloud Gateways
**B)** Verify that (1) `endpoint` points to the correct OTel Collector address, (2) the Collector is running and reachable from the DP, (3) the correct protocol (grpc/http) is configured, and (4) the plugin is applied to the intended routes/services
**C)** The OTel plugin requires the Analytics ingestion to be enabled first
**D)** Traces require the `prometheus` plugin to be co-deployed

**Answer: B**
*OTel troubleshooting: endpoint reachability, protocol match (gRPC vs HTTP/Protobuf), plugin scope (is it actually applied?), and Collector health. Check Kong's `error.log` for OTel exporter errors.*

---

## Q21. A `deck sync` fails with the error: `"failed to authenticate: 401 Unauthorized"`. What is the most likely cause and fix?

**A)** The state file has invalid YAML syntax
**B)** The Konnect access token used by decK is expired, revoked, or missing — regenerate a system account token in Konnect and update the CI/CD secret
**C)** The Control Plane does not support the decK format version
**D)** A route name conflict exists in the state file

**Answer: B**
*Authentication failures in decK pipelines almost always mean the token is bad. Tokens expire or get rotated. The fix: generate a new Konnect system account token and update the pipeline secret.*

---

## Q22. What does a sudden drop in Analytics traffic volume to near-zero (while the DP is healthy) most likely indicate?

**A)** An upstream outage — the backend stopped responding
**B)** Analytics ingestion was disabled for the Control Plane, OR a networking change blocked telemetry from the DP, OR the clients stopped sending traffic
**C)** The Analytics data was archived after the 7-day retention period
**D)** A rate limiting plugin is blocking all requests

**Answer: B**
*Near-zero traffic with healthy DP: check ingestion toggle (was it turned off?), check DP-to-analytics connectivity, and verify that actual client traffic is still flowing (use X-Kong headers or DP access logs as a ground truth).*

---

## Q23. How can you verify whether a Kong plugin is actually executing for a specific request without access to server logs?

**A)** Add `debug: true` to the plugin config
**B)** Enable the `correlation-id` plugin and check the correlation header in the response — if present, Kong processed the request through its plugin chain
**C)** Enable Kong's response debug headers (`Kong-Debug: 1` request header) to receive `X-Kong-Matched-Route` and active plugin names in the response
**D)** Use `deck diff` to check plugin attachment

**Answer: C**
*Sending `Kong-Debug: 1` (when enabled on the DP) causes Kong to return debug headers including the matched route, service, and executing plugins — visible without server log access.*

---

## Q24. An upstream service was replaced with a new version at a different IP. How do you update Kong routing with zero downtime?

**A)** Delete and recreate the Service object with the new IP
**B)** Add the new IP as a Target in the Upstream (with weight), verify traffic shifts correctly, then set the old Target's weight to 0 to drain it — no Service or Route changes needed
**C)** Update the Service `host` field directly — this takes effect immediately without downtime
**D)** Restart all DP nodes after updating the Upstream

**Answer: B**
*Zero-downtime target swap via Upstream weight management: add new target → verify → drain old target. The Service and Routes are untouched. Traffic gradually shifts without any gap in service.*

---

## Q25. A team reports that their `deck sync` on a staging CP is accidentally modifying production routes on a different CP. What configuration mistake caused this?

**A)** The state files use the same entity names
**B)** The `--konnect-control-plane-name` flag (or `KONNECT_CONTROL_PLANE_NAME` env var) is pointing to the production CP instead of the staging CP
**C)** `--select-tag` is not set
**D)** Both CPs share the same Konnect organization

**Answer: B**
*decK uses `--konnect-control-plane-name` to target the correct CP. If the staging pipeline accidentally targets the prod CP name, it syncs staging config to production. Always verify the CP name in pipeline secrets.*

---

## Q26. Which Konnect Analytics metric most directly signals that a backend service is becoming overloaded?

**A)** Increasing 4xx error rate
**B)** Increasing `X-Kong-Upstream-Latency` values combined with rising 5xx error rates from specific upstream targets
**C)** Decreasing `X-Kong-Proxy-Latency`
**D)** Dropping request volume

**Answer: B**
*Upstream overload signature: P99 upstream latency rises first (slower responses), followed by 5xx errors (timeouts or capacity exhaustion). The combination is the classic backend saturation pattern.*

---

## Q27. A Konnect user reports they cannot log into the management console after a recent SSO configuration change. What is the first troubleshooting step?

**A)** Delete and recreate the user account
**B)** Verify the OIDC/SAML IdP configuration in Konnect — check the issuer URL, client ID/secret, redirect URI, and that the IdP is returning the expected claims (e.g., email, group memberships)
**C)** Disable RBAC temporarily to allow login
**D)** Reset the Konnect organization to default settings

**Answer: B**
*SSO login failures after config changes almost always trace to IdP configuration: wrong redirect URI, mismatched client credentials, or the IdP not returning the claim that Konnect maps to user identity.*

---

## Q28. What is the purpose of the `kong health` CLI command on a self-managed DP node?

**A)** It tests connectivity to all upstream targets
**B)** It checks whether the local Kong process is running and whether it can connect to its configured database (or CP in hybrid mode) — a quick node-level sanity check
**C)** It returns the Konnect Analytics health status
**D)** It validates the decK state file integrity

**Answer: B**
*`kong health` is the first command to run when something seems wrong on a self-managed node — it quickly confirms whether the process is up and the most critical dependencies are reachable.*

---

## Q29. A team is seeing intermittent 504 Gateway Timeout errors on an API. `X-Kong-Proxy-Latency` is normal but `X-Kong-Upstream-Latency` is very high. What does this point to?

**A)** A Kong plugin is causing excessive processing time
**B)** The upstream service is responding too slowly — requests that exceed Kong's `read_timeout` (default 60 seconds) are terminated with a 504
**C)** The client is sending oversized request bodies
**D)** The Kong DP is running out of worker processes

**Answer: B**
*High `X-Kong-Upstream-Latency` + 504 = upstream timing out. The upstream takes longer than Kong's `read_timeout`. Fix: increase `read_timeout` on the Service if the upstream legitimately needs more time, or investigate and optimize the upstream.*

---

## Q30. When troubleshooting a Konnect issue, what is the recommended sequence of diagnostic steps?

**A)** Restart all DP nodes → check logs → contact Kong support
**B)** (1) Check Konnect Gateway Manager for CP/DP status and sync health, (2) inspect Analytics for error spikes and latency patterns, (3) review `error.log` on the DP node, (4) use `deck diff` to check for config drift, (5) validate connectivity with `deck ping` — escalate to Kong support with diagnostic data if unresolved
**C)** Run `deck sync` → check Analytics → restart DP nodes
**D)** Contact Kong support immediately for any production issue

**Answer: B**
*Structured troubleshooting: start with the highest-level view (Gateway Manager → Analytics), narrow to node logs, check config correctness (diff), validate tooling connectivity (ping). Systematic top-down isolation finds root causes faster than random actions.*
