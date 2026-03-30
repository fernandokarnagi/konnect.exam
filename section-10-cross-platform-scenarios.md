# Section 10: Cross-Platform Scenarios

> **Exam Theme:** Combining Gateway, Mesh, Dev Portal, Analytics, security controls, and hosting models in real-world architecture scenarios.

---

## Q1. A customer wants to secure both internet-facing API traffic AND service-to-service traffic inside their cluster. Which components should you combine?

**A)** Analytics + AI Gateway — for observability and AI traffic governance
**B)** Dev Portal + Service Catalog — for external and internal discoverability
**C)** API Gateway + Analytics — for traffic routing and error monitoring
**D)** API Gateway + Service Mesh — Gateway secures north-south (external) traffic; Mesh secures east-west (internal service-to-service) traffic using mTLS

**Answer: D**
*North-south = API Gateway (external clients to services). East-west = Service Mesh (service to service inside the cluster). Together they provide full-stack traffic security.*

---

## Q2. Which three components create an end-to-end platform for exposing APIs, securing microservices, and offering a developer portal?

**A)** Service Catalog, Dev Portal, and AI Gateway
**B)** API Gateway, Analytics, and Organizations
**C)** Analytics, Service Catalog, and AI Gateway
**D)** API Gateway, Service Mesh, and Dev Portal

**Answer: D**
*The classic three-component platform story: **Gateway** (expose and secure APIs externally) + **Mesh** (secure microservices internally) + **Dev Portal** (self-service for developers).*

---

## Q3. What happens during a Control Plane outage in a hybrid deployment?

**A)** The Data Plane switches to a read-only mode and rejects all configuration write requests
**B)** Traffic is rerouted to a backup Control Plane automatically through the Konnect failover mechanism
**C)** The Data Plane continues proxying live traffic using its last known good configuration — no traffic interruption occurs; only new configuration changes cannot be applied until connectivity is restored
**D)** All Data Plane nodes immediately stop serving traffic to prevent serving stale configuration

**Answer: C**
*This is operationally critical: a CP outage is a management-plane event, not a traffic event. Customers on hybrid deployments are protected from management-plane failures.*

---

## Q4. Which security and compliance controls belong in an enterprise-ready Konnect design?

**Select TWO correct answers.**

**A)** Rate limiting, load balancing, and proxy_listen configuration
**B)** Semantic caching, PII sanitization, and prompt guard
**C)** SSO/IdP integration and role-based teams — for identity federation and least-privilege access
**D)** Audit logs and customer-managed encryption keys (CMEK) — for accountability and data sovereignty
**E)** Declarative configuration tools like decK and Terraform

**Answer: C, D**
*Enterprise compliance checklist: **SSO** (identity federation), **RBAC** (least-privilege access), **audit logs** (accountability), **CMEK** (data encryption sovereignty).*

---

## Q5. When is hybrid mode the best hosting answer?

**A)** When the customer has fewer than 5 APIs and wants the simplest possible setup
**B)** When the customer wants zero infrastructure management responsibility
**C)** When the customer uses Kubernetes and wants to use the Kong Operator
**D)** When the customer needs Data Planes to run in their own infrastructure — on-prem, private cloud, or specific cloud regions — for control, compliance, or data residency reasons

**Answer: D**
*Hybrid = customer-managed Data Plane, Kong-managed Control Plane. Best for: data residency requirements, on-prem mandates, or needing direct control over DP nodes.*

---

## Q6. When are Dedicated Cloud Gateways the best hosting answer?

**A)** When the customer needs to run gateway nodes on bare metal servers
**B)** When the customer wants fully managed, Kong-operated gateway infrastructure in the cloud — no data plane management overhead
**C)** When the customer's compliance requires all traffic to stay within a private network
**D)** When the customer has strict on-premises data residency requirements

**Answer: B**
*Dedicated Cloud Gateways = Kong manages everything including the infrastructure. Best for: customers who want all the power of Kong Gateway without any operational burden.*

