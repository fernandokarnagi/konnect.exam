# Section 3: API Gateway

> **Exam Theme:** Gateway core function, centralized management, routing, load balancing, expressions router, rate limiting, Admin API security, and declarative configuration.

---

## Q1. What is the core function of Kong Gateway in the Konnect platform?

**A)** To store API configuration and distribute it to Data Planes
**B)** To provide a developer portal for API consumers
**C)** To act as a high-performance reverse proxy that sits between clients and upstream services, enforcing policies at runtime
**D)** To generate OpenAPI specifications from backend services

**Answer: C**
*Kong Gateway is a reverse proxy. Clients call the gateway, the gateway enforces plugins (auth, rate limiting, etc.), then forwards requests to upstream services.*

---

## Q2. What does Gateway Manager give teams from a centralized management perspective?

**A)** Direct SSH access to all Data Plane nodes
**B)** A single interface to configure, monitor, and manage control planes and data planes across all environments
**C)** Automatic code generation for Kong plugin development
**D)** A built-in CI/CD pipeline for deploying API changes

**Answer: B**
*Gateway Manager centralizes CP and DP management across environments — enabling teams to configure services, routes, plugins, and upstreams from one place.*

---

## Q3. What does the `proxy_listen` directive control?

**A)** The port on which the Admin API is accessible
**B)** The IP address and port on which the gateway listens for incoming API client traffic
**C)** The upstream service URL that Kong forwards requests to
**D)** The interval at which the Data Plane polls the Control Plane for configuration

**Answer: B**
*`proxy_listen` defines where Kong accepts incoming proxied requests. Misconfiguring it means the gateway won't be reachable on the expected address and port.*

---

## Q4. How does load balancing improve reliability for upstream services?

**A)** It stores multiple copies of the configuration across Data Planes
**B)** It spreads incoming requests across multiple upstream instances, preventing any single instance from becoming a bottleneck or single point of failure
**C)** It caches upstream responses to reduce load
**D)** It automatically retries failed requests indefinitely

**Answer: B**
*Load balancing distributes traffic across an upstream pool. If one instance fails, traffic routes to healthy instances — improving both reliability and availability.*

---

## Q5. When should you think of the expressions router instead of simple route matching?

**A)** When routes only need to match based on the URL path
**B)** When matching logic requires complex conditions combining HTTP method, headers, query parameters, paths, and other attributes using a domain-specific language
**C)** When the gateway is deployed in hybrid mode only
**D)** When using the Admin API to create routes programmatically

**Answer: B**
*The expressions router uses a DSL for fine-grained, multi-attribute route matching — necessary when simple path/method matching is not expressive enough.*

---

## Q6. What kinds of criteria can be used for rate limiting policies?

**A)** Only IP address
**B)** Only API key
**C)** Consumer identity, IP address, credential type, or a combination — applied per second, minute, hour, or day
**D)** Only upstream service response time

**Answer: C**
*Rate limiting in Kong can be applied per consumer, per IP, per credential, or globally, and can be scoped to different time windows (second, minute, hour, day).*

---

## Q7. Why is securing the Admin API considered a best practice?

**A)** Because the Admin API handles live API traffic that must be encrypted
**B)** Because the Admin API provides unrestricted control over gateway configuration — exposure could allow unauthorized changes to all routes, plugins, and upstreams
**C)** Because the Admin API is publicly accessible by default
**D)** Because unsecured Admin APIs cause higher latency on the proxy port

**Answer: B**
*The Admin API is the configuration interface for Kong. If compromised, an attacker could modify routing rules, disable security plugins, or extract sensitive configuration.*

---

## Q8. How do declarative configuration tools such as decK or Terraform fit a GitOps workflow?

**A)** They replace the need for a Control Plane in hybrid deployments
**B)** They allow gateway configuration to be stored in version-controlled files, reviewed as code, and applied through automated pipelines
**C)** They are only used for initial gateway setup and are not suitable for ongoing changes
**D)** They push configuration directly to the Data Plane, bypassing the Control Plane

