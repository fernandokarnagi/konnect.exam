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
**B)** By enabling mTLS so all service-to-service communication is encrypted and authenticated, with traffic denied by default unless explicitly permitted
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
