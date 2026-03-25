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
