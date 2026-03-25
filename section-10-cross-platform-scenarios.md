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
