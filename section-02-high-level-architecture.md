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

---

## Q13. What is the primary responsibility of the Data Plane in a Konnect hybrid deployment?

**A)** Storing and versioning all gateway configuration
**B)** Serving as the runtime proxy that intercepts API requests, applies plugin policies, and forwards traffic to upstream services
**C)** Hosting the Konnect management UI and API
**D)** Managing user authentication for the organization

**Answer: B**
*The Data Plane is the runtime execution layer. It handles every live API request — applying auth, rate limiting, transformations, and routing at the point of traffic entry.*

---

## Q14. In the context of Konnect architecture, what is the significance of "configuration propagation"?

**A)** It refers to the process of deploying gateway nodes to new cloud regions
**B)** It is the mechanism by which the Control Plane pushes updated configuration (routes, plugins, services) down to connected Data Plane nodes
**C)** It describes how analytics telemetry flows from the Data Plane to the analytics system
**D)** It refers to the replication of gateway logs across multiple regions

**Answer: B**
*Configuration propagation is how changes made in the Control Plane (via UI, API, decK, or Terraform) reach the Data Plane nodes that enforce those changes in traffic.*

---

## Q15. Which scenario best illustrates the value of CP/DP separation for enterprise resilience?

**A)** A team uses Terraform to provision a new gateway instance
**B)** A Control Plane undergoes maintenance; during this time Data Planes continue serving live traffic without interruption using their cached configuration
**C)** A Data Plane is upgraded to a new Kong version without restarting
**D)** A new plugin is added to the gateway configuration using decK

**Answer: B**
*CP/DP separation means management-plane availability is decoupled from traffic-plane availability. Control Plane downtime doesn't mean customer-facing downtime.*

---

## Q16. How does decK differ from the Konnect API as an automation tool?

**A)** decK is a visual UI tool; the Konnect API is a command-line tool
**B)** decK operates declaratively against YAML state files, reconciling differences automatically; the Konnect API is an imperative REST interface used programmatically for custom integrations and automation scripts
**C)** decK only works for creating objects; the Konnect API only works for deleting objects
**D)** They are identical tools with different names

**Answer: B**
*decK = declarative, YAML-driven, Git-friendly (desired state). Konnect API = imperative, REST-based, flexible for custom tooling. Both automate configuration but with different paradigms.*

---

## Q17. Why would an organization choose Terraform over decK for managing Konnect configuration?

**A)** Terraform supports more Kong plugins than decK
**B)** When the team wants to manage Konnect resources as part of a broader infrastructure-as-code workflow that also provisions cloud resources (VMs, networks, databases) in the same plan
**C)** Terraform is faster than decK for small configuration changes
**D)** Terraform bypasses the Control Plane, making it more reliable

**Answer: B**
*Terraform is the right tool when API gateway config is part of a larger IaC picture — provisioning the gateway infrastructure AND configuring it in the same workflow.*

---

## Q18. A team needs to integrate Konnect configuration changes into a custom internal deployment pipeline. Which automation tool is best suited for this?

**A)** Kong Operator
**B)** decK YAML files only
**C)** Konnect APIs — which provide programmatic REST access to all Konnect resources for custom integration
**D)** Terraform provider for Konnect

**Answer: C**
*Konnect APIs are the most flexible option for custom integrations — any language, any pipeline tool, any workflow that can make HTTP requests can use them.*

---

## Q19. Which statement about hybrid mode is TRUE?

**A)** In hybrid mode, both the Control Plane and Data Plane are managed by Kong
**B)** In hybrid mode, the Control Plane is managed by Konnect (hosted by Kong) while the Data Plane is deployed and operated by the customer in their own infrastructure
**C)** Hybrid mode requires all Data Plane nodes to be in the same cloud as the Control Plane
**D)** Hybrid mode does not support plugin enforcement on the Data Plane

**Answer: B**
*Hybrid mode = Konnect-managed CP + customer-managed DP. The customer controls where and how the data plane runs; Kong manages the control plane.*

---

## Q20. What is the role of Kong Operator specifically in a Kubernetes environment?

**A)** It provides a web UI for managing Kong configuration from inside Kubernetes
**B)** It is a Kubernetes controller that watches custom resource definitions (CRDs) and automatically reconciles Kong Gateway and Konnect configuration to match the desired state declared in Kubernetes manifests
**C)** It replaces the Konnect Control Plane for Kubernetes-only deployments
**D)** It monitors Kong Data Plane health and restarts unhealthy pods

**Answer: B**
*Kong Operator brings the Kubernetes operator pattern to Kong: define KongService, KongRoute, KongPlugin resources as Kubernetes CRDs, and the operator ensures Kong's configuration matches.*