---

## Q7. How would you explain the difference between north-south and east-west security in one sentence?

**A)** North-south is handled by the mesh; east-west is handled by the gateway
**B)** North-south and east-west refer to the same traffic pattern in different cloud environments
**C)** North-south is internal cluster traffic; east-west is external client traffic
**D)** North-south security protects external client-to-service traffic at the perimeter; east-west security protects service-to-service traffic inside the cluster

**Answer: D**
*The one-sentence answer: "North-south = external traffic protected by API Gateway; east-west = internal service traffic protected by Service Mesh."*

---

## Q8. If a question asks for "developer discovery plus runtime traffic protection," which products come to mind?

**A)** Service Catalog + Analytics
**B)** AI Gateway + Service Mesh
**C)** Dev Portal (developer discovery) + API Gateway or Service Mesh (runtime protection)
**D)** Organizations + Roles

**Answer: C**
*"Developer discovery" = Dev Portal. "Runtime traffic protection" = API Gateway (north-south) and/or Service Mesh (east-west). These are the correct product pairings.*

---

## Q9. How would you choose between Gateway, Mesh, Dev Portal, Service Catalog, and Analytics in a cross-platform scenario?

**A)** Always recommend all five components for any enterprise scenario
**B)** Choose based on the customer's cloud provider preference and licensing tier
**C)** Always start with Analytics and add other components based on observed traffic volume
**D)** Map each component to its primary use case: Gateway (external API traffic), Mesh (internal service traffic), Dev Portal (developer self-service), Service Catalog (internal asset inventory), Analytics (observability)

**Answer: D**
*The decision tree: external traffic? → Gateway. Internal traffic? → Mesh. Developer consumption? → Dev Portal. Asset tracking? → Service Catalog. Observability? → Analytics.*

---

## Q10. Why are cross-platform questions often the strongest test of real understanding?

**A)** Because they require memorizing the most detailed configuration parameters
**B)** Because they always involve the most technically complex network topologies
**C)** Because they require correctly mapping multiple components to a real-world scenario — distinguishing what each component does, when to use it, and how they combine, rather than reciting isolated facts
**D)** Because they test pricing and licensing knowledge across the Kong product portfolio

**Answer: C**
*Cross-platform questions validate platform understanding as a whole — not just individual features. They test judgment, not just recall.*

---

## Q11. An enterprise customer has five requirements: (1) external API consumers, (2) internal microservice security, (3) developer self-service, (4) strict EU data residency, (5) tracking all internal services. What is the correct component mapping?

**A)** All five needs are solved by the API Gateway alone
**B)**
- External API consumers → **Dev Portal**
- Internal microservice security → **API Gateway**
- Developer self-service → **Service Catalog**
- EU data residency → **Dedicated Cloud Gateway**
- Tracking internal services → **Analytics**
**C)**
- External API consumers → **API Gateway**
- Internal microservice security → **Service Mesh**
- Developer self-service → **Dev Portal**
- EU data residency → **EU Geo selection**
- Tracking internal services → **Service Catalog**
**D)** All five needs are solved by Service Catalog and Analytics together

**Answer: C**
*Full platform mapping: each component has a distinct role. Gateway + Mesh + Dev Portal + geo + catalog = complete enterprise architecture.*

---

## Q12. A customer says: "We need high availability for API traffic even if our management infrastructure goes down." What best addresses this?

**A)** Multi-organization setup — because organizations are isolated tenants providing redundancy
**B)** Dedicated Cloud Gateways — because Kong manages everything including the failover
**C)** Advanced Analytics — because telemetry buffering prevents data loss during outages
**D)** Hybrid mode — because the Data Plane continues serving traffic using cached configuration even when the Control Plane is unreachable

**Answer: D**
*Hybrid mode + CP/DP separation is the answer. The DP caches configuration locally. A CP outage is a management-plane failure, not a traffic-plane failure.*