**Answer: B**
*decK and Terraform represent configuration as code (YAML/HCL), enabling Git-based review, approval, and automated deployment — the foundation of a GitOps approach.*

---

## Q9. Why is Kong Gateway considered the heart of the data plane?

**A)** Because it manages the Control Plane's configuration database
**B)** Because it is the runtime component that actually proxies requests, enforces all plugin policies, and handles client traffic
**C)** Because it generates the Dev Portal documentation
**D)** Because it stores telemetry data for Analytics

**Answer: B**
*Every API request flows through Kong Gateway at runtime. It is the execution engine for all proxy logic, security, transformations, and traffic policies.*

---

## Q10. How would you explain the difference between runtime gateway traffic and gateway configuration management?

**A)** They are the same thing — configuration changes are applied in real-time to live traffic
**B)** Runtime traffic is handled by the Data Plane (live requests through Kong); configuration management is handled by the Control Plane (defining routes, plugins, upstreams)
**C)** Runtime traffic is handled by the Control Plane; configuration management is handled by the Data Plane
**D)** Both are handled exclusively by decK

**Answer: B**
*This is the CP/DP split applied to the gateway context: Data Plane = live traffic execution; Control Plane = configuration authoring and distribution.*

---

## Q11. A team wants to apply a plugin only to a specific route rather than globally. Which Kong configuration scope should they use?

**A)** Global plugin scope on the Control Plane
**B)** Service-level plugin scope
**C)** Route-level plugin scope
**D)** Consumer-level plugin scope only

**Answer: C**
*Kong plugins can be scoped globally, to a service, to a route, or to a consumer. Route-level scope applies the plugin only to requests matching that specific route.*

---

## Q12. Which statement about declarative configuration with decK is TRUE?

**A)** decK pushes configuration directly to Data Plane nodes, skipping the Control Plane
**B)** decK reads the desired state from YAML files and syncs it to the Konnect Control Plane, reconciling differences
**C)** decK is a real-time traffic analysis tool for Kong Gateway
**D)** decK only works with self-hosted Kong deployments, not Konnect

**Answer: B**
*decK is a declarative configuration tool — it takes a YAML file as the desired state and applies (syncs) it to Kong/Konnect, adding, updating, or removing objects as needed.*

---

## Q13. What is a Kong "upstream" in the context of load balancing?

**A)** The Control Plane that manages the gateway configuration
**B)** A named pool of backend service targets that the gateway distributes traffic across using a configured load balancing algorithm
**C)** A plugin that handles upstream authentication
**D)** The network port on which the gateway receives client requests

**Answer: B**
*An upstream defines the backend pool. It contains one or more targets (host:port) and a load balancing algorithm (round-robin, least-connections, etc.).*

---

## Q14. Which Kong plugin category is responsible for controlling how many requests a consumer or IP can make within a time window?

**A)** Authentication plugins (e.g., JWT, API key)
**B)** Transformation plugins (e.g., request-transformer)
**C)** Traffic control plugins (e.g., Rate Limiting, Rate Limiting Advanced)
**D)** Logging plugins (e.g., HTTP Log, File Log)

**Answer: C**
*Rate Limiting falls under traffic control. It enforces quotas on request frequency — protecting upstreams from overload and ensuring fair usage across consumers.*

---

## Q15. Why are audit logs important for the Admin API?

**A)** They improve gateway performance by caching configuration reads
**B)** They record all configuration changes made through the Admin API, providing an accountability trail for compliance and incident investigation
**C)** They replace the need for encryption on Admin API traffic
**D)** They are used to replay configuration changes during Data Plane disconnects

**Answer: B**
*Audit logs answer "who changed what, when." For the Admin API specifically, this is critical because changes there affect all traffic flowing through the gateway.*

---

## Q16. What is the primary difference between Kong Gateway's traditional routing and the expressions router?

**A)** Traditional routing supports HTTPS; expressions router only supports HTTP
**B)** Traditional routing matches on path and method using simple string patterns; the expressions router uses a DSL that allows complex boolean logic combining any request attribute
**C)** The expressions router is only available in hybrid mode
**D)** Traditional routing is faster but supports fewer upstream targets

