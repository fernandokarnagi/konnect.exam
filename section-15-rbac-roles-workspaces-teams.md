# Section 15: Dataplane Security — RBAC, Roles, Workspaces, and Teams

> **Exam Theme:** Securing Kong Gateway's administrative plane using RBAC, workspaces, roles, and teams; isolating configurations and delegating access.

---

## Q1. What does RBAC stand for in the context of Kong Gateway?

**A)** Route-Based Access Control
**B)** Role-Based Access Control
**C)** Resource-Bound Access Configuration
**D)** Restricted Backend Access Controller

**Answer: B**
*RBAC (Role-Based Access Control) is Kong's administrative security model that restricts access to the Admin API and Kong Manager based on roles assigned to tokens/users.*

---

## Q2. Which Kong Gateway tier includes RBAC and Workspaces?

**A)** Kong Gateway Open Source (OSS)
**B)** Kong Gateway Enterprise (EE)
**C)** Kong Gateway Lite
**D)** Kong Konnect Free tier

**Answer: B**
*RBAC, Workspaces, and Teams are Enterprise features. Kong OSS does not include multi-tenancy or fine-grained access controls.*

---

## Q3. What is a Kong Workspace?

**A)** A set of load balancer targets
**B)** An isolated logical partition of Kong configuration (services, routes, plugins, consumers)
**C)** A Kubernetes namespace mapping
**D)** A plugin execution environment

**Answer: B**
*Workspaces partition Kong entities into isolated groups. Each workspace has its own Services, Routes, Plugins, and Consumers, enabling multi-tenancy on a single Kong cluster.*

---

## Q4. Can a Service in Workspace A be accessed or referenced from Workspace B?

**A)** Yes, by default all workspaces share entities
**B)** Yes, but only with a cross-workspace plugin
**C)** No, entities in one workspace are isolated from other workspaces
**D)** Yes, if the same Consumer is in both workspaces

**Answer: C**
*Workspace isolation is strict. Entities belong to exactly one workspace and cannot be directly referenced from another workspace.*

---

## Q5. What is the `default` workspace in Kong?

**A)** A workspace reserved for health checks
**B)** The pre-existing workspace where entities are placed when no workspace is specified
**C)** A read-only workspace for auditing
**D)** The workspace used by the Admin API exclusively

**Answer: B**
*The `default` workspace always exists and is used when no workspace is explicitly specified. New installations start with all entities in `default`.*

---

## Q6. Which RBAC object grants permissions to access specific Admin API endpoints?

**A)** Consumer
**B)** Team
**C)** Role
**D)** Credential

**Answer: C**
*An RBAC Role is a collection of permissions (endpoint access + HTTP verb). Roles are assigned to RBAC Tokens (or users) to grant corresponding access.*

---

## Q7. Which built-in RBAC role has full administrative access to all Kong resources?

**A)** `read-only`
**B)** `admin`
**C)** `super-admin`
**D)** `workspace-admin`

**Answer: C**
*The `super-admin` role has unrestricted access across all workspaces and all endpoints. `admin` is typically scoped to specific workspaces.*

---

## Q8. What must be sent with Admin API requests when RBAC is enabled?

**A)** A valid JWT in the `Authorization` header
**B)** An `Kong-Admin-Token` header containing a valid RBAC token
**C)** An API key as a query parameter
**D)** A signed certificate in the TLS handshake

**Answer: B**
*When RBAC is enabled, Admin API requests must include the `Kong-Admin-Token` header with a valid RBAC token that has appropriate permissions.*

---

## Q9. How do you enable RBAC in Kong Gateway Enterprise?

**A)** Set `enforce_rbac = on` in `kong.conf`
**B)** POST to `/rbac/enable`
**C)** Install the `rbac` plugin globally
**D)** Set `rbac: true` in the decK state file

**Answer: A**
*RBAC is enabled via `enforce_rbac = on` (or `entity` / `both`) in `kong.conf` or the corresponding environment variable `KONG_ENFORCE_RBAC`.*

---

## Q10. Which RBAC permission scope limits a role to read-only access on the `/services` endpoint?

**A)** `endpoint: /services | action: read`
**B)** `endpoint: /services | action: GET`
**C)** An endpoint permission with actions `read` (GET allowed, write/delete denied)
**D)** All of the above represent equivalent configurations

**Answer: C**
*RBAC endpoint permissions consist of an endpoint pattern and a set of allowed actions (`create`, `read`, `update`, `delete`). Granting only `read` limits the token to GET requests.*