---

## Q13. A security architect asks: "How do we ensure only approved identity providers can log into the developer portal AND the Konnect management console?"

**A)** mTLS and MeshTrafficPermission for both access points
**B)** CMEK and audit logs for identity verification
**C)** SSO/IdP integration for Dev Portal (developer login) + SSO/IdP integration for Konnect organization (admin console login) — each configured independently
**D)** Rate limiting and load balancing for traffic-based identity enforcement

**Answer: C**
*SSO/IdP integration operates at two layers: (1) the Dev Portal for developer users and (2) the Konnect organization management for admin/operator users.*

---

## Q14. Which combination forms a complete compliance posture for a regulated enterprise?

**A)** Semantic caching + prompt guard + PII sanitizer
**B)** Load balancing + rate limiting + expressions router + Admin API security
**C)** Declarative config + decK + Kong Operator + Terraform
**D)** SSO/IdP integration + RBAC (role-based teams) + audit logs + CMEK + geo selection

**Answer: D**
*Compliance posture = identity (SSO), access control (RBAC), accountability (audit logs), data protection (CMEK), and data residency (geo). These five together address most regulatory requirements.*

---

## Q15. A fintech company needs to: (1) expose a payments API to external partners, (2) protect internal microservices, (3) allow partners to test the API before integration. Which three components address each need?

**A)** Service Mesh (expose API) → API Gateway (protect internal) → Analytics (partner testing)
**B)** Dev Portal (expose API) → API Gateway (protect microservices) → Service Catalog (partner testing)
**C)** API Gateway (expose payments API) → Service Mesh (protect internal microservices) → Dev Portal (allow partners to test)
**D)** Analytics → Service Catalog → AI Gateway

**Answer: C**
*Classic three-part scenario: external API exposure = Gateway. Internal service security = Mesh. Partner self-service testing = Dev Portal Try It flow.*

---

## Q16. A company adopting AI services wants to prevent sensitive data from reaching external AI providers. Which components are most relevant?

**A)** Dev Portal + SSO integration
**B)** API Gateway + mTLS Service Mesh
**C)** AI Gateway with AI PII Sanitizer (prevent PII from reaching providers) + Config Store (secure API keys)
**D)** Service Catalog + Analytics

**Answer: C**
*AI security scenario: PII Sanitizer redacts sensitive data before it leaves the gateway. Config Store secures AI provider credentials. Both are AI Gateway concerns.*

---

## Q17. A customer wants to reduce AI provider costs without changing their application code. Which feature provides this?

**A)** Prompt compression — by reducing token count of every prompt before forwarding
**B)** Provider-agnostic routing — by routing to the cheapest available provider
**C)** Prompt Guard — by blocking prompts that would generate unnecessarily long responses
**D)** Semantic caching — by returning cached responses for semantically similar prompts without making new LLM calls

**Answer: D**
*Cost reduction without code changes = semantic caching. The gateway handles it transparently — same application, fewer LLM calls, lower bill.*

---

## Q18. How does the combination of Service Catalog and Analytics provide a more complete picture than either alone?

**A)** Analytics populates the Service Catalog with real-time performance data automatically
**B)** Service Catalog provides API documentation that Analytics uses to generate dashboards
**C)** They are redundant — choosing one or the other is sufficient for complete observability
**D)** Service Catalog shows structural metadata (what exists, who owns it, governance status); Analytics shows runtime data (traffic, errors, latency) — together they answer "what is this service?" AND "how is it performing?"

**Answer: D**
*Catalog + Analytics = complete picture. Catalog = "what is this service, who owns it, is it compliant?" Analytics = "how is it behaving, is it within SLA?" Neither alone answers both.*

---

## Q19. A company is being acquired. How does the acquirer migrate the acquired company's APIs into their Konnect organization?

