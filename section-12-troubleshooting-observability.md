# Section 12: Troubleshooting & Observability

> **Exam Theme:** Diagnosing and resolving common Konnect and Kong Gateway problems — CP/DP connectivity, plugin errors, traffic anomalies, health checks, and using observability tools to find root causes.

---

## Q1. A Data Plane node is not appearing in Konnect's Gateway Manager under "Data Plane Nodes." What should you check first?

**A)** Whether Analytics ingestion is enabled for the Control Plane
**B)** Whether the Konnect Terraform provider version is compatible with the current CP
**C)** Whether the Dev Portal is published and accessible from the DP network
**D)** Whether `cluster_control_plane`, `cluster_cert`, `cluster_cert_key`, and `cluster_server_name` are correctly configured on the DP, and whether port 443 to the CP endpoint is reachable

**Answer: D**
*A DP not appearing in Konnect means the CP/DP mTLS channel never established. Check: correct CP endpoint, valid cluster cert/key from this CP, port 443 reachable, matching `cluster_server_name`.*

---

## Q2. A Data Plane node shows "Disconnected" in Konnect but traffic is still being served. What explains this?

**A)** The DP is disconnected and returning 503 errors to all clients
**B)** The DP is in maintenance mode and serving cached read-only responses
**C)** The DP is running a newer Kong version than the CP and has failed over autonomously
**D)** The DP cached its last known configuration and continues proxying traffic — the disconnect only prevents new config changes from propagating until reconnection

**Answer: D**
*CP/DP separation resilience: DP continues operating from cached config during a disconnect. "Disconnected" means management-plane loss only — data-plane traffic is unaffected.*

---

## Q3. After connecting a new Data Plane node, it shows as "Connected" but requests are returning 404. What is the most likely cause?

**A)** The DP version is incompatible with the CP and cannot process routes
**B)** The DP is using the wrong cluster certificate, causing partial config sync
**C)** Analytics ingestion is disabled and hiding the route configuration
**D)** The Control Plane has no routes or services configured yet — the DP is connected but the gateway configuration is empty

**Answer: D**
*"Connected but 404" means the DP is working correctly — it's serving an empty (or partial) configuration. Add services and routes to the CP; they propagate to the DP.*

---

## Q4. Which Konnect endpoint confirms that a DP node is connected to a Control Plane?

**A)** `GET /health`
**B)** `GET /clustering/data-planes` — returns all DP nodes, their connection status, Kong version, and last sync timestamp
**C)** `GET /nodes`
**D)** `GET /status`

**Answer: B**
*`GET /clustering/data-planes` is the authoritative endpoint for DP connectivity status and last configuration sync time.*

---

## Q5. A client receives `HTTP 401 Unauthorized` and is certain the correct API key is being sent. What three things should you verify?

**A)** The CMEK configuration, the SSO provider settings, and the cluster certificate validity
**B)** The rate limiting counter, the Data Plane memory usage, and the Analytics retention period
**C)** The upstream service logs, the DP version compatibility, and the CP geo assignment
**D)** (1) The header/param name the plugin expects matches what the client sends, (2) the Consumer credential is enabled and not expired, (3) the key-auth plugin is applied to the matched route/service

**Answer: D**
*401 checklist: Is the key sent in the right header/param? Is the Consumer credential active? Is the auth plugin applied to the correct route or service? Any one being wrong causes 401.*

---

## Q6. A client receives `HTTP 403 Forbidden` after successfully authenticating with a valid API key. What is the most likely cause?

**A)** The rate limit for the authenticated Consumer has been exceeded
**B)** The upstream service is returning 403 back to Kong
**C)** The Consumer is authenticated but is not a member of the ACL group allowed by the `acl` plugin on that route
**D)** The API key has reached its expiry date and is being rejected silently

**Answer: C**
*403 after successful auth = authorization failure. The `acl` plugin checks Consumer group membership — authenticated but not authorized. Verify the Consumer's `acl` group assignments.*

---

