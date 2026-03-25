# Section 10: Cross-Platform Scenarios

> **Exam Theme:** Combining Gateway, Mesh, Dev Portal, Analytics, security controls, and hosting models in real-world architecture scenarios.

---

## Q1. A customer wants to secure both internet-facing API traffic AND service-to-service traffic inside their cluster. Which platform components should you combine, and why?

**A)** Dev Portal + Service Catalog — for external and internal discoverability
**B)** API Gateway + Service Mesh — Gateway secures north-south (external) traffic; Mesh secures east-west (internal service-to-service) traffic using mTLS
**C)** Analytics + AI Gateway — for observability and AI traffic governance
**D)** API Gateway + Analytics — for traffic routing and error monitoring

**Answer: B**
*North-south = API Gateway (external clients to services). East-west = Service Mesh (service to service inside the cluster). Together they provide full-stack traffic security.*

---

## Q2. Which three components create an end-to-end platform for exposing APIs, securing microservices, and offering a developer portal?

**A)** Analytics, Service Catalog, and AI Gateway
**B)** API Gateway, Service Mesh, and Dev Portal
**C)** API Gateway, Analytics, and Organizations
**D)** Service Catalog, Dev Portal, and AI Gateway

**Answer: B**
*The classic three-component platform story: **Gateway** (expose and secure APIs externally) + **Mesh** (secure microservices internally) + **Dev Portal** (self-service for developers).*

---

## Q3. What happens during a Control Plane outage in a hybrid deployment, and why is that operationally important?

**A)** All Data Plane nodes immediately stop serving traffic
**B)** The Data Plane continues proxying live traffic using its last known good configuration — no traffic interruption occurs; only new configuration changes cannot be applied until connectivity is restored
**C)** Traffic is rerouted to a backup Control Plane automatically
**D)** The Data Plane switches to a read-only mode and rejects write requests only

**Answer: B**
*This is operationally critical: a CP outage is a management-plane event, not a traffic event. Customers on hybrid deployments are protected from management-plane failures.*

---

## Q4. Which security and compliance controls should you mention in an enterprise-ready Konnect design?

**A)** Rate limiting, load balancing, and proxy_listen configuration
**B)** SSO/IdP integration, role-based teams, audit logs, and customer-managed encryption keys (CMEK)
**C)** Semantic caching, PII sanitization, and prompt guard
**D)** Declarative configuration, decK, and Terraform

**Answer: B**
*Enterprise compliance checklist: **SSO** (identity federation), **RBAC** (least-privilege access), **audit logs** (accountability), **CMEK** (data encryption sovereignty).*

---

## Q5. When is hybrid mode the best hosting answer?

**A)** When the customer wants zero infrastructure management responsibility
**B)** When the customer needs Data Planes to run in their own infrastructure — on-prem, private cloud, or specific cloud regions — for control, compliance, or data residency reasons
**C)** When the customer has fewer than 5 APIs and wants the simplest setup
**D)** When the customer uses Kubernetes and wants Kong Operator

**Answer: B**
*Hybrid = customer-managed Data Plane, Kong-managed Control Plane. Best for: data residency requirements, on-prem infrastructure mandates, or needing direct control over DP nodes.*

---

## Q6. When are Dedicated Cloud Gateways the best hosting answer?

**A)** When the customer has strict on-premises data residency requirements
**B)** When the customer wants fully managed, Kong-operated gateway infrastructure in the cloud — no data plane management overhead
**C)** When the customer needs to run gateway nodes on bare metal
**D)** When the customer's compliance requires all traffic to stay within a private network

**Answer: B**
*Dedicated Cloud Gateways = Kong manages everything including the infrastructure. Best for: customers who want all the power of Kong Gateway without any operational burden.*

---

## Q7. How would you explain the difference between north-south and east-west security in one sentence?

