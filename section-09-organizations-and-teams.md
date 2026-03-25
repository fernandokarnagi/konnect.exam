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

---

## Q13. What is the purpose of "predefined teams" in Konnect?

**A)** They are placeholder teams created during organization setup that must be renamed before use
**B)** They are system-provided teams with fixed, immutable roles (e.g., Org Admin, Service Admin) that cover common platform functions — they cannot be modified or deleted
**C)** They are templates that organizations can copy to create custom teams
**D)** They are teams shared across all organizations in a Konnect instance

**Answer: B**
*Predefined teams are built-in, locked team definitions for key roles. They ensure consistent governance patterns without requiring organizations to configure critical roles from scratch.*

---

## Q14. A company operates in both the EU and the US and has separate engineering teams for each region. They need API data from EU operations to never leave Europe. How should they structure their Konnect deployment?

**A)** Use a single organization with two separate teams, one for EU and one for US
**B)** Create Control Planes in the EU geo for EU operations and US geo for US operations — the geo ensures that EU configuration and telemetry data remains within European infrastructure
**C)** Use two separate organizations, one for EU and one for US
**D)** Use a single geo and restrict access with role-based permissions

**Answer: B**
*Geo selection ensures data residency. EU Control Plane = EU data stays in EU. This satisfies data sovereignty requirements without requiring separate Konnect organizations.*

---

## Q15. What happens when an Organization Admin assigns a role to a team?

**A)** The role applies only to the admin who assigned it
**B)** All current and future members of the team inherit the permissions defined by that role for the specified resource scope
**C)** The role applies temporarily until the next team review
**D)** The role only applies to resources created after the assignment

**Answer: B**
*Role assignment to a team is inherited by all team members. Adding a new user to the team automatically grants them the team's roles — scalable permission management.*

---

## Q16. Why is it important to follow the principle of least privilege when assigning roles in Konnect?

**A)** Because Konnect performance degrades when users have too many permissions
**B)** Because granting excessive permissions increases the risk of accidental or malicious misconfiguration — users should only have the minimum access needed for their role
**C)** Because Konnect billing is based on the number of permissions granted
**D)** Because least privilege is only required for external API consumers, not internal users

**Answer: B**
*Least privilege limits the blast radius of errors and breaches. A developer who only needs to view analytics should not have the ability to delete gateway routes or modify security plugins.*

---

## Q17. Can a user belong to multiple teams in Konnect?

**A)** No — each user can only belong to one team at a time
**B)** Yes — a user can be a member of multiple teams, and their effective permissions are the union of all roles assigned to all teams they belong to
**C)** Yes, but only if all teams have identical role assignments
**D)** Only if the user has Organization Admin access

**Answer: B**
*Users can be on multiple teams. Their effective permissions are cumulative — a user on both "Gateway Viewer" and "Analytics Admin" teams has read access to gateways AND full analytics access.*

---

## Q18. What is an "Org Admin" in Konnect, and what are they responsible for?

**A)** An Org Admin is a Kong Support engineer with escalated access
**B)** An Org Admin is a user with the highest privilege level within an organization — responsible for user management, team creation, role assignment, SSO configuration, and organizational settings
**C)** An Org Admin manages only the gateway configuration for the organization
**D)** An Org Admin role can only be assigned by Kong via a support ticket

**Answer: B**
*Org Admin = the superuser within a Konnect organization. Full control over the user and permissions model, identity settings, and organizational configuration.*

---

## Q19. A Konnect organization has 5 teams, each managing different parts of the platform. Team A should not be able to see or modify Team B's gateway configuration. How is this enforced?

**A)** By creating separate organizations for each team
**B)** By using role-based access control — scoping each team's roles to only the specific gateway resources they own, preventing access to other teams' resources
**C)** By deploying each team's gateway in a different geo
**D)** By assigning each team a predefined team role that automatically restricts access

**Answer: B**
*RBAC with resource-scoped roles achieves team-level isolation within a single organization. Team A's roles only grant access to Team A's resources — no access to Team B's.*

---

## Q20. What is the consequence of having no roles defined for a user in Konnect?

**A)** The user defaults to Org Admin access
**B)** The user has no access to any Konnect resources — they can log in but cannot perform any management actions
**C)** The user gets read-only access to all resources by default
**D)** The user automatically joins the first team created in the organization

**Answer: B**
*No roles = no access. Konnect uses explicit role assignment — there is no default-grant model. A user without roles is effectively locked out of all management functions.*

---

## Q21. Which of the following BEST describes the relationship between a "role" and a "permission" in Konnect?

**A)** They are the same thing — role and permission are interchangeable terms
**B)** A role is a named collection of permissions — defining what actions (create, read, update, delete) can be performed on what resource types — and is assigned to teams or users
**C)** Permissions are assigned directly to users; roles are assigned only to teams
**D)** Roles define geographic scope; permissions define functional scope

**Answer: B**
*Role = bundled permissions. Instead of assigning individual permissions to each user, roles group related permissions (e.g., "Gateway Admin" = create/update/delete gateway resources) and are assigned as a unit.*

---

## Q22. An organization is being audited for access control compliance. The auditor asks: "Who has the ability to modify gateway routing rules?" Which Konnect feature provides this information?