## Q7. A route is returning 404 for requests that should match it. After confirming the route exists, what are the next diagnostic steps?

**A)** Verify the Analytics ingestion is enabled for the Control Plane
**B)** Check the upstream health check status for the associated service
**C)** Restart the Control Plane and wait for the DP to re-sync
**D)** Verify the route's `hosts`, `paths`, and `methods` precisely match the request; check `strip_path` and `preserve_host` settings; confirm the CP has propagated config to the DP

**Answer: D**
*404 from Kong = no route matched. Inspect: exact path/host/method match, regex correctness, protocols. A new route may not be on the DP yet if CP/DP sync is delayed.*

---

## Q8. An API returns `HTTP 502 Bad Gateway`. Kong logs show "upstream connect error." What does this indicate?

**A)** The JWT token in the request has expired and is being rejected
**B)** The client sent a malformed or oversized request body
**C)** Kong's Admin API is misconfigured for the affected service
**D)** Kong established routing correctly but failed to connect to the upstream — check that the upstream host is reachable, the port is correct, the service is running, and TLS settings match

**Answer: D**
*502 = Kong reached the upstream but got an invalid response or no connection. Common causes: wrong host/port, upstream down, TLS mismatch.*

---

## Q9. You see a spike in `HTTP 503` errors in Konnect Analytics. What should you investigate?

**A)** Client-side authentication configuration changes and expired credentials
**B)** Control Plane version upgrades that may have introduced breaking changes
**C)** The decK state file for syntax errors causing routing misconfigurations
**D)** Upstream health — check whether all targets in the upstream pool are marked unhealthy, and review active/passive health check results

**Answer: D**
*503 = no healthy upstream targets available. Check target health via `GET /upstreams/{name}/health`. Investigate whether backend services are down, overloaded, or misconfigured.*

---

## Q10. Which response headers should you inspect to understand where latency is introduced in a Kong-proxied request?

**A)** `X-Forwarded-For` and `X-Real-IP` — for client IP tracing
**B)** `X-Kong-Proxy-Latency` (Kong processing time) and `X-Kong-Upstream-Latency` (time waiting for upstream response)
**C)** `X-Response-Time` and `Server-Timing` — standard HTTP performance headers
**D)** `X-Kong-Gateway-Version` and `X-Kong-Plugin-Chain`

**Answer: B**
*`X-Kong-Proxy-Latency` = Kong overhead. `X-Kong-Upstream-Latency` = upstream response time. High upstream latency → backend problem. High proxy latency → Kong plugin or config issue.*

---

## Q11. A plugin is throwing a Lua error in production. Where do you find the full stack trace?

**A)** In the Konnect Analytics dashboard under the error monitoring tab
**B)** In the Dev Portal application logs for the affected API consumer
**C)** In the decK state file diff output from the last sync operation
**D)** In Kong's `error.log` file — Lua runtime errors with stack traces are written there at `error` log level

**Answer: D**
*`error.log` is the primary diagnostic log for plugin execution errors. Set `log_level = debug` for more context, but `error` level is where Lua exceptions always appear.*

---

## Q12. After a `deck sync`, several existing routes disappeared from the gateway. What happened?

**A)** A Konnect platform outage deleted the routes during the sync window
**B)** The routes were automatically moved to a different workspace during the sync
**C)** The CP propagation was interrupted mid-sync, causing partial configuration loss
**D)** The synced state file did not include those routes — decK deleted them to reconcile Kong's running state with the desired state in the file

**Answer: D**
*`deck sync` enforces desired state. Routes absent from the file are "extra" entities — decK removes them. Always run `deck diff` first to review what will be deleted.*

---

## Q13. How do you prevent `deck sync` from deleting entities not present in the state file?

**A)** Run `deck validate` immediately before `deck sync` — this prevents deletion of unvalidated entities
**B)** Tag all entities with `protected: true` to mark them as deletion-exempt
**C)** Add `--skip-delete` flag — decK will only create and update entities, never delete them
**D)** Run `deck diff` first — this blocks subsequent deletions in the same pipeline run

