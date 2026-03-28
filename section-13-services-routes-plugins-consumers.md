# Section 13: Services, Routes, Plugins, and Consumers

> **Exam Theme:** The four core Kong configuration objects — what they are, how they relate, and how to create and configure them.

---

## Q1. What does a Kong Service object represent?

**A)** A client that consumes an API
**B)** An upstream backend service that Kong proxies requests to
**C)** A set of routing rules for incoming requests
**D)** A configuration plugin attached to a route

**Answer: B**
*A Service object defines the upstream destination: protocol, host, port, and path. It is the target Kong sends requests to after matching a Route.*

---

## Q2. What does a Kong Route object represent?

**A)** A backend server address
**B)** Rules that match incoming client requests and link them to a Service
**C)** A set of authentication credentials
**D)** An analytics dashboard filter

**Answer: B**
*A Route defines matching criteria (hosts, paths, methods, headers, SNIs) and is always associated with a Service. Matched requests are forwarded to that Service.*

---

## Q3. Which Route property would you set to match requests on the path `/api/v2/orders`?

**A)** `hosts: ["api.v2.orders"]`
**B)** `paths: ["/api/v2/orders"]`
**C)** `methods: ["GET", "POST"]`
**D)** `protocols: ["http", "https"]`

**Answer: B**
*The `paths` field on a Route takes a list of URI prefixes or regex patterns. `/api/v2/orders` as a prefix matches all requests whose path starts with that string.*

---

## Q4. A Service has `path: /internal` and the Route has `strip_path: true` with `paths: ["/v1"]`. A request comes in to `/v1/users`. What path does the upstream receive?

**A)** `/v1/users`
**B)** `/internal/v1/users`
**C)** `/internal/users`
**D)** `/users`

**Answer: C**
*`strip_path=true` removes the matched prefix `/v1`, leaving `/users`. Kong then prepends the Service's `path` (`/internal`), resulting in `/internal/users`.*

---

## Q5. What is a Kong Consumer?

**A)** A backend service that provides an API
**B)** An entity representing a client or application that consumes an API
**C)** A plugin that tracks API usage
**D)** A route that consumes traffic from an upstream

**Answer: B**
*A Consumer represents an API client (user, application, partner). Consumers can have credentials, be assigned plugins, and be tracked in analytics.*

---

## Q6. Which credential type is used with the `key-auth` plugin?

**A)** Username and password
**B)** JWT token
**C)** API key
**D)** OAuth2 access token

**Answer: C**
*The `key-auth` plugin validates an API key sent in a header or query parameter. The key is a credential stored on a Consumer object.*

---

## Q7. You want to apply a rate-limiting plugin ONLY to a specific route. How do you scope it?

**A)** Set `consumer_id` on the plugin
**B)** POST to `/routes/{route-id}/plugins`
**C)** POST to `/plugins` with `config.global=false`
**D)** POST to `/services/{service-id}/plugins`

**Answer: B**
*Posting to `/routes/{route-id}/plugins` creates a plugin instance scoped exclusively to that Route.*

---

## Q8. What is the effect of setting `enabled: false` on a plugin instance?

**A)** The plugin is deleted
**B)** The plugin is paused and will not execute for matching requests
**C)** The plugin's scope changes to global
**D)** The plugin only runs during the `log` phase

**Answer: B**
*Setting `enabled: false` deactivates a plugin without deleting it. It remains in the configuration but is skipped during request processing.*

---

## Q9. A Consumer needs to authenticate with both a JWT and a rate limit of 100 req/min. Which two objects capture this?

**A)** Two Services
**B)** A Consumer with a JWT credential + a rate-limiting plugin scoped to that Consumer
**C)** Two Routes, one for JWT and one for rate limiting
**D)** A single plugin with both JWT and rate-limit config

**Answer: B**
*The Consumer holds the JWT credential (via `jwt` credentials endpoint). A `rate-limiting` plugin scoped to that Consumer enforces the per-consumer rate limit.*

---

## Q10. How many Routes can be associated with a single Service?

**A)** Exactly one
**B)** Up to five
**C)** Unlimited
**D)** Up to the number of available ports