---

## Q11. What is the difference between `endpoint` RBAC and `entity` RBAC?

**A)** Endpoint RBAC controls access to Admin API URLs; entity RBAC controls access to specific Kong entities by ID
**B)** Endpoint RBAC applies to the proxy; entity RBAC applies to the Admin API
**C)** They are the same feature with different names
**D)** Endpoint RBAC is OSS; entity RBAC is Enterprise-only

**Answer: A**
*Endpoint RBAC grants/denies access to API endpoint paths. Entity RBAC adds finer-grained control over individual entities (e.g., a specific Service UUID).*

---

## Q12. An RBAC token is associated with a role that has `read` on `/services` but `*` (all) on `/routes`. The user tries to DELETE a Service. What happens?

**A)** The delete succeeds because the role has wildcard on routes
**B)** The delete is denied with 403 Forbidden
**C)** The delete succeeds because the user has read access
**D)** The delete is queued for admin approval

**Answer: B**
*The role only grants `read` on `/services`. A DELETE request requires `delete` permission. Kong returns `403 Forbidden`.*

---

## Q13. What is a Kong Team in the context of Kong Manager / Konnect?

**A)** A group of Kong nodes sharing the same configuration
**B)** A collection of users sharing the same roles and access permissions
**C)** A set of consumers grouped by usage tier
**D)** A deployment unit containing services and routes

**Answer: B**
*Teams are user groups. Roles are assigned at the team level, and all team members inherit those roles. This simplifies permission management for large organizations.*

---

## Q14. Which workspace-level role allows full administrative access within a single workspace but not across workspaces?

**A)** `super-admin`
**B)** `workspace-super-admin`
**C)** `workspace-admin`
**D)** `admin`

**Answer: C**
*`workspace-admin` is the built-in role granting full CRUD access within a specific workspace. It cannot modify other workspaces or global settings.*

---

## Q15. A team manages the "payments" workspace. They should not be able to see entities in the "inventory" workspace. How is this enforced?

**A)** Using firewall rules on the Admin API port
**B)** Assigning the team roles scoped only to the "payments" workspace
**C)** Encrypting the "inventory" workspace entities
**D)** Enabling workspace-level TLS certificates

**Answer: B**
*RBAC roles are workspace-scoped. Assigning a team roles only within the "payments" workspace means their tokens cannot access the "inventory" workspace's Admin API paths.*

---

## Q16. What happens to requests hitting the Kong Proxy when RBAC is enabled?

**A)** Proxy requests also require Kong-Admin-Token
**B)** RBAC only applies to the Admin API, not the proxy
**C)** All proxy requests are blocked until consumers are RBAC-authorized
**D)** RBAC replaces the authentication plugins on the proxy

**Answer: B**
*RBAC is an administrative control layer for the Admin API and Kong Manager. It has no effect on proxied API traffic — proxy security uses plugins (key-auth, JWT, OAuth2, etc.).*

---

## Q17. Which command in Kong Gateway Enterprise creates a new RBAC token?

**A)** `kong create-token`
**B)** `POST /rbac/users` followed by `POST /rbac/users/{user}/tokens`
**C)** `POST /consumers/{id}/tokens`
**D)** `kong generate-key`

**Answer: B**
*RBAC users are created at `/rbac/users`, then tokens are generated at `/rbac/users/{user}/tokens`. The token value is what goes in the `Kong-Admin-Token` header.*

---

## Q18. An organization wants team A and team B to share the same Kong cluster but with completely separate configurations. What is the recommended approach?

**A)** Run two separate Kong clusters
**B)** Use two separate Workspaces with team-scoped RBAC roles
**C)** Use Consumer Groups to separate traffic
**D)** Use separate plugins on each team's routes

**Answer: B**
*Workspaces provide configuration isolation on a shared cluster. Each team gets their own workspace with independent entities, and RBAC prevents cross-workspace access.*

---

## Q19. Which built-in role is recommended for operators who need to view configuration but must not make changes?

**A)** `super-admin`
**B)** `workspace-admin`
**C)** `read-only`
**D)** `viewer`

**Answer: C**
*The `read-only` built-in role grants GET (read) access to all endpoints in a workspace, suitable for auditors, monitoring agents, or junior operators.*

---

## Q20. Roles are assigned to RBAC tokens. What is the maximum number of roles assignable to a single token?

**A)** 1
**B)** 5
**C)** Unlimited
**D)** Limited by workspace count