**Answer: B**
*Traditional router: simple path/method matching. Expressions router: a DSL enabling `(http.path starts_with "/api") && (http.headers["X-Version"] == "v2")` style rules.*

---

## Q17. A development team wants all requests to `/internal/*` routes to bypass rate limiting but all requests to `/public/*` routes to be rate-limited. How should they configure this in Kong?

**A)** Apply rate limiting globally and use the expressions router to exclude `/internal/*` routes
**B)** Apply rate limiting at the service level for the public service; do not apply it to the internal service or routes
**C)** Apply rate limiting as a route-level plugin only to routes matching `/public/*`, leaving `/internal/*` routes without the plugin
**D)** Rate limiting cannot be selectively applied — it is always global

**Answer: C**
*Route-level plugin scoping allows different policies on different routes. Apply rate limiting only to the `/public/*` routes; the `/internal/*` routes remain unrestricted.*

---

## Q18. How does Kong Gateway implement health checks for upstream targets?

**A)** Health checks are not supported — unhealthy targets must be removed manually
**B)** Kong supports passive health checks (based on response codes from proxied traffic) and active health checks (Kong probes targets periodically), automatically removing or re-adding targets based on health status
**C)** Health checks are performed by the Control Plane and results are pushed to the Data Plane
**D)** Health checks require a dedicated monitoring plugin installed on each upstream target

**Answer: B**
*Kong supports both active (probe-based) and passive (traffic-based) health checks. Unhealthy targets are automatically removed from the load balancing pool and restored when healthy again.*

---

## Q19. What does the term "consumer" mean in Kong Gateway?

**A)** Any upstream service that consumes data from Kong
**B)** A representation of an API client (user, application, or service) that can be authenticated, rate-limited, and tracked independently within Kong
**C)** A Data Plane node that consumes configuration from the Control Plane
**D)** An analytics record representing an API call

**Answer: B**
*A consumer in Kong is an entity with credentials (API key, JWT, OAuth token) that Kong can identify, authenticate, and apply consumer-specific policies to.*

---

## Q20. Which of the following best describes how Kong Gateway handles a request when multiple plugins are applied to a route?

**A)** Only the first plugin in the list is executed; others are ignored
**B)** All applicable plugins execute in a defined priority order based on their phase (access, header_filter, body_filter, log), allowing complex multi-step policy enforcement
**C)** Plugins execute in parallel simultaneously to reduce latency
**D)** Plugin execution order is random and cannot be controlled

**Answer: B**
*Kong's plugin system executes plugins in priority order across lifecycle phases. This allows chaining: authentication first, then rate limiting, then transformations, then logging.*

---

## Q21. What is the purpose of Kong Gateway's "service" object?

**A)** It represents a consumer application that calls the API
**B)** It is an abstraction for an upstream API or microservice — defining the protocol, host, port, and path that Kong uses when forwarding requests
**C)** It is a grouping of routes with shared configuration
**D)** It stores the credentials used by Kong to authenticate to upstream services

**Answer: B**
*A Service in Kong represents the upstream backend. A Route points to a Service; the Service defines where and how Kong forwards the request (host, port, path, protocol).*

---

## Q22. Why should the Admin API port be bound only to a loopback or internal network interface rather than a public interface?

**A)** Because public interfaces have lower bandwidth, slowing down configuration changes
**B)** Because binding to a public interface exposes the full gateway configuration management capability to potential attackers on the internet
**C)** Because the Admin API only functions correctly on loopback interfaces
**D)** Because the proxy_listen port requires the Admin API port to be private

**Answer: B**
*The Admin API has no built-in authentication by default in some configurations. Exposing it on a public interface means anyone who can reach it can read or modify all gateway configuration.*

---

## Q23. What is the difference between a "route" and a "service" in Kong's data model?

**A)** Routes store upstream connection details; services store request matching rules
**B)** A route defines the rules for matching incoming requests (path, method, headers, host); a service defines the upstream backend that matching requests are forwarded to
**C)** They are the same object with different names in different Kong versions
**D)** Services are configured in decK; routes are configured in the Konnect UI only