**Answer: C**
*A Service can have many Routes. Multiple routes allow different path/host/method patterns to all proxy to the same upstream Service.*

---

## Q11. Which field on a Service defines the upstream hostname?

**A)** `name`
**B)** `url`
**C)** `host`
**D)** `path`

**Answer: C**
*The `host` field specifies the DNS hostname of the upstream. Alternatively, `url` is a convenience field that sets `host`, `port`, `protocol`, and `path` from a single string.*

---

## Q12. You create a Service with `url: "http://backend:8080/api"`. Which fields does Kong derive from this URL?

**A)** `host=backend`, `port=8080`, `protocol=http`, `path=/api`
**B)** `name=backend`, `port=8080`, `path=/api`
**C)** `host=backend:8080`, `path=/api`, `protocol=http`
**D)** `host=backend`, `port=80`, `protocol=http`, `path=/api`

**Answer: A**
*The `url` convenience field parses into `protocol`, `host`, `port`, and `path` separately. `http://backend:8080/api` → protocol=http, host=backend, port=8080, path=/api.*

---

## Q13. What is the purpose of a Consumer Group in Kong?

**A)** Grouping Consumers to apply shared plugin configurations
**B)** Routing groups of requests to different services
**C)** Merging multiple services into one upstream
**D)** Managing API key rotation schedules

**Answer: A**
*Consumer Groups allow a set of Consumers to share rate-limiting tiers or plugin configurations, enabling tiered API plans (basic, premium, enterprise).*

---

## Q14. Which Admin API endpoint retrieves all routes associated with a specific service?

**A)** `GET /routes?service_id={id}`
**B)** `GET /services/{id}/routes`
**C)** `GET /routes/{service-name}`
**D)** `GET /services/routes`

**Answer: B**
*`GET /services/{service-id}/routes` returns all Route objects linked to that Service.*

---

## Q15. To require authentication on all routes of a service, where should you attach the auth plugin?

**A)** As a global plugin
**B)** On each Route individually
**C)** On the Service object
**D)** On the Upstream object

**Answer: C**
*Attaching an auth plugin to the Service applies it to all requests to that Service, regardless of which Route matched. This avoids repeating the plugin on every Route.*

---

## Q16. A Route has `methods: ["GET"]`. A client sends a `POST` request to a matching path. What does Kong return?

**A)** 404 Not Found
**B)** 405 Method Not Allowed
**C)** 401 Unauthorized
**D)** 200 OK (methods are advisory)

**Answer: B**
*If a Route matches the path but not the method, Kong returns `405 Method Not Allowed`. If no route matches at all, it returns `404`.*

---

## Q17. Which Consumer credential is used with the `basic-auth` plugin?

**A)** API key header
**B)** Username and password sent as Base64-encoded `Authorization: Basic` header
**C)** HMAC signature
**D)** OAuth2 client credentials

**Answer: B**
*Basic Auth credentials consist of a `username` and `password` stored on the Consumer. Kong validates the Base64-encoded `Authorization: Basic <credentials>` header.*

---

## Q18. What is the `config` object on a Plugin instance?

**A)** The routing configuration for the plugin
**B)** The plugin-specific parameters that control its behavior
**C)** The list of consumers the plugin applies to
**D)** The RBAC roles allowed to modify the plugin

**Answer: B**
*Each plugin has a `config` field containing its parameters (e.g., `config.minute=100` for rate limiting, `config.secret=...` for JWT). These are plugin-specific.*

---

## Q19. How do you attach a plugin to a specific Consumer?

**A)** POST to `/consumers/{consumer-id}/plugins`
**B)** POST to `/plugins` with `consumer_id` in the body
**C)** Either A or B
**D)** Plugins cannot be scoped to Consumers

**Answer: C**
*Both methods work. Posting to `/consumers/{id}/plugins` or including `consumer.id` in the plugin body both scope the plugin to that Consumer.*

---

## Q20. A Route is configured with `regex_priority: 0` (default) and another with `regex_priority: 10`. For an incoming request matching both, which Route is selected?

**A)** The one with lower `regex_priority`
**B)** The one with higher `regex_priority`
**C)** The most recently created
**D)** The first listed in the configuration

**Answer: B**
*Higher `regex_priority` wins. This allows operators to control which regex route takes precedence when multiple patterns match.*

