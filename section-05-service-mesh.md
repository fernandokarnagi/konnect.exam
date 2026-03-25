# Section 5: Service Mesh

> **Exam Theme:** Zero-trust east-west traffic, mTLS, MeshTrafficPermission, and sidecar-based traffic routing.

---

## Q1. What traffic pattern is Service Mesh mainly responsible for protecting?

**A)** North-south traffic — requests coming from external clients into the cluster
**B)** East-west traffic — service-to-service communication inside the cluster or environment
**C)** Outbound traffic from services to external APIs
**D)** Traffic between the Control Plane and Data Plane

**Answer: B**
*Service Mesh focuses on east-west (lateral) traffic — the calls between microservices inside the same environment. API Gateway handles north-south (external-to-internal) traffic.*

---

## Q2. How does Kong Mesh support zero-trust communication between services?

**A)** By using JWT tokens shared between all services
**B)** By enabling mTLS so all service-to-service communication is encrypted and authenticated, with traffic denied by default until explicitly permitted
**C)** By placing all services behind a single gateway that enforces authentication
**D)** By using network-level IP whitelisting between service pods

**Answer: B**
*Zero-trust means no service is trusted by default. mTLS enforces mutual authentication (both sides verify identity), and MeshTrafficPermission provides explicit allow rules.*

---

## Q3. What changes when mTLS is enabled inside the mesh?

**A)** All HTTP traffic is upgraded to HTTP/2 automatically
**B)** All service-to-service communication becomes encrypted in transit, and each service must present a valid certificate to communicate with another
**C)** Services can no longer communicate until they register with the Control Plane
**D)** The Data Plane begins routing all traffic through the Control Plane for inspection

**Answer: B**
*mTLS encrypts traffic between services and enforces mutual certificate-based authentication — both the caller and the callee verify each other's identity.*

---

## Q4. Why is MeshTrafficPermission important in a default-deny model?

**A)** It sets the rate limits for east-west traffic
**B)** It is the mechanism that explicitly allows specific services to communicate when all traffic is denied by default under the zero-trust model
**C)** It encrypts traffic between service sidecars
**D)** It routes traffic to the correct upstream in a load-balanced pool

**Answer: B**
*In default-deny, no traffic flows unless explicitly allowed. MeshTrafficPermission is the policy resource that creates the explicit allow rules between services.*

---

## Q5. What should the `targetRef` and `from` clauses represent in an allow policy?

**A)** `targetRef` names the source service; `from` names the destination service
**B)** `targetRef` identifies the destination service being protected; `from` identifies which source services are allowed to send traffic to it
**C)** Both clauses identify services that are blocked from communicating
**D)** `targetRef` sets the traffic weight; `from` sets the retry policy

**Answer: B**
*`targetRef` = the service receiving traffic (the one being protected). `from` = the services that are permitted to send traffic to the target.*

---

## Q6. How do sidecar proxies communicate in a single-zone deployment according to the guide?

**A)** All sidecar-to-sidecar communication is routed through a central gateway node
**B)** Sidecars communicate indirectly through the Control Plane for every request
**C)** In a single-zone deployment, sidecars communicate directly over the flat network using the advertised IP addresses of peer services
**D)** Sidecars use DNS-based discovery and always resolve through an external load balancer

**Answer: C**
*In a single-zone mesh, the network is flat — sidecars can reach each other directly via IP. The control plane manages policy and certificates, but data-path communication is direct.*

---

## Q7. Why is Kong Mesh described as a centralized management plane for service meshes?

**A)** Because it runs inside every service pod alongside the application container
**B)** Because it provides a single control point for defining mTLS policies, traffic permissions, observability, and mesh configuration across all services and zones
**C)** Because it replaces Kubernetes' built-in networking with its own flat network
**D)** Because it proxies all service traffic through a single central node

**Answer: B**
*Kong Mesh centralizes mesh policy management — one place to configure mTLS, traffic permissions, observability, and routing rules — rather than configuring each service individually.*

---

## Q8. How would you explain the difference between API Gateway security and Service Mesh security?

**A)** API Gateway provides encryption; Service Mesh provides authentication
**B)** API Gateway secures north-south (external) traffic using plugins; Service Mesh secures east-west (internal service-to-service) traffic using mTLS and traffic permissions
**C)** They are functionally identical and can be used interchangeably
**D)** Service Mesh handles external traffic; API Gateway handles internal traffic

