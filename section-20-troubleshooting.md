# Section 20: Troubleshoot Common Problems on Kong Gateway

> **Exam Theme:** Diagnosing and resolving common Kong issues — routing failures, plugin errors, connectivity problems, performance degradation, and configuration mistakes.

---

## Q1. A client receives `HTTP 404 Not Found` from Kong for a request that should match a route. What is the most likely cause?

**A)** The upstream service is down
**B)** No Route matches the incoming request's host, path, or method
**C)** The Consumer's credentials are expired
**D)** The rate-limiting plugin is returning 404

**Answer: B**
*Kong returns 404 when its router finds no matching Route. Check that the Route's `hosts`, `paths`, `methods`, and `protocols` match the incoming request exactly.*

---

## Q2. A client receives `HTTP 503 Service Unavailable`. Where should you start troubleshooting?

**A)** Check the Admin API configuration
**B)** Check the upstream service health and Kong's health check / Upstream targets
**C)** Review the RBAC roles
**D)** Restart Kong Manager

**Answer: B**
*503 indicates Kong cannot reach a healthy upstream. Check whether the upstream service is running, whether health checks are failing, and whether all Targets are marked unhealthy.*

---

## Q3. Which Kong log level provides the most detailed output for debugging plugin behavior?

**A)** `warn`
**B)** `info`
**C)** `error`
**D)** `debug`

**Answer: D**
*Setting `log_level = debug` in `kong.conf` (or `KONG_LOG_LEVEL=debug`) produces verbose output including plugin execution, routing decisions, and connection details.*

---

## Q4. Where are Kong Gateway error and access logs located by default on a Linux installation?

**A)** `/var/log/kong/kong.log`
**B)** `/usr/local/kong/logs/error.log` and `access.log`
**C)** `/etc/kong/logs/`
**D)** `/tmp/kong/`

**Answer: B**
*By default, Kong writes `error.log` and `access.log` to `/usr/local/kong/logs/`. The path is configurable via `nginx_err_logs` and `proxy_access_log` in `kong.conf`.*

---

## Q5. A plugin that worked correctly is now throwing a Lua error. Which log level and log file would reveal the stack trace?

**A)** `info` level in `access.log`
**B)** `error` level in `error.log`
**C)** `warn` level in `admin_access.log`
**D)** `debug` level in `status.log`

**Answer: B**
*Lua errors and stack traces are written to `error.log` at `error` (or `warn`) log level. The `debug` level adds more context but `error.log` is where exceptions appear first.*

---

## Q6. A client gets `HTTP 401 Unauthorized` even with what appears to be a valid API key. What are the likely causes?

**A)** The upstream service is rejecting the key
**B)** The key is being sent in the wrong header/parameter, the consumer credential is disabled, or the key-auth plugin isn't applied to this route
**C)** The rate limit has been exceeded
**D)** The Service host cannot be resolved

**Answer: B**
*Check: Is the key sent in the header/param the plugin expects (`key-names` config)? Is the Consumer enabled? Is the credential correctly stored? Is the plugin applied to the correct route/service?*

---

## Q7. After enabling a new plugin, all requests return `HTTP 500 Internal Server Error`. What is the first thing to check?

**A)** The upstream service response time
**B)** The plugin's `config` for invalid or missing required parameters
**C)** The Consumer's ACL group membership
**D)** The database connection pool

**Answer: B**
*A misconfigured plugin (missing required fields, wrong data types, invalid regex) often causes Lua runtime errors that surface as 500s. Review the plugin config and `error.log`.*

---

## Q8. How can you verify whether a specific plugin is actually executing for a given request?

**A)** Check the upstream response headers
**B)** Enable `debug` logging and look for the plugin name in error.log, or add an `X-Kong-Matched-Route` debug header
**C)** Query the Admin API `/plugins` endpoint
**D)** Check the Upstream health status

**Answer: B**
*Debug-level logs show which plugins execute per request. Kong also provides `X-Kong-Matched-Route` and related headers in responses (when enabled) for diagnostic inspection.*

---

## Q9. A decK `sync` command fails with "conflict: entity already exists." What causes this?

**A)** Two entities in the state file have the same plugin type
**B)** A resource in the state file has a name or ID that conflicts with an existing Kong entity not managed by decK
**C)** The decK version is outdated
**D)** The state file has a syntax error

**Answer: B**
*If a resource exists in Kong but with a different configuration or created outside decK, a name/ID collision can occur. Use `deck diff` to inspect the conflict before syncing.*

---

## Q10. Requests are taking much longer than expected. `X-Kong-Proxy-Latency` is low but `X-Kong-Upstream-Latency` is high. Where is the bottleneck?

**A)** Kong's plugin chain
**B)** The upstream backend service
**C)** Kong's DNS resolution
**D)** The Admin API

**Answer: B**
*`X-Kong-Upstream-Latency` measures the time from when Kong sent the request upstream to when it received the response. High values indicate slow upstream services.*