**Answer: B**
*Route = what incoming request looks like (matching rules). Service = where the request goes (upstream definition). A service can have many routes.*

---

## Q24. How does Kong Gateway support canary deployments or A/B testing?

**A)** Kong does not support traffic splitting natively
**B)** By using multiple upstream targets with weighted load balancing, or by configuring multiple services with traffic-weight plugins to split traffic between versions
**C)** By creating separate Data Planes for each version
**D)** By using the expressions router to permanently route all traffic to a single version

**Answer: B**
*Weighted upstream targets allow Kong to send X% of traffic to v1 and Y% to v2. This is the foundation of canary deployments and gradual rollouts at the gateway layer.*

---

## Q25. Which Kong plugin would be appropriate for adding authentication to an API using industry-standard tokens with expiry and signing?

**A)** Basic Authentication plugin
**B)** IP Restriction plugin
**C)** JWT (JSON Web Token) plugin
**D)** Request Termination plugin

**Answer: C**
*JWT plugin validates JSON Web Tokens — industry-standard signed tokens with expiry. Appropriate for modern OAuth2/OIDC flows and service-to-service authentication.*

---

## Q26. What is "declarative configuration" in the context of Kong Gateway?

**A)** A configuration approach where you write scripts that imperatively create each Kong object via API calls
**B)** A configuration approach where you define the desired final state of all Kong objects in a file, and a tool (decK or Kong's native declarative config) reconciles the current state to match
**C)** A method of configuring Kong that only works in Kubernetes environments
**D)** A Kong feature that automatically generates configuration from API traffic patterns

**Answer: B**
*Declarative config = desired state in a file. You say "I want these services, routes, plugins" and the tool makes reality match — regardless of what currently exists.*

---

## Q27. Which load balancing algorithm distributes requests by cycling through upstream targets in order?

**A)** Least Connections
**B)** IP Hash
**C)** Round Robin
**D)** Random

**Answer: C**
*Round-robin sends request 1 to target A, request 2 to target B, request 3 to target C, then cycles back. It's the simplest and most commonly used algorithm.*

---

## Q28. A team has deployed Kong Gateway and finds that occasional upstream failures cause poor user experience. They want Kong to stop sending traffic to failing targets automatically. Which feature enables this?

**A)** Rate limiting with burst configuration
**B)** Passive health checks — Kong monitors responses from proxied traffic and marks targets as unhealthy when they return too many errors
**C)** The expressions router with error-based routing rules
**D)** Declarative configuration with automatic rollback

**Answer: B**
*Passive health checks monitor live traffic responses. When a target returns too many 5xx errors, Kong automatically removes it from the upstream pool, protecting users from hitting the bad target.*

---

## Q29. What is the recommended approach for securing the Konnect Admin API in a production environment?

**A)** Leave it open but monitor it with analytics
**B)** Bind it to a private/internal interface only, enable RBAC, use HTTPS, and restrict access with network policies or firewall rules
**C)** Disable the Admin API entirely and use decK for all changes
**D)** Use basic authentication with a shared team password

**Answer: B**
*Production Admin API security: private binding + RBAC + HTTPS + network-level access controls. Multiple layers of defense to prevent unauthorized configuration access.*

---

## Q30. In a GitOps workflow using decK, what typically triggers a decK sync operation?

**A)** Manually logging into the Konnect UI and clicking sync
**B)** A CI/CD pipeline triggered by a pull request merge to the main branch of the repository containing the Kong configuration YAML files
**C)** Automatic scheduled polling by decK every 5 minutes
**D)** A Konnect webhook fired when the Control Plane detects configuration drift

**Answer: B**
*GitOps with decK: Config YAML lives in Git → PR reviewed and merged → CI/CD pipeline runs `deck sync` → Control Plane updated → Data Planes receive new config.*

---

## Q31. What is APIOps, and how does it differ from general DevOps?