**Answer: B**
*Complementary, not competing: Gateway = perimeter security for external requests. Mesh = interior security for service-to-service communication. Both can coexist in a platform.*

---

## Q9. What is the exam-safe definition of east-west traffic?

**A)** Traffic flowing from the internet into the cluster through a public endpoint
**B)** Traffic flowing between microservices or applications within the same cluster or internal network
**C)** Traffic flowing from one Konnect organization to another
**D)** Traffic flowing from Data Plane to Control Plane for configuration synchronization

**Answer: B**
*East-west = lateral, internal traffic. Service A calls Service B inside the same environment. This is the traffic pattern Kong Mesh is designed to secure.*

---

## Q10. In a scenario asking for service-to-service encryption plus explicit allow rules, which section concepts should immediately come to mind?

**A)** API Gateway + rate limiting + Admin API security
**B)** Service Mesh + mTLS + MeshTrafficPermission
**C)** Dev Portal + application authentication + SSO
**D)** AI Gateway + Prompt Guard + Config Store

**Answer: B**
*Encryption of service-to-service traffic = mTLS. Explicit allow rules in a default-deny model = MeshTrafficPermission. Both are core Service Mesh concepts.*

---

## Q11. A security team wants all service-to-service traffic to be encrypted AND requires that only specific services can talk to each other. What is the minimum set of Kong Mesh features needed?

**A)** Load balancing and health checks
**B)** mTLS (for encryption and mutual authentication) and MeshTrafficPermission (for explicit allow policies)
**C)** Rate limiting and JWT authentication
**D)** Semantic caching and PII sanitization

**Answer: B**
*mTLS ensures encrypted, mutually authenticated traffic. MeshTrafficPermission provides the explicit allow rules. Together they implement zero-trust service-to-service security.*

---

## Q12. Which statement about Kong Mesh and sidecars is TRUE?

**A)** Sidecars are optional in Kong Mesh and only needed for mTLS
**B)** Each service in the mesh has a sidecar proxy injected alongside it; the sidecar intercepts all inbound and outbound traffic for the service
**C)** Sidecars communicate with each other exclusively through the Kong Mesh Control Plane
**D)** Sidecars replace the need for a Data Plane in a Konnect deployment

**Answer: B**
*The sidecar proxy pattern is fundamental to Kong Mesh. The sidecar is injected next to each service and intercepts traffic, enabling mTLS, observability, and policy enforcement transparently.*

---

## Q13. What is "mutual TLS" (mTLS) and how does it differ from standard TLS?

**A)** mTLS only encrypts traffic in one direction; standard TLS encrypts both directions
**B)** Standard TLS authenticates only the server to the client; mTLS requires both parties to present valid certificates, authenticating both the client and the server to each other
**C)** mTLS is faster than standard TLS because it skips certificate verification
**D)** They are identical; mTLS is just a marketing term for TLS 1.3

**Answer: B**
*Standard TLS: client verifies server. mTLS: both verify each other. This bilateral verification is essential for zero-trust — the server knows exactly which service is calling it.*

---

## Q14. What happens to service-to-service communication in a Kong Mesh deployment when mTLS is enabled and NO MeshTrafficPermission policies exist?

**A)** All traffic is allowed by default, with mTLS providing encryption only
**B)** All traffic is denied by default — no service can communicate with any other service until explicit allow policies are created
**C)** Traffic is allowed but unencrypted until the first MeshTrafficPermission is created
**D)** Services can communicate only if they are in the same namespace

**Answer: B**
*mTLS + default-deny is the zero-trust baseline. Without MeshTrafficPermission allow rules, the mesh blocks all service-to-service communication — forcing explicit authorization for every flow.*

---

## Q15. Which Kong Mesh component is responsible for distributing certificates to sidecar proxies for mTLS?

**A)** The Data Plane node
**B)** The Kong Mesh Control Plane — which acts as a certificate authority (CA) and issues certificates to each sidecar in the mesh
**C)** The Admin API
**D)** decK

**Answer: B**
*The Kong Mesh Control Plane manages the PKI (Public Key Infrastructure) for the mesh — generating, distributing, and rotating certificates that sidecars use for mTLS.*

---

## Q16. Why is sidecar-based architecture preferred over library-based service mesh approaches?