---

## Q21. In a Konnect deployment, which component should NEVER handle live API traffic?

**A)** Data Plane
**B)** Control Plane
**C)** Kong Gateway node
**D)** Sidecar proxy

**Answer: B**
*The Control Plane is exclusively a management plane. It stores and distributes configuration but should never be in the request path for live API traffic.*

---

## Q22. Which of the following best describes the relationship between a Control Plane and multiple Data Planes?

**A)** Each Data Plane has its own independent Control Plane
**B)** A single Control Plane can manage multiple Data Plane nodes across different environments, all receiving the same configuration
**C)** Data Planes are only connected to a Control Plane during initial setup
**D)** Multiple Control Planes are required to manage Data Planes in different cloud regions

**Answer: B**
*One CP → many DPs. This is the hub-and-spoke model that makes centralized management of geographically distributed gateway nodes possible.*

---

## Q23. What distinguishes a "serverless" gateway deployment from a hybrid deployment in Konnect?

**A)** Serverless gateways do not support plugin-based policy enforcement
**B)** Serverless gateways are consumption-based, with no fixed infrastructure to provision or manage; hybrid mode requires customer-managed persistent Data Plane nodes
**C)** Hybrid mode has no Control Plane; serverless mode always has a dedicated Control Plane
**D)** Serverless and hybrid are different names for the same deployment model

**Answer: B**
*Serverless = ephemeral, consumption-priced, zero-infrastructure-ops. Hybrid = persistent DP nodes managed by the customer. Different operational and cost models.*

---

## Q24. What happens when a new route is added to a Konnect Control Plane?

**A)** The new route is immediately active in the Dev Portal
**B)** The Control Plane stores the new route and propagates the updated configuration to all connected Data Plane nodes, which then start routing matching traffic
**C)** The Data Plane must be restarted to pick up the new route
**D)** The new route requires a decK sync before it takes effect

**Answer: B**
*Configuration changes made on the Control Plane are propagated to Data Planes automatically. There is no restart required — Kong's dynamic configuration system applies changes live.*

---

## Q25. Why is the Konnect architecture described as a "hub-and-spoke" model?

**A)** Because the Dev Portal is at the center with gateways as spokes
**B)** Because the Control Plane (hub) is the central configuration authority, and multiple Data Plane nodes (spokes) receive configuration from and report to it
**C)** Because analytics data flows from multiple sources to a single dashboard hub
**D)** Because multiple organizations connect to a single shared gateway hub

**Answer: B**
*Hub = Control Plane (single source of truth). Spokes = Data Plane nodes (distributed execution). Configuration flows out from the hub; telemetry flows back in.*

---

## Q26. A customer needs to run Data Plane nodes in an air-gapped environment with no internet access. Which hosting model supports this?

**A)** Dedicated Cloud Gateways
**B)** Serverless gateways
**C)** Hybrid mode — the customer deploys and manages Data Plane nodes in their own network, including air-gapped environments
**D)** Konnect does not support air-gapped environments

**Answer: C**
*Hybrid mode is the only model where the customer controls the Data Plane deployment location. Air-gapped environments require the customer to manage DP nodes on their own infrastructure.*

---

## Q27. Which Konnect component would you reference when a customer asks about managing multiple gateway environments (dev, staging, production) from a single interface?

**A)** Service Catalog
**B)** Gateway Manager — which provides centralized management of multiple Control Planes and their associated Data Planes across all environments
**C)** Analytics — which aggregates data from all environments
**D)** Dev Portal — which exposes APIs from all environments in one catalog

**Answer: B**
*Gateway Manager is the centralized management interface for all Control Planes and Data Planes. It's the single pane of glass for multi-environment gateway operations.*

---

## Q28. What is the consequence of having no automation tooling (no decK, no Terraform, no Konnect API) in a Konnect deployment?

**A)** The gateway will not function without at least one automation tool
**B)** All configuration must be made manually through the Konnect UI, which is not reproducible, not version-controlled, and not suitable for CI/CD pipelines
**C)** The Control Plane will automatically use its default configuration
**D)** Analytics data will not be collected without automation tooling

**Answer: B**
*Without automation tooling, teams lose GitOps, reproducibility, and pipeline integration. Manual UI changes are error-prone and cannot be reviewed, version-controlled, or automated.*

---

## Q29. Which statement about decK and its relationship to the Control Plane is TRUE?

**A)** decK communicates directly with Data Plane nodes to push configuration
**B)** decK reads desired state from YAML files and syncs it to the Konnect Control Plane via the Admin API, with the Control Plane then propagating changes to Data Planes
**C)** decK replaces the Control Plane for declarative deployments
**D)** decK only works in offline/air-gapped mode