---

## Q11. How do you check the health status of all Upstream Targets via the Admin API?

**A)** `GET /upstreams/{upstream}/health`
**B)** `GET /targets/health`
**C)** `GET /upstreams/{upstream}/targets`
**D)** `GET /health`

**Answer: A**
*`GET /upstreams/{upstream-name}/health` returns the health status of all Targets in the upstream, showing healthy, unhealthy, and DNS error states.*

---

## Q12. Kong is running but the Admin API is unreachable on port 8001. What should you check first?

**A)** The proxy port 8000 listener
**B)** The `admin_listen` configuration in `kong.conf` and firewall/security group rules
**C)** The database connection
**D)** Whether RBAC is enabled

**Answer: B**
*Verify `admin_listen` includes the expected IP and port (`0.0.0.0:8001` or `127.0.0.1:8001`). Also check OS-level firewalls, cloud security groups, and that the Kong process is running.*

---

## Q13. A Consumer is successfully authenticated but receives `HTTP 403 Forbidden`. What plugin is most likely causing this?

**A)** `rate-limiting`
**B)** `acl`
**C)** `jwt`
**D)** `cors`

**Answer: B**
*The `acl` plugin returns 403 when an authenticated Consumer is not a member of the configured allow-list group (or is in a deny-list group). Check the Consumer's group membership.*

---

## Q14. Kong returns `HTTP 502 Bad Gateway`. What does this typically indicate?

**A)** A client-side authentication error
**B)** Kong received an invalid response from the upstream (e.g., upstream closed the connection unexpectedly)
**C)** Kong's internal Lua runtime crashed
**D)** A DNS resolution failure

**Answer: B**
*502 means Kong successfully connected to the upstream but received an invalid or incomplete HTTP response. Check the upstream application logs for errors.*

---

## Q15. After a `deck sync`, routes are disappearing that were manually created via Kong Manager. Why?

**A)** decK does not support routes
**B)** decK treats the state file as the source of truth and deletes resources not in the file
**C)** Kong Manager overwrote the decK state
**D)** The `--skip-delete` flag was used

**Answer: B**
*By default `deck sync` removes entities not present in the state file. To preserve manually created entities, either add them to the state file or use `--skip-delete`.*

---

## Q16. What does the `kong health` command output?

**A)** The health of all upstream targets
**B)** The status of Kong's nginx process and database connectivity
**C)** A list of failed plugins
**D)** The CPU and memory usage of Kong nodes

**Answer: B**
*`kong health` checks whether the Kong process is running and whether the database is reachable, providing a quick sanity check of the node's operational status.*

---

## Q17. A plugin configured at the Service level is not executing. The same plugin type is also configured at the Route level. What is the likely cause?

**A)** Route-level plugins shadow Service-level plugins — only the Route-level runs
**B)** Service-level plugins only execute for TCP traffic
**C)** The plugin requires a Consumer to be set
**D)** The plugin is disabled globally

**Answer: A**
*More specific plugins (Route > Service > Global) shadow less specific ones of the same type. If a Route-level plugin of the same type is active, the Service-level one may not execute.*

---

## Q18. You observe that `kong reload` does not pick up configuration changes made in `kong.conf`. What should you check?

**A)** Whether the decK state file was updated
**B)** Whether Kong is reading the correct `kong.conf` path (check `--conf` flag or `KONG_CONF` env var)
**C)** Whether RBAC is blocking the reload
**D)** Whether the upstream is healthy

**Answer: B**
*Kong may be using a different `kong.conf` than the one you edited. Use `kong check` or verify the `--conf` startup flag / `KONG_CONF` environment variable.*

---

## Q19. Requests to a specific service are failing with `connection refused` errors in the logs. What is the most direct diagnostic step?

**A)** Restart Kong
**B)** Manually test connectivity from the Kong node to the upstream host:port (e.g., `curl`, `telnet`, `nc`)
**C)** Delete and recreate the Service
**D)** Disable all plugins on the service

**Answer: B**
*"Connection refused" means the upstream is not accepting connections on the expected port. Test directly from the Kong node to confirm network reachability and that the service is listening.*

---

## Q20. A `rate-limiting` plugin is configured with Redis as the storage strategy but requests are not being counted. What should you check?

**A)** The Consumer's group membership
**B)** The Redis host/port/password settings in the plugin config and Redis connectivity from Kong
**C)** Whether the `proxy-cache` plugin is interfering
**D)** Whether the route uses HTTPS

**Answer: B**
*If Redis is unreachable or misconfigured, the rate-limiting plugin may fall back to local counters (per-node) or fail silently. Verify Redis connectivity and plugin config.*

---

## Q21. Which Admin API endpoint helps diagnose why a specific request matched (or didn't match) a route?