**A)** Analytics dashboards
**B)** Service Catalog scorecards
**C)** Organization audit logs combined with role and team assignments — showing which teams have gateway modification roles and which users are in those teams
**D)** Dev Portal application registrations

**Answer: C**
*Access control audit = role assignments + audit logs. The auditor needs: "these roles grant gateway modification" (roles/teams) + "these users have those roles" (team memberships) + "here's the change history" (audit logs).*

---

## Q23. What is the recommended approach when an organization needs to provide temporary access to an external contractor?

**A)** Share the Org Admin credentials for the duration of the engagement
**B)** Create a contractor user account, assign them to a custom team with the minimum required roles for their specific tasks, and remove the account when the engagement ends
**C)** Add the contractor to an existing predefined team that closely matches their needs
**D)** Create a new organization specifically for contractors

**Answer: B**
*Temporary, least-privilege access: custom team with minimal roles → contractor account → time-boxed engagement → account removal. No shared credentials, no permanent access, no excessive permissions.*

---

## Q24. Why are geos relevant even within a single Konnect organization?

**A)** Geos determine which predefined teams are available
**B)** Within a single organization, different Control Planes can be deployed in different geos, allowing one org to operate globally while keeping regional data within the appropriate geographic boundaries
**C)** Geos within a single org always share configuration automatically
**D)** Geos only apply when using multiple organizations

**Answer: B**
*One org, multiple geos: a global company can have one Konnect org but deploy EU CPs in the EU geo and US CPs in the US geo — data residency per region, unified management.*

---

## Q25. What is the difference between "system" roles and custom roles in Konnect?

**A)** System roles can be deleted; custom roles are permanent
**B)** System roles are predefined by Konnect and cover standard access patterns; custom roles allow organizations to define granular permission sets tailored to their specific operational needs
**C)** Custom roles can only be assigned to predefined teams
**D)** There is no distinction — all roles in Konnect are treated the same

**Answer: B**
*System/predefined roles = standard coverage for common use cases. Custom roles = flexibility to define exactly the permissions needed for unique organizational structures.*

---

## Q26. A company's security policy requires that no single user can both create and approve changes to gateway configuration (separation of duties). How can Konnect's RBAC model support this?

**A)** This is not possible in Konnect — all gateway admins have the same permissions
**B)** By creating separate roles: one with "write" permissions for configuration changes and another with "review/approve" permissions — and ensuring no single user is assigned to both
**C)** By enabling multi-factor authentication for all gateway admin users
**D)** By using geo separation to enforce the separation of duties

**Answer: B**
*Role-based separation of duties: the "maker" role creates changes; the "checker" role approves. Assigning these to different teams ensures no single user has both capabilities.*

---

## Q27. What is the significance of "team membership" versus "direct role assignment" in Konnect access management?

**A)** Direct role assignment is preferred for individual contractors; team membership is preferred for permanent employees
**B)** Team membership scales better — adding a user to a team automatically grants all the team's roles; direct assignment requires individually managing each user's permissions, which becomes unmanageable at scale
**C)** They are identical in effect — choose either approach based on preference
**D)** Direct role assignment provides more granular control than team membership

**Answer: B**
*Teams scale. As the organization grows, "add to team" is one operation that grants all necessary roles. Direct assignment requires revisiting each user's individual permissions as roles evolve.*

---

## Q28. Which scenario justifies creating a second Konnect organization (rather than using teams within one organization)?

**A)** When a team has more than 20 members
**B)** When two distinct business units require completely separate environments with no shared visibility, configuration, or administration between them — true tenant isolation
**C)** When the organization uses more than 5 geos
**D)** When custom teams are insufficient to meet access control requirements

**Answer: B**
*Second org = hard wall. If Business Unit A and Business Unit B should NEVER see each other's data, configs, or users, separate organizations are the right answer. RBAC alone cannot achieve this level of isolation.*

---

## Q29. How does SSO integration at the organization level affect user management in Konnect?

**A)** SSO prevents users from being assigned to custom teams
**B)** SSO allows users to be provisioned and authenticated through a corporate identity provider — optionally with SCIM for automatic user provisioning and deprovisioning based on the IdP's group membership
**C)** SSO is only available for the Dev Portal, not for the Konnect management console
**D)** SSO replaces all role assignments, using IdP group mappings instead

**Answer: B**
*Org-level SSO: users log in with corporate identity. SCIM enables automatic provisioning (new employee → automatically added to Konnect) and deprovisioning (employee leaves → access removed).*

---

## Q30. A team lead needs to grant a junior engineer "view-only" access to analytics dashboards but no ability to modify gateway configuration. What is the most appropriate configuration?

**A)** Add the junior engineer to the Org Admin team with restricted mode
**B)** Create a custom team, assign an analytics read-only role scoped to the analytics resources, and add the junior engineer to that team — with no gateway roles assigned
**C)** Give the junior engineer direct access to the Analytics section by sharing the analytics API key
**D)** Add the junior engineer to the Gateway Admin team but verbally instruct them not to make changes

**Answer: B**
*Correct approach: custom team + analytics read-only role + no gateway roles. This implements least privilege precisely. Verbal restrictions and shared keys are not acceptable security controls.*