**Answer: C**
*`--skip-delete` is the safety net for additive-only syncs. It prevents any deletions — useful during migrations or when managing partial configuration slices.*

---

## Q14. A rate-limiting plugin configured with `policy: redis` is not counting requests accurately across multiple DP nodes. What should you investigate?

**A)** Whether the route has `strip_path` enabled which interferes with rate counting
**B)** Whether the Analytics ingestion is capturing all rate-limit events from both nodes
**C)** Whether the Control Plane is fully synchronized before traffic reaches the DP nodes
**D)** Redis connectivity from all DP nodes — verify `redis_host`, `redis_port`, and `redis_password` in the plugin config, and test that every DP can reach the Redis instance

**Answer: D**
*With Redis policy, all DP nodes share counters via Redis. If some DPs can't reach Redis, they fall back to local (node-level) counting — leading to inaccurate distributed rate limiting.*

---

## Q15. What does a `deck diff` output showing `~ updating service "payments-service"` and `+ creating plugin "rate-limiting"` indicate?

**A)** The sync will fail because updates and creates cannot be mixed in a single operation
**B)** decK detected a conflict between two overlapping state files for the payments service
**C)** decK will modify the payments-service configuration AND create a new rate-limiting plugin that doesn't currently exist in Kong
**D)** The state file has a syntax error preventing full parsing of the payments-service definition

**Answer: C**
*`~` = update (entity exists, changing fields). `+` = create (entity does not exist yet). Reading the diff output helps reviewers understand exact changes before approving the sync.*

---

## Q16. A consumer consistently hits rate limits even though the configured limit appears high enough. What could cause this?

**A)** The Analytics ingestion is causing false positives in the rate limit counter
**B)** The Control Plane is applying rate limits at the CP level before traffic reaches the DP
**C)** The upstream service is enforcing its own rate limits independently of Kong's plugin
**D)** The `limit_by` field is set to `ip` instead of `consumer` — if multiple consumers share an IP (e.g., corporate NAT), they collectively exhaust the limit

**Answer: D**
*`limit_by: ip` with shared NAT IPs is a classic misconfiguration. Multiple consumers behind a NAT all count as the same IP and collectively hit the limit. Switch to `limit_by: consumer`.*

---

## Q17. What does `deck ping` return when the Konnect token has expired?

**A)** A timeout error — indicating the CP is temporarily unreachable
**B)** "Ping successful" with a warning that the token will expire soon
**C)** An authentication error (401 Unauthorized) — indicating the token is expired, revoked, or missing required permissions
**D)** "Ping failed — state file is invalid or not found"

**Answer: C**
*`deck ping` authenticates to the Konnect CP. A 401 response means the token is bad (expired, wrong, or revoked). Regenerate a system account token in Konnect and update the CI/CD secret.*

---

## Q18. How do you determine which Control Plane version and how many DP nodes are running in a Konnect deployment?

**A)** Run `kong version` on each node individually via SSH
**B)** Check the decK state file header, which records the CP version at last sync time
**C)** Check the Konnect Analytics summary dashboard for infrastructure health metrics
**D)** Use the Konnect Gateway Manager UI — the Control Plane overview shows CP version, connected DP node count, each node's Kong version, and last sync time

**Answer: D**
*Gateway Manager is the operational dashboard for CP/DP status. It shows connectivity, versions, and sync states — the first place to check for deployment health at a glance.*

---

## Q19. `X-Kong-Proxy-Latency` is consistently high (100ms+) even for simple requests with minimal plugins. What should you investigate?

**A)** The upstream service response time and backend database query performance
**B)** The Analytics retention period settings affecting telemetry processing overhead
**C)** Kong node resources (CPU, memory) and the number/complexity of executing plugins — high proxy latency without upstream delay suggests Kong itself is overloaded or a plugin is computationally expensive
**D)** The cluster certificate expiry and TLS handshake overhead to the upstream