**Answer: B**
*decK targets the Control Plane (via API), not the Data Plane directly. The flow is: decK YAML → Control Plane API → Control Plane → Data Plane.*

---

## Q30. An architect is comparing deployment models. The customer wants Kong to manage all gateway infrastructure but still wants a dedicated, isolated gateway (not shared with other customers). Which model fits?

**A)** Hybrid mode with customer-managed data planes
**B)** Serverless gateway with auto-scaling
**C)** Dedicated Cloud Gateways — fully managed by Kong with infrastructure dedicated to the customer, not shared
**D)** Self-hosted on-premises deployment

**Answer: C**
*Dedicated Cloud Gateways = fully managed by Kong + isolated to the customer (not multi-tenant shared infrastructure). Best of both worlds: no ops burden + dedicated resources.*

---

## Q31. When configuring a self-managed Data Plane to connect to Konnect in hybrid mode, which Kong configuration role must be set?

**A)** `role = control_plane`
**B)** `role = traditional`
**C)** `role = data_plane`
**D)** `role = hybrid`

**Answer: C**
*`role = data_plane` tells Kong to operate as a pure proxy with no local database — it receives configuration from the Control Plane via a secure mTLS channel instead.*

---

## Q32. What does the `cluster_control_plane` setting on a Data Plane node specify?

**A)** The IP address of the PostgreSQL database
**B)** The endpoint of the Konnect Control Plane that the DP connects to for configuration synchronization
**C)** The address of the Admin API listener
**D)** The upstream service host

**Answer: B**
*`cluster_control_plane` is the Konnect CP endpoint (e.g., `<cp-id>.cp0.konghq.com:443`). The DP dials this address over mTLS to receive its configuration stream.*

---

## Q33. What files must be present on a Data Plane node to establish the mTLS channel to the Konnect Control Plane?

**A)** `kong.conf` and a PostgreSQL client certificate
**B)** A cluster certificate (`cluster.crt`) and cluster private key (`cluster.key`) issued from the Konnect Control Plane
**C)** A self-signed TLS certificate for the proxy listener
**D)** The Admin API client certificate

**Answer: B**
*Konnect issues a unique cluster certificate pair per Control Plane. The DP uses this cert/key for mutual TLS when connecting to the CP — authenticating itself as a legitimate member of that CP's cluster.*

---

## Q34. Where do you obtain the cluster certificate and key needed to connect a new Data Plane to Konnect?

**A)** Generate them locally using `openssl` and upload to Konnect
**B)** Download them from the Konnect Gateway Manager UI when creating or viewing a Control Plane's DP connection instructions
**C)** They are automatically injected when the DP starts
**D)** They are stored in the Kong database

**Answer: B**
*The Konnect UI (Gateway Manager → Control Plane → Data Plane Nodes → Connect a Data Plane) provides ready-to-use configuration snippets including the cluster cert, key, and `cluster_control_plane` address.*

---

## Q35. Which Konnect Terraform resource provisions a new Control Plane?

**A)** `konnect_gateway_service`
**B)** `konnect_gateway_control_plane`
**C)** `konnect_control_plane_group`
**D)** `kong_gateway_cp`

**Answer: B**
*The `konnect_gateway_control_plane` Terraform resource (from the `kong/konnect` provider) creates and manages a Konnect Control Plane, including its name, description, and cluster type.*

---

## Q36. What is the primary advantage of using the Konnect Terraform provider alongside decK in a large enterprise deployment?

**A)** Terraform replaces decK entirely — no need for both
**B)** Terraform manages Konnect infrastructure (control planes, teams, roles, system accounts) while decK manages gateway entities (services, routes, plugins) — each tool operates at its appropriate layer
**C)** Terraform is faster than decK for syncing gateway configuration
**D)** Terraform directly pushes to the Data Plane, bypassing the Control Plane

**Answer: B**
*Terraform = infrastructure layer (CPs, identity, org settings). decK = configuration layer (gateway objects). Using both covers the full IaC stack without either tool overstepping its scope.*

---

## Q37. In a Konnect hybrid deployment, what is the `cluster_server_name` setting used for?

**A)** It sets the hostname the gateway advertises to clients
**B)** It provides the SNI value used by the Data Plane when establishing the TLS connection to the Control Plane endpoint
**C)** It names the cluster visible in Kong Manager
**D)** It configures the upstream service hostname

**Answer: B**
*`cluster_server_name` sets the TLS SNI for the CP connection, needed when the CP endpoint's certificate CN or SAN must be explicitly specified — common in Konnect where the CP hostname contains the CP ID.*