**A)** Sidecars use less memory than libraries
**B)** Sidecars are language-agnostic — they work regardless of what programming language the service uses, while library-based approaches require a mesh client library for each language
**C)** Sidecars are faster because they run in the same process as the application
**D)** Library-based approaches cannot enforce mTLS

**Answer: B**
*Sidecar = language-agnostic, no code changes needed. The proxy handles all mesh concerns outside the application. Libraries require importing and maintaining mesh code in every service.*

---

## Q17. What is the purpose of "zones" in Kong Mesh multi-zone deployments?

**A)** Zones represent different security tiers within a single cluster
**B)** Zones are separate deployment regions or clusters; Kong Mesh supports multi-zone deployments where services in different zones can communicate through zone-aware routing
**C)** Zones are groups of services that share the same MeshTrafficPermission policies
**D)** Zones define network partitions within a single Kubernetes namespace

**Answer: B**
*Multi-zone Kong Mesh deployments span multiple clusters or regions. Each zone has local data-plane nodes; the global control plane manages policies across all zones.*

---

## Q18. In the context of zero-trust networking, what does "default-deny" mean for a service mesh?

**A)** New services default to denying all incoming requests from external clients
**B)** All service-to-service communication is blocked unless explicitly permitted by policy — no implicit trust is granted based on network location alone
**C)** The mesh denies requests by default until the service passes a health check
**D)** Default-deny means new services cannot be added to the mesh without administrator approval

**Answer: B**
*Zero-trust default-deny: "never trust, always verify." No service can call another by default — every communication path must be explicitly authorized via MeshTrafficPermission.*

---

## Q19. How does Kong Mesh handle certificate rotation for mTLS?

**A)** Certificates are manually rotated by the platform team on a fixed schedule
**B)** The Kong Mesh Control Plane automatically rotates certificates before they expire, distributing new certificates to sidecars without service restart or traffic interruption
**C)** Certificate rotation requires downtime and a full mesh restart
**D)** Certificates do not expire in Kong Mesh

**Answer: B**
*Automatic certificate rotation is a key operational benefit of managed service mesh. The control plane handles the PKI lifecycle — sidecars get updated certs seamlessly.*

---

## Q20. What is the operational advantage of using Kong Mesh's centralized policy management over configuring TLS on each service individually?

**A)** Centralized configuration is faster at runtime
**B)** A single MeshTrafficPermission policy applied at the mesh level affects all relevant services consistently — avoiding the operational complexity and error risk of per-service TLS configuration
**C)** Centralized policies do not require the Control Plane to be available
**D)** Individual service TLS configuration is not possible in Kubernetes

**Answer: B**
*Configuring mTLS per-service means N services = N configurations to maintain. Mesh-level policies apply uniformly across all services from one place, reducing operational complexity.*

---

## Q21. Which statement about sidecar proxy injection in Kubernetes is TRUE?

**A)** Sidecars must be manually added to each Kubernetes pod definition
**B)** Kong Mesh can automatically inject sidecar proxies into pods at admission time using a Kubernetes mutating webhook, requiring no changes to the application's deployment manifests
**C)** Sidecar injection requires modifying the application's container image
**D)** Sidecars are only supported on Kubernetes 1.25 and above

**Answer: B**
*Automatic sidecar injection via mutating admission webhook is the standard approach. The mesh handles injection transparently — developers don't need to modify their deployment YAML.*

---

## Q22. A team has 50 microservices. Service A calls Service B, and Service C calls Service D. Using MeshTrafficPermission, what is the minimum number of policies needed to allow ONLY these two communication paths?

**A)** One policy allowing all traffic
**B)** Two policies — one allowing A→B and one allowing C→D
**C)** Four policies — one for each service in each direction
**D)** Fifty policies — one per service

**Answer: B**
*Each MeshTrafficPermission policy defines one allowed traffic path. Two paths require two policies. Everything else remains denied by default.*

---

## Q23. What type of traffic would NOT be managed by Kong Mesh?

**A)** REST API calls between two microservices in the same cluster
**B)** gRPC calls between services in the same Kubernetes namespace
**C)** External HTTPS requests from a mobile app to a public API endpoint
**D)** Database queries from a service to an internal database with a sidecar

**Answer: C**
*External requests from the internet to a public endpoint are north-south traffic — managed by the API Gateway, not the service mesh. Mesh = east-west internal traffic.*

---

## Q24. How does Kong Mesh provide observability for service-to-service traffic?

