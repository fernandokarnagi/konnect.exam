# Section 9: Organizations & Teams

> **Exam Theme:** Organization boundaries, geos, roles, predefined teams, and the custom team creation sequence.

---

## Q1. What is a Konnect organization, and how is it different from a team?

**A)** An organization is a group of users; a team is a group of API gateways
**B)** An organization is the top-level isolated tenant boundary — it has its own resources, users, and configuration; a team is a group of users within an organization with assigned roles for a specific scope
**C)** Teams are the same as organizations but used for smaller groups
**D)** Organizations manage billing; teams manage technical resources

**Answer: B**
*Organization = tenant isolation boundary (everything inside is separate from other orgs). Team = a role-based grouping of users within the org, scoped to specific resources.*

---

## Q2. Why does the guide recommend starting with a single organization for most users?

**A)** Because Konnect only supports one organization per subscription
**B)** Because multiple organizations add unnecessary isolation overhead and management complexity for most use cases — a single org with teams and roles provides sufficient separation
**C)** Because geos are only available in a single-organization setup
**D)** Because predefined teams are only available in the first organization created

**Answer: B**
*Multiple orgs are for strong tenant isolation (e.g., separate business units that should never share resources). For most teams, roles and teams within a single org are sufficient.*

---

## Q3. What are geos, and why do they matter for compliance discussions?

**A)** Geos are different network zones within a single Konnect region
**B)** Geos are separate regional Konnect deployments (e.g., US, EU) where the Control Plane infrastructure resides; they matter for compliance because data residency requirements may mandate that certain data stays within specific geographic boundaries
**C)** Geos are load balancing zones for Data Plane traffic distribution
**D)** Geos are billing regions that affect the cost of Dedicated Cloud Gateways

**Answer: B**
*Geos = where the Control Plane infrastructure runs. Compliance requirements like GDPR may require EU data to stay in EU geos. Geo selection is an early architectural decision.*

---

## Q4. What is shared across geos, and what remains distinct between them?

**A)** Configuration is shared across geos; users are distinct per geo
**B)** Organization-level user management and identity (SSO, IdP config) is shared across geos; gateway configuration, control planes, and data planes are distinct per geo
**C)** Everything is fully shared across geos with no distinction
**D)** Geos share nothing — each geo is a completely independent Konnect instance

**Answer: B**
*User/identity management is org-wide. But the actual gateway resources (control planes, routes, plugins) are geo-specific, ensuring data residency.*

---

## Q5. Who can manage users, teams, and roles according to the study guide?

**A)** Any authenticated user within the organization
**B)** Only the API gateway administrator
**C)** Only users with Organization Admin access
**D)** Only Kong Support engineers

**Answer: C**
*Organization Admin is the privileged role required to manage the user and permissions model — adding users, creating teams, assigning roles.*

---

## Q6. Why can predefined teams such as "Org Admin" not be modified or deleted?

**A)** They are locked by the Control Plane to prevent configuration drift
**B)** They are built-in teams with fixed roles that represent core platform functions — modifying or deleting them could break organizational governance and access control
**C)** They can be modified but not deleted
**D)** They are locked until the organization has more than 10 users

**Answer: B**
*Predefined teams (Org Admin, API Admin, etc.) are immutable system roles that Konnect uses to maintain consistent governance. Allowing modification could inadvertently remove critical access.*

---

## Q7. What are roles used for inside Konnect?

**A)** Roles define the geographic region where a user's data is stored
**B)** Roles define what actions a user or team is permitted to perform on specific Konnect resources — enabling fine-grained access control across the platform
**C)** Roles are only used to restrict access to the Dev Portal
**D)** Roles determine the billing tier for each user in the organization

**Answer: B**
*Roles = permissions. They define who can read, write, or manage which resources — gateways, services, Dev Portal configurations, analytics data, etc.*

---

## Q8. What are the valid custom team creation sequences called out in the guide?

**A)** Create team → Add users → Request role assignment from support
**B)** Create team → Assign roles to the team → Add users to the team
   **OR** Create team → Add users → Assign roles to the team
**C)** Assign roles → Create team → Add users
**D)** Add users → Assign roles → Create team

**Answer: B**
*Both sequences are valid: you can assign roles before or after adding users. The team must be created first, and roles + users must both be added — the order between them is flexible.*

---

## Q9. How should you explain the difference between organization boundaries and role-based permissions?

**A)** Organization boundaries control what actions users can take; role-based permissions control which organization a user belongs to
**B)** Organization boundaries provide hard tenant isolation (resources in one org are completely invisible to another org); role-based permissions control what a user within an org can see and do
**C)** They are the same concept at different scales
**D)** Roles apply across organizations; organization boundaries apply only within a single geo

**Answer: B**
*Org boundary = hard wall between tenants. Roles = fine-grained permissions within the same tenant. Both are needed for complete access control.*

---

## Q10. Why are organizations, roles, and regional deployment choices often tested together in scenario questions?

**A)** Because they all affect API routing performance
**B)** Because real-world enterprise deployments require all three: the right tenant isolation (org), correct permissions model (roles/teams), and data residency compliance (geos)
**C)** Because they share the same configuration interface in the Konnect UI
**D)** Because changing one always requires changing the other two

**Answer: B**
*Enterprise scenarios combine: "How do we isolate tenants?" (org/geo) + "Who can do what?" (roles/teams). Exam questions often present a scenario requiring all three layers.*

---

## Q11. A new engineer joins the platform team and needs read-only access to gateway configuration but should not be able to modify anything. What is the correct approach?

**A)** Add the engineer to the Org Admin predefined team
**B)** Create a custom team, assign a read-only role scoped to gateway resources, and add the engineer to the team
**C)** Give the engineer direct access using the Admin API credentials
**D)** Add the engineer as an API Viewer in the Dev Portal

**Answer: B**
*Least-privilege access through role assignment on a custom team. Org Admin has too many permissions. API Viewer is a Dev Portal role, not a gateway role.*

---

## Q12. Which statement about geos in Konnect is TRUE?

**A)** You can move a Control Plane from one geo to another after creation
**B)** Geo selection is irreversible for a Control Plane — once created in a geo, the configuration and data remain in that region
**C)** All Konnect organizations default to the US geo regardless of selection
**D)** Geos only matter for Dedicated Cloud Gateways, not hybrid deployments

**Answer: B**
*Geo selection is a permanent decision for a Control Plane. This is why it matters for compliance — data created in the EU geo stays in the EU geo.*