**A)** `GET /routes/debug`
**B)** `GET /router/inspect` or the router expression evaluation endpoint
**C)** `GET /services/{id}/routes`
**D)** `POST /routes/test`

**Answer: B**
*Kong provides router inspection capabilities (`/router/inspect` in newer versions or the router debug functionality) to evaluate which route would match a given request without sending real traffic.*

---

## Q22. A plugin returns `attempt to index field '?' (a nil value)` in the error log. What does this indicate?

**A)** A rate limit has been exceeded
**B)** A Lua nil dereference — likely a missing required config value or unexpected nil in plugin code
**C)** The upstream returned nil
**D)** The Consumer has no credentials

**Answer: B**
*This Lua error means the plugin attempted to access a field on a nil value — often caused by a missing configuration parameter or a code bug accessing an unset variable.*

---

## Q23. After upgrading Kong, existing decK state files fail to sync with schema validation errors. What is the most likely cause?

**A)** The PostgreSQL version changed
**B)** The new Kong version introduced breaking schema changes requiring state file updates
**C)** The decK version is too new
**D)** The workspace was renamed

**Answer: B**
*Upgrading Kong may change config schema (renamed/removed fields). Update the decK state file to reflect new schema fields and update `_format_version` if required.*

---

## Q24. How do you confirm that Kong is successfully connected to its Control Plane in a Hybrid mode deployment?

**A)** Run `kong start` on the DP and check for errors
**B)** Check the DP node's status in the Control Plane's Kong Manager or via `GET /clustering/data-planes`
**C)** Query `GET /status` on the DP node
**D)** Use `deck dump` from the DP

**Answer: B**
*The Control Plane Admin API endpoint `GET /clustering/data-planes` lists connected DP nodes, their versions, and last sync time. Kong Manager's "Data Plane Nodes" view shows the same.*

---

## Q25. A request is authenticated but a per-consumer rate limit is not being applied. What should you verify?

**A)** The Consumer Group configuration
**B)** Whether the rate-limiting plugin's `limit_by` is set to `consumer` and the consumer is correctly identified
**C)** Whether the upstream supports rate limiting
**D)** Whether the `acl` plugin is enabled

**Answer: B**
*If `limit_by` is set to `ip` instead of `consumer`, per-consumer limits won't apply. Also verify the authentication plugin is identifying the consumer before the rate-limiting plugin runs.*

---

## Q26. Kong's `error.log` contains `could not connect to database`. What are the first troubleshooting steps?

**A)** Restart Kong and check the proxy port
**B)** Verify `pg_host`, `pg_port`, `pg_database`, `pg_user`, `pg_password` in `kong.conf` and test connectivity from the Kong host to PostgreSQL
**C)** Re-run `kong migrations bootstrap`
**D)** Disable RBAC and retry

**Answer: B**
*Database connection errors require verifying the connection parameters in `kong.conf` and testing that the Kong host can reach PostgreSQL on the configured host/port with the right credentials.*

---

## Q27. After enabling the `proxy-cache` plugin, some clients receive stale data. What configuration should you review?

**A)** The rate-limiting TTL
**B)** The `cache_ttl` and `cache_control` settings, and whether cache invalidation is needed
**C)** The Consumer's credentials
**D)** The Upstream health check interval

**Answer: B**
*Stale cache responses result from a TTL that is too long or `cache_control` ignoring upstream `Cache-Control` headers. Reduce `cache_ttl` or enable `cache_control: true` to respect upstream directives.*

---

## Q28. Which command outputs the list of all installed and enabled plugins on a running Kong node?

**A)** `kong plugins list`
**B)** `GET /` on the Admin API (returns node info including loaded plugins)
**C)** `kong config show plugins`
**D)** `deck dump --plugins`

**Answer: B**
*`GET /` (root) on the Kong Admin API returns the node's version, configuration, and the list of loaded plugins in the response.*

---

## Q29. A client reports intermittent 502 errors that correlate with traffic spikes. What Kong settings should you investigate?

**A)** RBAC token TTL
**B)** Upstream `keepalive_pool_size`, `connect_timeout`, and whether the upstream is auto-scaling
**C)** The `cache_ttl` of the proxy-cache plugin
**D)** The Consumer's group membership

**Answer: B**
*Intermittent 502s under load often indicate upstream connection pool exhaustion or the upstream not keeping up. Review keepalive pool settings, timeouts, and upstream capacity.*

---

## Q30. Which tool provides a real-time view of Kong Gateway metrics (latency, error rate, request volume) to aid in live troubleshooting?

**A)** `deck diff`
**B)** Kong Analytics (in Konnect) or Prometheus/Grafana dashboards using the `prometheus` plugin
**C)** `kong check`
**D)** The `file-log` plugin output

**Answer: B**
*Real-time metrics for troubleshooting come from Kong Analytics (Konnect) or from Prometheus metrics exposed by the `prometheus` plugin, visualized in Grafana dashboards.*