**A)** North-south is internal; east-west is external
**B)** North-south security protects external client-to-service traffic at the perimeter; east-west security protects service-to-service traffic inside the cluster
**C)** North-south is handled by the mesh; east-west is handled by the gateway
**D)** Both terms refer to the same traffic pattern in different cloud environments

**Answer: B**
*The one-sentence answer: "North-south = external traffic protected by API Gateway; east-west = internal service traffic protected by Service Mesh."*

---

## Q8. If a question asks for "developer discovery plus runtime traffic protection," which products should immediately come to mind?

**A)** Service Catalog + Analytics
**B)** AI Gateway + Service Mesh
**C)** Dev Portal (developer discovery) + API Gateway or Service Mesh (runtime protection)
**D)** Organizations + Roles

**Answer: C**
*"Developer discovery" = Dev Portal. "Runtime traffic protection" = API Gateway (north-south) and/or Service Mesh (east-west). These are the correct product pairings.*

---

## Q9. How would you choose between Gateway, Mesh, Dev Portal, Service Catalog, and Analytics in a cross-platform scenario?

**A)** Always recommend all five components for any enterprise scenario
**B)** Map each component to its primary use case: Gateway (external API traffic), Mesh (internal service traffic), Dev Portal (developer self-service), Service Catalog (internal asset inventory), Analytics (observability)
**C)** Choose based on the customer's cloud provider preference
**D)** Always start with Analytics and add other components based on traffic volume

**Answer: B**
*The decision tree: external traffic? → Gateway. Internal traffic? → Mesh. Developer consumption? → Dev Portal. Asset tracking? → Service Catalog. Observability? → Analytics.*

---

## Q10. Why are cross-platform questions often the strongest test of real understanding on the Konnect Associate exam?

**A)** Because they require memorizing the most configuration details
**B)** Because they require correctly mapping multiple components to a real-world scenario — distinguishing what each component does, when to use it, and how components combine, rather than reciting isolated facts
**C)** Because they always involve the most technically complex configurations
**D)** Because they test pricing and licensing knowledge

**Answer: B**
*Cross-platform questions validate that you understand the platform as a whole — not just individual features. They test judgment, not just recall.*

---

## Q11. An enterprise customer has: (1) external API consumers, (2) internal microservices, (3) a need for developers to self-subscribe to APIs, (4) strict EU data residency, and (5) a requirement to track all internal services. Map each need to the correct Konnect component.

**A)** All five needs are solved by API Gateway alone
**B)**
- External API consumers → **API Gateway**
- Internal microservices security → **Service Mesh**
- Developer self-service → **Dev Portal**
- EU data residency → **Geo selection (EU geo)**
- Internal service tracking → **Service Catalog**
**C)**
- External API consumers → **Dev Portal**
- Internal microservices → **API Gateway**
- Developer self-service → **Service Catalog**
- EU data residency → **Dedicated Cloud Gateway**
- Internal service tracking → **Analytics**
**D)** All five needs are solved by Service Catalog and Analytics

**Answer: B**
*This is the full platform mapping: each component has a distinct role. Gateway + Mesh + Dev Portal + geo + catalog = complete enterprise architecture.*

---

## Q12. A customer says: "We need high availability for API traffic even if our management infrastructure goes down." Which deployment model and architectural fact best addresses this?

**A)** Dedicated Cloud Gateways — because Kong manages everything
**B)** Hybrid mode — because the Data Plane continues serving traffic using cached configuration even when the Control Plane is unreachable
**C)** Multi-organization setup — because organizations are isolated tenants
**D)** Advanced Analytics — because telemetry buffering prevents data loss during outages

**Answer: B**
*Hybrid mode + CP/DP separation is the answer. The DP caches configuration locally. A CP outage is a management-plane failure, not a traffic-plane failure — live requests continue uninterrupted.*

---

## Q13. A security architect asks: "How do we ensure that only approved identity providers can log into the developer portal AND the Konnect management console?" Which two Konnect features address this?