**Answer: C**
*High proxy latency (not upstream) = Kong is the bottleneck. Check: CPU utilization on the DP node, RAM, number of plugins in the chain, and whether any plugin has expensive Lua logic.*

---

## Q20. After enabling the `opentelemetry` plugin, traces are not appearing in the configured collector. What are the most likely causes?

**A)** OpenTelemetry only works with Dedicated Cloud Gateways, not hybrid deployments
**B)** Traces require the `prometheus` plugin to be co-deployed as a prerequisite
**C)** The OTel plugin requires the Analytics ingestion to be enabled on the Control Plane first
**D)** The `endpoint` may point to an unreachable OTel Collector address, the protocol (grpc/http) may be misconfigured, or the plugin may not be applied to the intended routes — check Kong's `error.log` for OTel exporter errors

**Answer: D**
*OTel troubleshooting: endpoint reachability, protocol match (gRPC vs HTTP/Protobuf), plugin scope (is it actually applied?), and Collector health. Check `error.log` for OTel exporter errors.*

---

## Q21. A `deck sync` fails with `"failed to authenticate: 401 Unauthorized"`. What is the most likely cause and fix?

**A)** A route name conflict exists in the state file causing a schema validation failure
**B)** The state file uses a `_format_version` incompatible with the current Kong version
**C)** The Control Plane does not support the decK CLI version being used
**D)** The Konnect access token used by decK is expired, revoked, or missing — regenerate a system account token in Konnect and update the CI/CD pipeline secret

**Answer: D**
*Authentication failures in decK pipelines almost always mean the token is bad. Tokens expire or get rotated. Fix: generate a new Konnect system account token and update the pipeline secret.*

---

## Q22. What does a sudden drop in Analytics traffic volume to near-zero (while the DP shows "Connected") most likely indicate?

**Select TWO plausible causes to investigate.**

**A)** The analytics retention period expired and data was automatically archived
**B)** Analytics ingestion was disabled for the Control Plane
**C)** A rate limiting plugin is blocking all traffic with 429 responses
**D)** A networking change blocked telemetry from the DP to the analytics ingestion endpoint
**E)** The cluster certificate expired, causing the DP to stop serving traffic

**Answer: B, D**
*Near-zero traffic with healthy DP: check ingestion toggle (was it turned off?) and DP-to-analytics connectivity. Verify actual client traffic is still flowing using DP access logs as ground truth.*

---

## Q23. How can you verify whether a Kong plugin is executing for a specific request without server log access?

**A)** Use `deck diff` to check plugin attachment in the current configuration
**B)** Add `debug: true` to the plugin configuration object
**C)** Enable Kong's response debug headers by sending `Kong-Debug: 1` in the request — this causes Kong to return `X-Kong-Matched-Route` and active plugin names in the response headers
**D)** Use `inso run test` to send the request and inspect which plugins are listed in the response

**Answer: C**
*Sending `Kong-Debug: 1` (when enabled on the DP) causes Kong to return debug headers including the matched route, service, and executing plugins — visible without server log access.*

---

## Q24. An upstream service was replaced with a new version at a different IP. How do you update Kong routing with zero downtime?

**A)** Update the Service `host` field directly and reload the DP — this takes effect without traffic interruption
**B)** Delete and recreate the Service object with the new IP during a maintenance window
**C)** Add the new IP as a Target in the Upstream at full weight, verify traffic shifts correctly, then set the old Target's weight to 0 to drain it — no Service or Route changes needed
**D)** Restart all DP nodes after updating the Upstream to force a configuration re-sync

**Answer: C**
*Zero-downtime target swap via Upstream weight management: add new target → verify → drain old target. Service and Routes are untouched. Traffic shifts gradually without any gap.*

---

## Q25. A team's `deck sync` on staging is accidentally modifying production routes on a different CP. What configuration mistake caused this?