**A)** Organizations can be merged directly in the Konnect UI by an Org Admin with super-admin privileges
**B)** Analytics data from the acquired company transfers automatically during the org merge process
**C)** All APIs must be rewritten to conform to the new organization's standards and re-published
**D)** Organizations are hard-isolated tenants — migration requires exporting configuration via decK and importing it into the acquirer's organization, then reassigning teams and roles

**Answer: D**
*Organizations are hard-isolated tenants. No direct merge exists. Migration requires declarative config export/import (decK) and manually re-establishing team structures and role assignments.*

---

## Q20. A platform team wants developers to discover and test APIs while the security team maintains strict control over which APIs are exposed. Which configuration achieves this?

**A)** Public Dev Portal with no authentication configured for maximum developer access
**B)** API Gateway without a Dev Portal — developers access APIs directly with provided credentials
**C)** Private Dev Portal (authentication required) + approval workflows + role-based access (API Viewer vs application registration) + SSO integration — balancing discovery with control
**D)** Service Catalog exposed to external developers for controlled self-service discovery

**Answer: C**
*Balancing discovery and control: Private portal + SSO + approval workflows + scoped roles. Developers can discover and test; the security team controls who gets in and what they can do.*

---

## Q21. Which scenario best illustrates the need for BOTH the API Gateway and Service Mesh in the same deployment?

**A)** A static website served through a CDN with no dynamic API calls
**B)** A batch processing system with no real-time external API traffic
**C)** A single-page web application calling a monolithic backend API
**D)** A microservices architecture where the frontend calls a public API gateway, and the gateway's backend services call each other internally — external calls need gateway security; internal calls need mesh mTLS

**Answer: D**
*Microservices architecture with external + internal traffic: Gateway secures the external perimeter; Mesh secures lateral service-to-service calls. Both layers are needed for complete security.*

---

## Q22. A platform architect requires that every API be documented, versioned, and security-reviewed before any developer can use it. Which components support this enforcement workflow?

**A)** mTLS + MeshTrafficPermission for pre-deployment verification
**B)** Gateway Manager + decK for configuration validation
**C)** Analytics dashboards for pre-deployment traffic simulation
**D)** Service Catalog (governance scorecards to verify documentation/security before promotion) + Dev Portal (publish only after approval, with controlled visibility and application authentication)

**Answer: D**
*Pre-publication governance: Catalog scorecards verify documentation and security compliance. Dev Portal publish + private visibility + approval workflow ensures only vetted APIs reach developers.*

---

## Q23. What is the correct Konnect component for "developer self-service" AND "controlled API exposure"?

**A)** Analytics — which controls which APIs are visible to which consumers
**B)** API Gateway — which handles both developer authentication and API exposure simultaneously
**C)** Service Catalog — for both asset tracking and developer access management
**D)** Dev Portal — which provides both self-service (documentation, Try It, application registration) and controlled exposure (public/private visibility, approval workflows, role-based access)

**Answer: D**
*Dev Portal is explicitly designed for this balance: developers get self-service while the platform team retains control.*

---

## Q24. A CISO asks: "How do we prove to auditors that no unauthorized changes were made to our API gateway configuration last quarter?"

**A)** Service Catalog scorecard history for the gateway services
**B)** Data Plane telemetry buffering records from the relevant time period
**C)** Analytics dashboards showing traffic patterns and configuration-correlated anomalies
**D)** Audit logs — recording all administrative actions including who made changes, what was changed, and when — combined with RBAC role assignments showing who had permission

**Answer: D**
*Audit evidence = audit logs (what changed, when, by whom) + RBAC records (who had permission). Together they satisfy the auditor's "change accountability" requirement.*

---

## Q25. A team is exposing internal APIs to hundreds of partner companies. Each partner should only see relevant APIs. Which capabilities address multi-tenant API exposure?

**A)** Use geo-based separation, with a separate geo per partner tier
**B)** Create separate Kong Gateway service objects for each partner company
**C)** Use Service Mesh mTLS for partner authentication and access control
**D)** Use Dev Portal private visibility + application authentication + approval workflows — potentially with separate portals per partner tier