**A)** Rate limiting and load balancing
**B)** SSO/IdP integration for Dev Portal (developer login) and SSO/IdP integration for Konnect organization (admin console login)
**C)** mTLS and MeshTrafficPermission
**D)** CMEK and audit logs

**Answer: B**
*SSO/IdP integration operates at two layers: (1) the Dev Portal for developer users, and (2) the Konnect organization management for admin/operator users. Both are independently configurable.*

---

## Q14. Which combination of features forms a complete compliance posture for a regulated enterprise using Konnect?

**A)** Semantic caching + prompt guard + PII sanitizer
**B)** SSO/IdP integration + RBAC (role-based teams) + audit logs + CMEK + geo selection
**C)** Declarative config + decK + Kong Operator + Terraform
**D)** Load balancing + rate limiting + expressions router + Admin API security

**Answer: B**
*Compliance posture = identity (SSO), access control (RBAC), accountability (audit logs), data protection (CMEK), and data residency (geo). These five together address most regulatory requirements.*

---

## Q15. A fintech company needs to: (1) expose a payments API to external partners, (2) protect microservice-to-microservice calls inside their cluster, and (3) allow partners to test the API before integration. Which three components address each need?

**A)** Analytics → Service Catalog → AI Gateway
**B)** API Gateway (expose payments API) → Service Mesh (protect internal microservices) → Dev Portal (allow partners to test)
**C)** Dev Portal (expose API) → API Gateway (protect microservices) → Service Catalog (partner testing)
**D)** Service Mesh (expose API) → API Gateway (internal calls) → Analytics (partner testing)

**Answer: B**
*Classic three-part scenario: external API exposure = Gateway. Internal service security = Mesh. Partner self-service testing = Dev Portal Try It flow.*

---

## Q16. In a scenario where a company is adopting AI services and wants to prevent sensitive data from reaching external AI providers, which Konnect components are most relevant?

**A)** Service Catalog + Analytics
**B)** AI Gateway with AI PII Sanitizer (prevent PII from reaching providers) + Config Store (secure API keys)
**C)** API Gateway + mTLS
**D)** Dev Portal + SSO integration

**Answer: B**
*AI security scenario: PII Sanitizer redacts sensitive data before it leaves the gateway. Config Store secures AI provider credentials. Both are AI Gateway concerns.*

---

## Q17. A customer wants to reduce AI provider costs without changing their application code. Which AI Gateway feature provides this, and how does it work?

**A)** Prompt Guard — by blocking expensive prompts
**B)** Semantic caching — by returning cached responses for semantically similar prompts without making new LLM calls
**C)** Prompt compression — by reducing token count of every prompt
**D)** Provider-agnostic routing — by routing to the cheapest available provider

**Answer: B**
*Cost reduction without code changes = semantic caching. The gateway handles it transparently — same application, fewer LLM calls, lower bill.*

---

## Q18. How does the combination of Service Catalog and Analytics provide a more complete picture than either alone?

**A)** They are redundant — choosing one or the other is sufficient
**B)** Service Catalog shows structural metadata (what exists, who owns it, governance status); Analytics shows runtime data (traffic volume, errors, latency) — together they answer "what is this service?" AND "how is it performing?"
**C)** Analytics populates the Service Catalog with real-time data automatically
**D)** Service Catalog provides API documentation that Analytics uses to generate dashboards

**Answer: B**
*Catalog + Analytics = complete picture. Catalog = "what is this service, who owns it, is it compliant?" Analytics = "how is it behaving, is it healthy, is it within SLA?" Neither alone answers both questions.*

---

## Q19. A company is being acquired. The acquirer wants to move all the acquired company's APIs under their Konnect organization. What is the key consideration?

**A)** All APIs must be rewritten to conform to the new organization's standards
**B)** The acquired company's Konnect organization is a separate isolated tenant — migration requires exporting configuration (via decK) and importing it into the acquirer's organization, then reassigning teams and roles
**C)** Organizations can be merged directly in the Konnect UI by an Org Admin
**D)** Analytics data from the acquired company transfers automatically when organizations are merged

