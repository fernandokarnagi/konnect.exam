# Section 2: High-Level Architecture

> **Exam Theme:** Control Plane vs Data Plane, Konnect organizations, Dedicated Cloud Gateways, and automation tools — Terraform, decK, Konnect APIs, Kong Operator.

---

## Q1. Explain the difference between the Control Plane and the Data Plane in Konnect.

**A)** The Control Plane proxies live traffic; the Data Plane stores configuration
**B)** The Control Plane stores and distributes configuration; the Data Plane proxies live API traffic and enforces policies
**C)** Both planes share responsibility for live traffic handling
**D)** The Control Plane runs inside Kubernetes; the Data Plane always runs in the cloud

**Answer: B**
*Control Plane = "brain" (configuration, management, UI). Data Plane = "muscle" (runtime proxy, policy enforcement, live traffic).*

---

## Q2. What happens to live traffic if the Data Plane loses connectivity to the Control Plane?

**A)** All traffic is immediately rejected until connectivity is restored
**B)** The Data Plane shuts down and restarts automatically
**C)** The Data Plane continues proxying traffic using its last known good configuration
**D)** Traffic is rerouted directly to the Control Plane

**Answer: C**
*The Data Plane caches configuration locally. A Control Plane disconnect does not interrupt live traffic — it just prevents new configuration changes from propagating.*

---

## Q3. Which component is responsible for storing and propagating configuration changes?

**A)** Data Plane
**B)** Kong Operator
**C)** Control Plane
**D)** decK

**Answer: C**
*The Control Plane is the single source of truth for all configuration. It propagates changes down to connected Data Planes.*

---

## Q4. Why is the Control Plane described as the "brain" and the Data Plane as the "muscle"?

**A)** Because the Control Plane executes requests faster than the Data Plane
**B)** Because the Control Plane makes decisions and holds configuration, while the Data Plane does the heavy lifting of proxying and enforcing policies at runtime
**C)** Because the Data Plane controls the UI and the Control Plane handles traffic
**D)** Because both planes have equal roles in Konnect

**Answer: B**
*This analogy captures the architectural split: intelligence/configuration sits in the Control Plane; execution/enforcement sits in the Data Plane.*

---

## Q5. What is a Konnect organization, and why is it important for isolation?

**A)** A logical group of developers with shared API credentials
**B)** A billing unit that groups gateway instances
**C)** An isolated tenant boundary with its own resources, teams, roles, and configurations — separate from other organizations
**D)** A region-specific deployment unit tied to a single cloud provider

**Answer: C**
*An organization is a top-level tenant. Resources, permissions, and data inside one organization are not visible to or shared with another organization.*

---

## Q6. How does a Dedicated Cloud Gateway differ from a self-managed data plane in hybrid mode?

**A)** Dedicated Cloud Gateways are managed by the customer; hybrid mode is fully managed by Kong
**B)** Dedicated Cloud Gateways are fully managed infrastructure provisioned and operated by Kong; hybrid mode requires the customer to deploy and manage their own data plane nodes
**C)** There is no operational difference between the two
**D)** Hybrid mode only works in a single cloud, while Dedicated Cloud Gateways support multi-cloud

**Answer: B**
*Dedicated Cloud Gateways remove the operational burden entirely — Kong manages the infrastructure. Hybrid mode gives customers control over where and how data plane nodes run.*

---

## Q7. Which automation tools are specifically called out in the study guide, and what are they used for?

**A)** Ansible, Puppet, Chef — for server provisioning
**B)** Terraform, decK, Konnect APIs, Kong Operator — for automating configuration, infrastructure, and Kubernetes-based deployment
**C)** Jenkins, GitHub Actions, CircleCI — for CI/CD pipelines
**D)** Helm, Argo CD, Flux — for GitOps-only deployments

**Answer: B**
*The four tools are: Terraform (infrastructure as code), decK (declarative Kong config), Konnect APIs (programmatic management), Kong Operator (Kubernetes-native management).*

---

## Q8. When would Kong Operator be the right automation tool to mention in a Kubernetes-focused discussion?

**A)** When the team uses Terraform for all infrastructure changes
**B)** When the team manages Konnect configuration through YAML files in a Git repository using decK
**C)** When the team runs Kong on Kubernetes and wants to manage Kong resources using native Kubernetes custom resources and controllers
**D)** When the team needs to query the Konnect Admin API directly

**Answer: C**
*Kong Operator is specifically designed for Kubernetes environments, enabling teams to manage Kong using Kubernetes-native patterns (CRDs, controllers, GitOps with kubectl).*

---

## Q9. Why is understanding CP/DP separation one of the most important architecture topics on the exam?

**A)** Because it determines the cost of a Konnect subscription
**B)** Because it directly affects decisions about data residency, fault tolerance, and deployment model selection
**C)** Because the Control Plane runs inside the Data Plane on certain deployments
**D)** Because decK only works when CP and DP are in the same region

**Answer: B**
*CP/DP separation explains where config lives, what happens during outages, how data residency works, and how to choose between hybrid and fully managed deployments.*

---

## Q10. Which Konnect components should you mention when asked for the core platform building blocks beyond the gateway itself?

**A)** Load balancer, firewall, CDN, and WAF
**B)** Dev Portal, Service Catalog, Analytics, AI Gateway, and Service Mesh
**C)** Jenkins, Prometheus, Grafana, and Elasticsearch
**D)** OpenAPI, AsyncAPI, GraphQL, and gRPC

**Answer: B**
*Beyond the Gateway, Konnect's platform includes: Dev Portal (developer self-service), Service Catalog (asset inventory), Analytics (observability), AI Gateway (AI traffic governance), and Service Mesh (east-west security).*

---

## Q11. A Data Plane node was recently updated with a new rate-limiting policy via the Control Plane. If the Control Plane becomes unreachable 5 minutes later, what policy does the Data Plane enforce?

**A)** No policy — all rate limiting is disabled during disconnection
**B)** The original policy before the update, restored from a backup
**C)** The last successfully synced policy (the new rate-limiting policy)
**D)** A default Kong-managed fallback policy

**Answer: C**
*The Data Plane caches the last known good configuration. Since the new policy was already synced before the disconnect, it remains active.*

---

## Q12. Which statement about Konnect organizations is TRUE?

**A)** Multiple organizations share the same set of roles and permissions
**B)** An organization can span multiple geos automatically
**C)** A Konnect organization is an isolated tenant — its resources, teams, and configurations are not shared with other organizations
**D)** Organizations must each have their own dedicated Konnect subscription

**Answer: C**
*Organizations provide hard isolation. Each org has its own users, teams, roles, gateways, portals, and configuration — completely separate from other orgs.*