**A)** APIOps is a marketing term for API management; it has no technical definition
**B)** APIOps applies DevOps principles (automation, version control, CI/CD) specifically to the API lifecycle — from design through deployment and governance — treating API configuration as code
**C)** APIOps refers only to operating the Admin API securely
**D)** APIOps is the same as GitOps with no distinction

**Answer: B**
*APIOps = DevOps for APIs. Design-first with OAS, store config in Git, automate deployment via CI/CD (inso + decK), govern with scorecards. Every API change is reviewed, tested, and deployed programmatically.*

---

## Q32. In an APIOps pipeline, what is the role of the `inso generate config` command?

**A)** It generates a random API key for Kong consumers
**B)** It converts an OpenAPI Specification file into a Kong-compatible decK state file that defines services, routes, and plugins
**C)** It generates the nginx configuration for the Data Plane
**D)** It creates a Konnect Control Plane from the OAS

**Answer: B**
*`inso generate config` reads an OAS file and produces a `kong.yaml` decK state file — with services, routes, and optionally plugins derived from `x-kong-*` OAS extensions. This bridges design and deployment.*

---

## Q33. What is the recommended APIOps pipeline sequence from design to deployed gateway?

**A)** `deck sync` → `inso lint spec` → `inso generate config` → `deck diff`
**B)** Design OAS in Insomnia → `inso lint spec` → `inso generate config` → `deck validate` → `deck diff` → `deck sync`
**C)** `deck dump` → Edit YAML → `deck sync` → `inso run test`
**D)** `inso run test` → Deploy → `deck validate` → `deck sync`

**Answer: B**
*The full APIOps pipeline: design spec → lint it → generate Kong config → validate the config → diff against live state (review) → sync to deploy. Each stage is a quality gate.*

---

## Q34. Which `x-kong-*` OAS extension would you add to an OpenAPI path operation to attach a rate-limiting plugin via APIOps?

**A)** `x-kong-route-defaults`
**B)** `x-kong-plugin-rate-limiting`
**C)** `x-kong-service-name`
**D)** `x-kong-upstream`

**Answer: B**
*`x-kong-plugin-<plugin-name>` extensions embed plugin configuration directly in the OAS. `inso generate config` translates them into plugin objects in the generated decK state file.*

---

## Q35. In a multi-environment APIOps pipeline (dev → staging → prod), how does decK's `--select-tag` flag enable environment-specific deployments?

**A)** It filters which Kong plugins execute at runtime
**B)** It scopes `deck sync` to only manage entities tagged with a specific value — allowing separate pipelines for dev, staging, and prod tags without affecting each other
**C)** It selects which OAS file to use for config generation
**D)** It limits which Konnect geo receives the configuration

**Answer: B**
*Tag-scoped deployments: `deck sync --select-tag env:dev` only touches dev-tagged entities. Staging and prod pipelines use their own tags — preventing cross-environment interference on the same Control Plane.*

---

## Q36. What is the Kong Ingress Controller (KIC), and when would you use it instead of decK?

**A)** KIC is a Kubernetes plugin; use it when decK is unavailable
**B)** KIC is a Kubernetes controller that watches Kubernetes resources (Ingress, KongPlugin CRDs) and automatically configures Kong — preferred over decK when the team manages Kong configuration entirely through Kubernetes manifests and kubectl
**C)** KIC replaces the Konnect Control Plane in Kubernetes deployments
**D)** KIC is a CLI tool equivalent to decK but written in Go

**Answer: B**
*KIC = Kubernetes-native configuration. Teams express Kong config as Kubernetes resources. Changes to k8s manifests automatically reconcile Kong. Choose KIC when Kubernetes is the source of truth; choose decK for GitOps YAML files.*

---

## Q37. Which `inso` command validates that an OpenAPI spec is structurally correct before using it in an APIOps pipeline?

**A)** `inso run test`
**B)** `inso generate config`
**C)** `inso lint spec`
**D)** `inso export spec`

**Answer: C**
*`inso lint spec` validates the OAS document against OpenAPI rules without generating anything. It's the first quality gate in an APIOps pipeline — catching spec errors before they propagate to gateway config.*