**A)** By logging all traffic to the Konnect Advanced Analytics platform
**B)** Through the sidecar proxies, which can emit metrics, traces, and logs for all intercepted traffic — visible in the Kong Mesh control plane and exportable to external observability tools
**C)** By requiring each service to implement OpenTelemetry instrumentation
**D)** Observability is only available for mTLS-encrypted traffic

**Answer: B**
*Sidecars observe all traffic they intercept. This gives Kong Mesh full visibility into service-to-service communication — latency, error rates, request counts — without application changes.*

---

## Q25. What is the relationship between Kong Mesh and Envoy?

**A)** Kong Mesh and Envoy are competing service mesh products
**B)** Kong Mesh uses Envoy as its underlying sidecar proxy — Envoy handles the data plane (traffic interception, mTLS, load balancing) while Kong Mesh provides the control plane
**C)** Kong Mesh replaces Envoy with a proprietary proxy for better performance
**D)** Envoy is only used in Kong Mesh's multi-zone deployments

**Answer: B**
*Kong Mesh (like Istio) is built on Envoy. Envoy is the battle-tested sidecar proxy that handles the data plane. Kong Mesh provides the control plane and management layer on top.*

---

## Q26. In a Kong Mesh deployment, which component is responsible for pushing policy updates (new MeshTrafficPermission rules) to the sidecar proxies?

**A)** The Konnect Control Plane via decK
**B)** The Kong Mesh Control Plane — which translates mesh policies into Envoy configuration and distributes them to the relevant sidecar proxies
**C)** The Admin API of each individual sidecar
**D)** Terraform via the Kong provider

**Answer: B**
*Kong Mesh Control Plane translates high-level mesh policies into low-level Envoy (xDS) configuration and pushes updates to sidecars dynamically — no restart required.*

---

## Q27. Why is it important to understand the difference between north-south and east-west traffic when designing a Konnect-based platform?

**A)** Because north-south traffic costs more to process than east-west traffic
**B)** Because each traffic pattern requires different security controls and different platform components — API Gateway for north-south perimeter security, Service Mesh for east-west lateral security
**C)** Because north-south and east-west traffic cannot be monitored simultaneously
**D)** Because the Data Plane only handles one type of traffic at a time

**Answer: B**
*Architectural clarity: each traffic pattern has the right tool. Using a gateway for internal service calls, or a mesh for external traffic, leads to incorrect architecture and security gaps.*

---

## Q28. What happens to the mesh's security posture if MeshTrafficPermission policies are too broadly defined (e.g., allowing all traffic from all services)?

**A)** The mesh becomes more efficient because fewer policy evaluations are needed
**B)** The zero-trust model is weakened — a compromised service can freely communicate with any other service, expanding the blast radius of a security incident
**C)** Broad policies improve performance because sidecars skip policy evaluation
**D)** Broad policies are required for multi-zone deployments

**Answer: B**
*Overly permissive policies defeat zero-trust. If any service can call any other, a compromised service has lateral movement freedom. Least-privilege traffic policies minimize blast radius.*

---

## Q29. A Kubernetes cluster runs Kong Mesh. A developer deploys a new service but forgets to create a MeshTrafficPermission policy for it. What is the result?

**A)** The new service can communicate with all existing services freely
**B)** The new service cannot send or receive mesh traffic — default-deny blocks all communication until explicit allow policies are created
**C)** The new service falls back to unencrypted HTTP until policies are added
**D)** Kong Mesh automatically generates a permissive policy for new services

**Answer: B**
*Default-deny is the zero-trust baseline. A new service without policies is completely isolated from all other services — intentional behavior that forces explicit authorization.*

---

## Q30. Which of the following BEST describes the combined value of mTLS + MeshTrafficPermission in a Kong Mesh deployment?

**A)** mTLS prevents DNS spoofing; MeshTrafficPermission prevents unauthorized API calls
**B)** mTLS provides encryption and mutual identity verification (who is calling?); MeshTrafficPermission provides authorization (is this caller allowed to talk to this service?) — together they implement complete zero-trust service-to-service security
**C)** They serve the same purpose and only one needs to be configured
**D)** mTLS applies to outbound traffic; MeshTrafficPermission applies to inbound traffic only

**Answer: B**
*The two-layer model: mTLS = authentication (identity). MeshTrafficPermission = authorization (permission). Authentication tells you who. Authorization tells you whether they're allowed. Both are required for zero-trust.*