**Answer: C**
*Multiple roles can be assigned to a single RBAC token. Permissions are additive — the token gains the union of all permissions from all assigned roles.*

---

## Q21. Which RBAC permission mode enforces access control on both Admin API endpoints AND individual entity objects?

**A)** `endpoint`
**B)** `entity`
**C)** `both`
**D)** `strict`

**Answer: C**
*`enforce_rbac = both` enforces endpoint-level AND entity-level permissions simultaneously, providing the most granular access control.*

---

## Q22. A new workspace is created in Kong Enterprise. Which roles are automatically available in it?

**A)** No roles; they must be created from scratch
**B)** The global built-in roles (`super-admin`, `admin`, `read-only`) are inherited
**C)** Only `read-only` is available in new workspaces
**D)** Roles from the `default` workspace are copied

**Answer: B**
*Kong Enterprise automatically creates built-in roles in every workspace (`workspace-super-admin`, `workspace-admin`, `workspace-read-only`) plus global roles are accessible.*

---

## Q23. What does the `neg` (negative) flag on an RBAC permission do?

**A)** Grants permissions to all endpoints except the specified one
**B)** Explicitly denies the specified permission even if other roles grant it
**C)** Negates the role's priority
**D)** Removes the permission from the role permanently

**Answer: B**
*A negative (deny) RBAC permission explicitly blocks access to an endpoint/action. Denies take precedence over allows when a token has conflicting permissions.*

---

## Q24. In Konnect, what is the equivalent concept to Kong Gateway Enterprise Workspaces?

**A)** Organizations
**B)** Control Planes
**C)** Geos
**D)** Teams

**Answer: B**
*In Kong Konnect, Control Planes serve as the isolation boundary (analogous to Workspaces). Each Control Plane has its own configuration, teams, and RBAC assignments.*

---

## Q25. Which configuration method should be used to set the initial `super-admin` token during Kong bootstrap?

**A)** `KONG_ADMIN_TOKEN` environment variable or `admin_gui_session_conf`
**B)** `KONG_PASSWORD` environment variable passed to `kong migrations bootstrap`
**C)** POST to `/rbac/users` before enabling RBAC
**D)** Editing `kong.conf` after deployment

**Answer: B**
*During `kong migrations bootstrap`, the `KONG_PASSWORD` environment variable sets the initial super-admin password. This must be done before enabling RBAC enforcement.*

---

## Q26. What is the purpose of an RBAC token TTL (time-to-live)?

**A)** It sets the caching duration for RBAC permissions
**B)** It automatically expires tokens after a specified period, enhancing security
**C)** It controls how long roles remain active
**D)** It limits the session duration in Kong Manager

**Answer: B**
*Token TTLs enforce automatic expiration. Expired tokens are rejected, reducing the risk of compromised long-lived credentials. TTL=0 means no expiration.*

---

## Q27. When a user is removed from a team in Kong Manager, what happens to their permissions?

**A)** Their permissions are retained until manually revoked
**B)** Their permissions derived from that team are immediately revoked
**C)** They retain permissions for 24 hours (grace period)
**D)** They are moved to the `read-only` role automatically

**Answer: B**
*Team membership drives permissions. Removing a user from a team immediately revokes the permissions granted by that team's roles.*

---

## Q28. Which API path prefix is used to access workspace-specific resources via the Admin API?

**A)** `/workspaces/{workspace-name}/api/`
**B)** `/{workspace-name}/`
**C)** `/admin/{workspace-name}/`
**D)** `/config/{workspace-name}/`

**Answer: B**
*Workspace resources are accessed by prefixing the workspace name: e.g., `GET /payments/services` lists services in the "payments" workspace.*

---

## Q29. A role has `endpoint: /services | actions: read,create`. Can a token with only this role delete a Service?

**A)** Yes, create implies delete
**B)** No, delete requires explicit `delete` action permission
**C)** Yes, if the token also has `workspace-admin`
**D)** Delete is always allowed for Service creators

**Answer: B**
*RBAC permissions are explicit. `delete` must be listed separately. Not having `delete` in the actions list means DELETE requests are denied.*

---

## Q30. What is the recommended way to audit who made a configuration change in Kong Enterprise?

**A)** Review nginx access logs
**B)** Use the RBAC audit log (`/audit/requests`)
**C)** Check the `X-Kong-Admin` header in proxy logs
**D)** Review the decK diff output

**Answer: B**
*Kong Enterprise records Admin API requests in an audit log accessible at `/audit/requests`. Each entry records the token used, the action taken, the entity affected, and the timestamp.*