**Answer: B**
*Organizations are hard-isolated tenants. No direct merge exists. Migration requires declarative config export/import (decK) and manually re-establishing team structures and role assignments.*

---

## Q20. A platform team wants to ensure developers can discover and test APIs while the security team maintains strict control over which APIs are exposed and to whom. Which components and configuration model achieves this?

**A)** Public Dev Portal + no authentication = maximum developer access
**B)** Private Dev Portal (authentication required) + approval workflows + role-based access (API Viewer vs application registration) + SSO integration — balancing discovery with control
**C)** Service Catalog exposed to external developers
**D)** API Gateway without a Dev Portal — developers access APIs directly

**Answer: B**
*Balancing discovery and control: Private portal + SSO + approval workflows + scoped roles. Developers can discover and test; the security team controls who gets in and what they can do.*

---

## Q21. Which scenario best illustrates the need for BOTH the API Gateway and Service Mesh in the same deployment?

**A)** A single-page web application calling a monolithic backend API
**B)** A microservices architecture where the frontend calls a public API gateway, and the gateway's backend services call each other internally — external calls need gateway security; internal calls need mesh mTLS
**C)** A batch processing system with no real-time API traffic
**D)** A static website served through a CDN

**Answer: B**
*Microservices architecture with external + internal traffic: Gateway secures the external perimeter; Mesh secures lateral service-to-service calls. Both layers are needed for complete security.*

---

## Q22. A platform architect presents a requirement: "We need every API to be documented, versioned, and security-reviewed before it can be used by any developer." Which Konnect components support an enforcement workflow for this?

**A)** Analytics dashboards
**B)** Service Catalog (governance scorecards to verify documentation/security before promotion) + Dev Portal (publish only after approval, with controlled visibility and application authentication)
**C)** Gateway Manager + decK
**D)** mTLS + MeshTrafficPermission

**Answer: B**
*Pre-publication governance: Catalog scorecards verify documentation and security compliance. Dev Portal publish + private visibility + approval workflow ensures only vetted APIs reach developers.*

---

## Q23. What is the correct Konnect component to recommend when asked about "developer self-service" AND "controlled API exposure"?

**A)** Service Catalog — for asset tracking
**B)** Dev Portal — which provides both self-service (documentation, Try It, application registration) and controlled exposure (public/private visibility, approval workflows, role-based access)
**C)** API Gateway — which handles both developer authentication and API exposure
**D)** Analytics — which controls which APIs are visible to which consumers

**Answer: B**
*Dev Portal is explicitly designed for this balance: developers get self-service (discover, test, subscribe) while the platform team retains control (visibility, approvals, authentication strategies).*

---

## Q24. A CISO asks: "How do we prove to auditors that no unauthorized changes were made to our API gateway configuration last quarter?" Which Konnect features provide the evidence?

**A)** Analytics dashboards showing traffic patterns
**B)** Audit logs — which record all administrative actions including who made configuration changes, what was changed, and when — combined with RBAC role assignments showing who had permission
**C)** Service Catalog scorecard history
**D)** Data Plane telemetry buffering records

**Answer: B**
*Audit evidence = audit logs (what changed, when, by whom) + RBAC records (who had permission to make changes). Together they satisfy the auditor's "change accountability" requirement.*

---

## Q25. A team is building a platform for exposing internal APIs to hundreds of partner companies. Each partner should only see APIs relevant to them. Which Konnect capabilities address multi-tenant API exposure?

**A)** Create separate Kong Gateway services for each partner
**B)** Use Dev Portal private visibility + application authentication + approval workflows to control which partners can access which APIs — potentially with separate portals per partner tier
**C)** Use Service Mesh mTLS for partner authentication
**D)** Use geo-based separation, with a separate geo per partner

**Answer: B**
*Multi-tenant portal: private visibility restricts access. Application authentication provides per-partner credentials. Approval workflows give the platform team control over which partner accesses what.*