**A)** Both CPs share the same Konnect organization, allowing cross-CP mutations
**B)** The `--select-tag` flag was not set, allowing the sync to touch all untagged entities
**C)** The state files for staging and production use entities with identical names
**D)** The `--konnect-control-plane-name` flag (or `KONNECT_CONTROL_PLANE_NAME` env var) is pointing to the production CP instead of the staging CP

**Answer: D**
*decK uses `--konnect-control-plane-name` to target the correct CP. If the staging pipeline accidentally targets the prod CP name, it syncs staging config to production.*

---

## Q26. Which Konnect Analytics metric most directly signals that a backend service is becoming overloaded?

**A)** Decreasing `X-Kong-Proxy-Latency` values indicating Kong is processing faster
**B)** Dropping request volume indicating clients are self-throttling
**C)** Increasing 4xx error rate indicating clients are sending malformed requests
**D)** Increasing `X-Kong-Upstream-Latency` values combined with rising 5xx error rates from specific targets

**Answer: D**
*Upstream overload signature: P99 upstream latency rises first (slower responses), followed by 5xx errors (timeouts or capacity exhaustion). This is the classic backend saturation pattern.*

---

## Q27. A Konnect user cannot log into the management console after a recent SSO configuration change. What is the first troubleshooting step?

**A)** Reset the Konnect organization to default settings and reconfigure SSO
**B)** Disable RBAC temporarily to allow the user to log in and investigate
**C)** Delete and recreate the user account to force re-provisioning from the IdP
**D)** Verify the OIDC/SAML IdP configuration in Konnect — check the issuer URL, client ID/secret, redirect URI, and that the IdP is returning the expected claims (e.g., email, group memberships)

**Answer: D**
*SSO login failures after config changes almost always trace to IdP configuration: wrong redirect URI, mismatched client credentials, or the IdP not returning the claim Konnect maps to user identity.*

---

## Q28. What is the purpose of the `kong health` CLI command on a self-managed DP node?

**A)** It validates the decK state file integrity against the current gateway configuration
**B)** It tests connectivity to all upstream targets configured in the gateway
**C)** It returns the Konnect Analytics health status for this node's telemetry stream
**D)** It checks whether the local Kong process is running and whether it can connect to its configured database or CP — a quick node-level sanity check

**Answer: D**
*`kong health` is the first command to run when something seems wrong on a self-managed node — quickly confirms whether the process is up and critical dependencies are reachable.*

---

## Q29. A team sees intermittent 504 Gateway Timeout errors that correlate with traffic spikes. `X-Kong-Proxy-Latency` is normal but `X-Kong-Upstream-Latency` is very high. What does this indicate?

**A)** A Kong plugin in the chain is causing excessive processing time under load
**B)** The Kong DP is running out of nginx worker processes during the spikes
**C)** The upstream service is responding too slowly — requests exceeding Kong's `read_timeout` (default 60s) are terminated with a 504
**D)** The client is sending oversized request bodies that overwhelm the upstream parser

**Answer: C**
*High `X-Kong-Upstream-Latency` + 504 = upstream timing out. The upstream takes longer than Kong's `read_timeout`. Fix: increase `read_timeout` on the Service if justified, or optimize the upstream.*

---

## Q30. When troubleshooting a Konnect issue, what is the recommended diagnostic sequence?

**Select TWO correct statements about the recommended troubleshooting approach.**

**A)** Restart all DP nodes immediately as the first step to clear any transient state
**B)** Start with the highest-level view: Konnect Gateway Manager (CP/DP status) and Analytics (error patterns), then narrow down to node-level logs and configuration checks
**C)** Contact Kong support immediately for any production issue to avoid making it worse
**D)** Use `deck diff` to check for configuration drift and `deck ping` to validate tooling connectivity before escalating to Kong support
**E)** Run `deck sync` as soon as possible to ensure the gateway is in the desired state before investigating

**Answer: B, D**
*Structured troubleshooting: start with the highest-level view (Gateway Manager → Analytics), narrow to node logs, check config correctness (diff), validate tooling (ping). Systematic top-down isolation finds root causes fastest.*