**Answer: D**
*Multi-tenant portal: private visibility restricts access. Application authentication provides per-partner credentials. Approval workflows give the platform team control over partner access.*

---

## Q26. How does Konnect's platform model address the "build vs. buy" decision for API infrastructure?

**A)** Konnect only replaces the API gateway component; all other capabilities must be built in-house
**B)** Konnect is only for companies that cannot build their own API management tooling
**C)** Konnect provides Gateway, Mesh, Dev Portal, Service Catalog, AI Gateway, and Analytics as a unified platform — reducing build cost while retaining flexibility through automation tools and open standards
**D)** Konnect requires replacing all existing tools immediately, making the buy decision all-or-nothing

**Answer: C**
*Buy argument: Konnect provides all major API platform capabilities pre-integrated, reducing the engineering cost of building and maintaining each separately.*

---

## Q27. In a cross-platform scenario involving external API consumers and internal microservices in a regulated industry, which statement best describes the complete security architecture?

**A)** Only SSO and RBAC are needed for regulated industries — traffic security is optional
**B)** Deploy an API Gateway for all traffic — no mesh is needed with proper plugin configuration
**C)** Service Mesh alone is sufficient for both internal and external traffic in a regulated environment
**D)** API Gateway secures external traffic; Service Mesh secures internal traffic with mTLS and default-deny; RBAC + audit logs govern administrative access; CMEK + geo selection address data residency

**Answer: D**
*Complete regulated-industry architecture: Gateway (perimeter) + Mesh (lateral) + RBAC/audit (admin governance) + CMEK/geo (data protection). All four layers working together.*

---

## Q28. A startup has 10 services, a small team, no compliance requirements, and needs to start quickly. What is the recommended minimal architecture?

**A)** Start with Service Mesh to establish zero-trust before exposing any APIs
**B)** Start with Service Catalog and Analytics only to understand the landscape first
**C)** Full deployment: Gateway + Mesh + Dev Portal + Analytics + Service Catalog from day one
**D)** Start with API Gateway (routing and basic security) + Dev Portal (developer discovery) in a single organization — add complexity incrementally as needs grow

**Answer: D**
*Start simple: Gateway + Dev Portal covers the core use case. Add Mesh when microservices proliferate. Add Catalog when service inventory becomes unmanageable.*

---

## Q29. When a prospect asks "What Kong products would I use for a complete API lifecycle platform?" which answer is most comprehensive?

**A)** API Gateway only — it handles all lifecycle stages
**B)** Service Catalog + Analytics + Organizations
**C)** AI Gateway + Service Mesh + RBAC
**D)** API Gateway (runtime execution) + Dev Portal (consumer lifecycle) + Service Catalog (service lifecycle + governance) + Analytics (observability) + AI Gateway (AI traffic) + Service Mesh (internal security)

**Answer: D**
*The full Konnect platform story: Gateway (run APIs), Portal (publish and consume), Catalog (govern and track), Analytics (observe), AI Gateway (govern AI traffic), Mesh (secure internal calls).*

---

## Q30. After a security incident where an unauthorized party modified gateway configuration, which combination of controls prevents recurrence?

**Select TWO correct answers.**

**A)** Increase rate limiting and enable semantic caching for configuration endpoints
**B)** Deploy Service Mesh to protect gateway-to-upstream traffic
**C)** Enforce RBAC with minimum necessary roles and enable audit logs to record all configuration changes
**D)** Implement SSO with strong authentication (MFA) and restrict Admin API network access
**E)** Enable telemetry buffering and increase analytics retention for security events

**Answer: C, D**
*Post-incident hardening: RBAC (who can change config) + audit logs (detect/investigate) + SSO with MFA (strong identity) + Admin API network restriction (limit exposure surface). Defense in depth.*