---

## Q26. How does Konnect's platform model address the "build vs. buy" decision for API infrastructure?

**A)** Konnect is only for companies that cannot build their own API management tooling
**B)** Konnect provides Gateway, Mesh, Dev Portal, Service Catalog, AI Gateway, and Analytics as a unified platform — reducing the build cost of integrating these capabilities separately while retaining flexibility through automation tools and open standards
**C)** Konnect requires replacing all existing tools immediately, making the buy decision all-or-nothing
**D)** Konnect only replaces the API gateway component; all other capabilities must be built in-house

**Answer: B**
*Buy argument: Konnect provides all major API platform capabilities pre-integrated, reducing the engineering cost of building and maintaining each separately — while decK, Terraform, and APIs preserve flexibility.*

---

## Q27. In a cross-platform scenario involving both external API consumers and internal microservices in a regulated industry, which statement best describes the complete security architecture?

**A)** Deploy an API Gateway for all traffic — no mesh is needed
**B)** API Gateway secures external traffic (authentication, rate limiting, TLS termination); Service Mesh secures internal traffic (mTLS, MeshTrafficPermission default-deny); RBAC + audit logs govern administrative access; CMEK + geo selection address data residency
**C)** Service Mesh alone is sufficient for both internal and external traffic
**D)** Only SSO and RBAC are needed for regulated industries

**Answer: B**
*Complete regulated-industry architecture: Gateway (perimeter) + Mesh (lateral) + RBAC/audit (admin governance) + CMEK/geo (data protection). All four layers working together.*

---

## Q28. A startup is adopting Konnect for the first time. They have 10 services, a small team, no compliance requirements, and need to get started quickly. What is the recommended minimal starting architecture?

**A)** Full deployment: Gateway + Mesh + Dev Portal + Analytics + Service Catalog
**B)** Start with API Gateway (for routing and basic security) + Dev Portal (for developer discovery) in a single organization with a simple team structure — add complexity incrementally as needs grow
**C)** Start with Service Catalog and Analytics only
**D)** Start with Service Mesh to establish zero-trust before exposing any APIs

**Answer: B**
*Start simple: Gateway + Dev Portal covers the core use case. Add Mesh when microservices proliferate. Add Catalog when service inventory becomes unmanageable. Add AI Gateway when AI use cases emerge.*

---

## Q29. When a prospect asks "What Kong products would I use for a complete API lifecycle platform?" which components form the most comprehensive answer?

**A)** API Gateway only — it handles all lifecycle stages
**B)** API Gateway (runtime execution) + Dev Portal (consumer lifecycle) + Service Catalog (service lifecycle + governance) + Analytics (observability) + AI Gateway (AI traffic) + Service Mesh (internal security)
**C)** Service Catalog + Analytics + Organizations
**D)** AI Gateway + Service Mesh + RBAC

**Answer: B**
*The full Konnect platform story: Gateway (run APIs), Portal (publish and consume APIs), Catalog (govern and track APIs), Analytics (observe APIs), AI Gateway (govern AI traffic), Mesh (secure internal calls).*

---

## Q30. A company has just suffered a security incident where an unauthorized party modified gateway configuration. In the post-incident review, the team asks how to prevent this in the future. Which combination of Konnect controls addresses this?

**A)** Increase rate limiting and enable semantic caching
**B)** Enforce RBAC (minimum necessary roles for all users), enable audit logs (record all configuration changes), secure the Admin API (restrict network access), and implement SSO (centralized identity with strong authentication) — then review role assignments regularly
**C)** Deploy Service Mesh to protect gateway-to-upstream traffic
**D)** Enable telemetry buffering and increase analytics retention

**Answer: B**
*Post-incident hardening: RBAC (who can change config) + audit logs (detect and investigate changes) + Admin API security (limit exposure surface) + SSO with MFA (strong identity). Defense in depth for gateway administration.*