---

## Q21. Which field on a Service controls the timeout for connecting to the upstream?

**A)** `read_timeout`
**B)** `write_timeout`
**C)** `connect_timeout`
**D)** `upstream_timeout`

**Answer: C**
*`connect_timeout` (default 60000 ms) is the maximum time to establish a TCP connection to the upstream. `read_timeout` and `write_timeout` control data transfer.*

---

## Q22. Which HTTP status does Kong return when a Plugin's configuration is invalid during creation?

**A)** 500 Internal Server Error
**B)** 404 Not Found
**C)** 400 Bad Request
**D)** 409 Conflict

**Answer: C**
*Invalid plugin configuration (missing required fields, wrong data types) causes Kong's Admin API to return `400 Bad Request` with a descriptive error.*

---

## Q23. What does the `tags` field on a Kong entity enable?

**A)** Plugin-specific labeling for analytics
**B)** Filtering and grouping entities for management and decK selection
**C)** Assigning entities to specific Kong nodes
**D)** Setting execution priority for plugins

**Answer: B**
*Tags are arbitrary strings attached to entities for organizational purposes. decK uses `--select-tag` to scope operations; Kong Manager filters by tags.*

---

## Q24. A Consumer authenticates with a key-auth credential. Which header does Kong add to the upstream request to identify the consumer?

**A)** `X-Consumer-Username`
**B)** `X-API-Key`
**C)** `X-Consumer-ID` and `X-Consumer-Username`
**D)** `Authorization: Bearer {consumer-id}`

**Answer: C**
*After successful authentication, Kong injects `X-Consumer-ID`, `X-Consumer-Username`, and `X-Consumer-Custom-ID` headers into the upstream request.*

---

## Q25. Which Route field controls whether the matched path prefix is forwarded to the upstream?

**A)** `preserve_host`
**B)** `strip_path`
**C)** `path_handling`
**D)** `request_path`

**Answer: B**
*`strip_path: true` removes the matched path prefix from the request before forwarding. `strip_path: false` preserves the full original path.*

---

## Q26. How do you make a plugin apply globally (to all requests through Kong)?

**A)** POST to `/services/global/plugins`
**B)** POST to `/plugins` without specifying a `service`, `route`, or `consumer`
**C)** Set `scope: global` in the plugin config
**D)** POST to `/global/plugins`

**Answer: B**
*A plugin posted to `/plugins` with no `service`, `route`, or `consumer` association is a global plugin — it applies to every request.*

---

## Q27. What happens if you try to create a Route without associating it with a Service?

**A)** Kong creates the Route with a default Service
**B)** Kong returns an error; every Route must be linked to a Service
**C)** The Route matches but returns 503
**D)** The Route is created but inactive until a Service is assigned

**Answer: B**
*Routes must be associated with a Service. Kong's Admin API will return a validation error if no `service` reference is provided.*

---

## Q28. Which Kong entity is primarily used to implement tiered API rate limits (e.g., free vs. premium users)?

**A)** Upstreams
**B)** Consumer Groups
**C)** Routes
**D)** Workspaces

**Answer: B**
*Consumer Groups let you define rate-limiting overrides per group. Free consumers get one limit, premium consumers get another, by assigning them to different Consumer Groups.*

---

## Q29. What is the minimum set of fields required to create a functional Service?

**A)** `name` and `url` (or `host`)
**B)** `name`, `host`, `port`, `protocol`, and `path`
**C)** `name` only
**D)** `host` and at least one Route

**Answer: A**
*At minimum, a Service requires `host` (or the shorthand `url`). `name` is strongly recommended for reference. All other fields have sensible defaults.*

---

## Q30. A plugin is configured at the Service level with `config.minute: 100` and at the Route level with `config.minute: 10`. Which limit applies to requests matching that Route?

**A)** 100 (Service-level wins)
**B)** 10 (Route-level wins because it is more specific)
**C)** 110 (limits are additive)
**D)** The most recently configured value

**Answer: B**
*Route-level plugins are more specific than Service-level plugins. The Route-level `rate-limiting` plugin (10 req/min) takes precedence over the Service-level one (100 req/min).*
