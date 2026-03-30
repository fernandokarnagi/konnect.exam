# Section 9: Organizations & Teams

> **Exam Theme:** Organization boundaries, geos, roles, predefined teams, and the custom team creation sequence.

---

## Q1. What is a Konnect organization, and how is it different from a team?

**A)** Organizations manage billing; teams manage technical resources
**B)** Teams are the same as organizations but used for smaller user groups
**C)** An organization is a group of API gateways; a team is a group of users managing them
**D)** An organization is the top-level isolated tenant boundary with its own resources, users, and configuration; a team is a role-based grouping of users within an organization scoped to specific resources

**Answer: D**
*Organization = tenant isolation boundary (everything inside is separate from other orgs). Team = a role-based grouping of users within the org, scoped to specific resources.*

---

## Q2. Why does the guide recommend starting with a single organization for most users?

**A)** Because predefined teams are only available in the first organization created
**B)** Because geos are only available in a single-organization setup
**C)** Because multiple organizations add unnecessary isolation overhead and management complexity — a single org with teams and roles provides sufficient separation for most use cases
**D)** Because Konnect only supports one organization per subscription tier

**Answer: C**
*Multiple orgs are for strong tenant isolation (e.g., separate business units that should never share resources). For most teams, roles and teams within a single org are sufficient.*

---

## Q3. What are geos, and why do they matter for compliance discussions?

**A)** Geos are load balancing zones for Data Plane traffic distribution within a region
**B)** Geos are separate regional Konnect deployments (e.g., US, EU) where the Control Plane infrastructure resides; they matter because data residency requirements may mandate that certain data stays within specific geographic boundaries
**C)** Geos are billing regions that affect the cost of Dedicated Cloud Gateways
**D)** Geos are different network zones within a single Konnect deployment region

**Answer: B**
*Geos = where the Control Plane infrastructure runs. Compliance requirements like GDPR may require EU data to stay in EU geos. Geo selection is an early architectural decision.*

---

## Q4. What is shared across geos, and what remains distinct between them?

**Select TWO correct statements.**

**A)** Gateway configuration — services, routes, and plugins — is shared across all geos automatically
**B)** Organization-level user management and SSO/IdP configuration is shared across geos
**C)** Analytics data aggregates across geos into a single unified dataset
**D)** Gateway configuration, Control Planes, and Data Planes are distinct per geo — not shared
**E)** Geos share everything except billing settings

**Answer: B, D**
*User/identity management is org-wide. But the actual gateway resources (control planes, routes, plugins) are geo-specific, ensuring data residency.*

---

## Q5. Who can manage users, teams, and roles according to the study guide?

**A)** Only Kong Support engineers via a support ticket escalation
**B)** Any authenticated user within the organization who has gateway access
**C)** Only users with Organization Admin access
**D)** Only users who created the organization initially

**Answer: C**
*Organization Admin is the privileged role required to manage the user and permissions model — adding users, creating teams, assigning roles.*

---

## Q6. Why can predefined teams such as "Org Admin" not be modified or deleted?

**A)** They are locked until the organization has more than 10 users
**B)** They can be modified but not deleted — deletion is restricted to prevent orphaned permissions
**C)** They are locked by the Control Plane to prevent configuration drift across geos
**D)** They are built-in teams with fixed roles representing core platform functions — modifying or deleting them could break organizational governance and access control

**Answer: D**
*Predefined teams (Org Admin, API Admin, etc.) are immutable system roles that Konnect uses to maintain consistent governance. Allowing modification could inadvertently remove critical access.*

---

## Q7. What are roles used for inside Konnect?

**A)** Roles determine the billing tier for each user in the organization
**B)** Roles define what actions a user or team is permitted to perform on specific Konnect resources — enabling fine-grained access control across the platform
**C)** Roles define the geographic region where a user's configuration data is stored
**D)** Roles are only used to restrict access to the Dev Portal for external consumers

**Answer: B**
*Roles = permissions. They define who can read, write, or manage which resources — gateways, services, Dev Portal configurations, analytics data, etc.*

---

## Q8. What are the valid custom team creation sequences?

**A)** Assign roles → Create team → Add users
**B)** Add users → Assign roles → Create team
**C)** Create team → Assign roles to the team → Add users to the team; OR Create team → Add users → Assign roles to the team
**D)** Create team → Request role assignment from Org Admin → Add users after roles are confirmed

**Answer: C**
*Both sequences are valid: you can assign roles before or after adding users. The team must be created first, and both roles and users must be added — the order between them is flexible.*

---

## Q9. How should you explain the difference between organization boundaries and role-based permissions?

**A)** They are the same concept at different scales — organizations for enterprise, roles for teams
**B)** Roles apply across organizations; organization boundaries apply only within a single geo
**C)** Organization boundaries control what actions users can take; role-based permissions control which organization a user belongs to
**D)** Organization boundaries provide hard tenant isolation — resources in one org are completely invisible to another; role-based permissions control what a user within an org can see and do

**Answer: D**
*Org boundary = hard wall between tenants. Roles = fine-grained permissions within the same tenant. Both are needed for complete access control.*

---

## Q10. Why are organizations, roles, and regional deployment choices often tested together in scenario questions?

**A)** Because they share the same configuration interface in the Konnect UI
**B)** Because changing one always requires modifying the other two simultaneously
**C)** Because they all affect API routing performance under different load conditions
**D)** Because real-world enterprise deployments require all three: correct tenant isolation (org), permissions model (roles/teams), and data residency compliance (geos)

**Answer: D**
*Enterprise scenarios combine: "How do we isolate tenants?" (org/geo) + "Who can do what?" (roles/teams). Exam questions often present a scenario requiring all three layers.*

---

## Q11. A new engineer needs read-only access to gateway configuration but must not be able to modify anything. What is the correct approach?

**A)** Give the engineer direct access using the Admin API credentials with a read-only flag
**B)** Add the engineer as an API Viewer in the Dev Portal
**C)** Add the engineer to the Org Admin predefined team
**D)** Create a custom team, assign a read-only role scoped to gateway resources, and add the engineer to the team

**Answer: D**
*Least-privilege access through role assignment on a custom team. Org Admin has too many permissions. API Viewer is a Dev Portal role, not a gateway configuration role.*

---

## Q12. Which statement about geos in Konnect is TRUE?

**A)** Geos only matter for Dedicated Cloud Gateways, not hybrid deployments
**B)** All Konnect organizations default to the US geo regardless of the selection made during setup
**C)** Geo selection is irreversible for a Control Plane — once created in a geo, the configuration and data remain in that region
**D)** You can migrate a Control Plane from one geo to another after creation using the Admin API

**Answer: C**
*Geo selection is a permanent decision for a Control Plane. This is why it matters for compliance — data created in the EU geo stays in the EU geo.*

---

## Q13. What is the purpose of "predefined teams" in Konnect?

**A)** They are teams shared across all organizations in the same Konnect instance
**B)** They are placeholder teams created during organization setup that must be renamed before use
**C)** They are templates that organizations copy to create custom teams
**D)** They are system-provided teams with fixed, immutable roles (e.g., Org Admin, Service Admin) covering common platform functions — they cannot be modified or deleted

**Answer: D**
*Predefined teams are built-in, locked team definitions for key roles. They ensure consistent governance patterns without requiring organizations to configure critical roles from scratch.*

---

## Q14. A company operates in both the EU and the US and needs API data from EU operations to never leave Europe. How should they structure their Konnect deployment?

**A)** Use a single geo and restrict access with role-based permissions
**B)** Use two separate organizations, one for EU and one for US operations
**C)** Create Control Planes in the EU geo for EU operations and US geo for US operations — the geo ensures EU configuration and telemetry data remains within European infrastructure
**D)** Use a single organization with two separate teams, one for EU and one for US

**Answer: C**
*Geo selection ensures data residency. EU Control Plane = EU data stays in EU. This satisfies data sovereignty without requiring separate Konnect organizations.*

---

## Q15. What happens when an Organization Admin assigns a role to a team?

**A)** The role only applies to resources created after the assignment date
**B)** The role applies temporarily until the next annual team review
**C)** All current and future members of the team inherit the permissions defined by that role for the specified resource scope
**D)** The role applies only to the admin who performed the assignment

**Answer: C**
*Role assignment to a team is inherited by all team members. Adding a new user automatically grants them the team's roles — scalable permission management.*

---

## Q16. Why is it important to follow the principle of least privilege when assigning roles in Konnect?

**A)** Because Konnect billing is based on the number of permissions granted per user
**B)** Because Konnect performance degrades when users have too many simultaneous permissions
**C)** Because least privilege is only required for external API consumers, not internal users
**D)** Because granting excessive permissions increases the risk of accidental or malicious misconfiguration — users should have only the minimum access needed for their role

**Answer: D**
*Least privilege limits the blast radius of errors and breaches. A developer who only needs to view analytics should not have the ability to delete gateway routes or modify security plugins.*

---

## Q17. Can a user belong to multiple teams in Konnect?

**A)** Only if the user has Organization Admin access to self-assign team memberships
**B)** Yes — a user can be a member of multiple teams, and their effective permissions are the union of all roles assigned to all teams they belong to
**C)** Yes, but only if all teams have identical role assignments to prevent permission escalation
**D)** No — each user can only belong to one team at a time to enforce clean role separation

**Answer: B**
*Users can be on multiple teams. Their effective permissions are cumulative — a user on both "Gateway Viewer" and "Analytics Admin" teams has read access to gateways AND full analytics access.*

---

## Q18. What is an "Org Admin" in Konnect, and what are they responsible for?

**A)** An Org Admin manages only the gateway configuration for the entire organization
**B)** An Org Admin is a Kong Support engineer with escalated access to the organization
**C)** An Org Admin role can only be assigned by Kong via a support ticket
**D)** An Org Admin is a user with the highest privilege level within an organization — responsible for user management, team creation, role assignment, SSO configuration, and organizational settings

**Answer: D**
*Org Admin = the superuser within a Konnect organization. Full control over the user and permissions model, identity settings, and organizational configuration.*

---

## Q19. Team A should not be able to see or modify Team B's gateway configuration. How is this enforced within a single organization?

**A)** By assigning each team a predefined team role that automatically restricts cross-team access
**B)** By using role-based access control — scoping each team's roles to only their specific gateway resources, preventing access to other teams' resources
**C)** By deploying each team's gateway in a different geo
**D)** By creating separate organizations for teams A and B

**Answer: B**
*RBAC with resource-scoped roles achieves team-level isolation within a single organization. Team A's roles only grant access to Team A's resources.*

---

## Q20. What is the consequence of having no roles defined for a user in Konnect?

**A)** The user automatically joins the first team created in the organization
**B)** The user gets read-only access to all resources by default as a safety baseline
**C)** The user defaults to Org Admin access until explicit roles are assigned
**D)** The user has no access to any Konnect resources — they can log in but cannot perform any management actions

**Answer: D**
*No roles = no access. Konnect uses explicit role assignment — there is no default-grant model. A user without roles is effectively locked out of all management functions.*

---

## Q21. Which statement BEST describes the relationship between a "role" and a "permission" in Konnect?

**A)** Roles define geographic scope; permissions define functional scope
**B)** Permissions are assigned directly to users; roles are assigned only to teams
**C)** They are identical — role and permission are interchangeable terms in Konnect documentation
**D)** A role is a named collection of permissions — defining what actions can be performed on what resource types — and is assigned to teams or users as a unit

**Answer: D**
*Role = bundled permissions. Instead of assigning individual permissions to each user, roles group related permissions and are assigned as a unit.*

---

## Q22. An auditor asks: "Who has the ability to modify gateway routing rules?" Which Konnect feature provides this information?

**A)** Service Catalog scorecards filtered by gateway compliance criteria
**B)** Dev Portal application registration logs
**C)** Analytics dashboards filtered by gateway modification events
**D)** Organization audit logs combined with role and team assignments — showing which teams have gateway modification roles and which users are in those teams

**Answer: D**
*Access control audit = role assignments + audit logs. The auditor needs: "these roles grant gateway modification" + "these users have those roles" + "here's the change history."*

---

## Q23. What is the recommended approach when providing temporary access to an external contractor?

**A)** Create a new organization specifically for contractors to isolate them completely
**B)** Add the contractor to an existing predefined team that closely matches their needs
**C)** Share the Org Admin credentials for the duration of the engagement
**D)** Create a contractor user account, assign them to a custom team with the minimum required roles, and remove the account when the engagement ends

**Answer: D**
*Temporary, least-privilege access: custom team with minimal roles → contractor account → time-boxed engagement → account removal. No shared credentials, no permanent access.*

---

## Q24. Why are geos relevant even within a single Konnect organization?

**Select TWO correct statements.**

**A)** Geos within a single org always share Control Plane configuration automatically
**B)** Within a single organization, different Control Planes can be deployed in different geos
**C)** Geos determine which predefined teams are available to organization members
**D)** Different regional geos in one org allow regional data to stay within the appropriate geographic boundaries
**E)** Geos only apply when using multiple organizations, not within a single one

**Answer: B, D**
*One org, multiple geos: a global company can have one Konnect org but deploy EU CPs in the EU geo and US CPs in the US geo — data residency per region, unified management.*

---

## Q25. What is the difference between "system" roles and custom roles in Konnect?

**A)** Custom roles can only be assigned to predefined teams, not to custom teams
**B)** There is no distinction — all roles in Konnect are treated identically
**C)** System roles can be deleted; custom roles are permanent once created
**D)** System roles are predefined by Konnect and cover standard access patterns; custom roles allow organizations to define granular permission sets tailored to specific operational needs

**Answer: D**
*System/predefined roles = standard coverage for common use cases. Custom roles = flexibility to define exactly the permissions needed for unique organizational structures.*

---

## Q26. A security policy requires that no single user can both create and approve changes to gateway configuration. How can Konnect's RBAC model support this?

**A)** By using geo separation to enforce the approval boundary between maker and checker
**B)** By enabling multi-factor authentication for all gateway admin users
**C)** This is not possible in Konnect — all gateway admins receive the same permission set
**D)** By creating separate roles — one with "write" permissions for configuration changes and another with "review/approve" permissions — and ensuring no single user is assigned to both

**Answer: D**
*Role-based separation of duties: the "maker" role creates changes; the "checker" role approves. Assigning these to different teams ensures no single user has both capabilities.*

---

## Q27. What is the advantage of "team membership" over "direct role assignment" for access management at scale?

**A)** Direct role assignment provides more granular control than team membership
**B)** They are identical in effect — choose either based on organizational preference
**C)** Team membership scales better — adding a user to a team automatically grants all the team's roles; direct assignment requires managing each user's permissions individually, which becomes unmanageable at scale
**D)** Direct role assignment is preferred for permanent employees; team membership for contractors

**Answer: C**
*Teams scale. As the organization grows, "add to team" is one operation that grants all necessary roles. Direct assignment requires revisiting each user's permissions as roles evolve.*

---

## Q28. Which scenario justifies creating a second Konnect organization?

**A)** When the organization uses more than 5 geos simultaneously
**B)** When custom teams are insufficient to meet access control requirements
**C)** When two distinct business units require completely separate environments with no shared visibility, configuration, or administration between them — true tenant isolation
**D)** When a team has more than 20 members and roles become difficult to manage

**Answer: C**
*Second org = hard wall. If Business Unit A and Business Unit B should NEVER see each other's data, configs, or users, separate organizations are the right answer.*

---

## Q29. How does SSO integration at the organization level affect user management in Konnect?

**A)** SSO replaces all role assignments, using IdP group mappings instead of Konnect roles
**B)** SSO is only available for the Dev Portal, not for the Konnect management console
**C)** SSO prevents users from being assigned to custom teams
**D)** SSO allows users to authenticate through a corporate IdP — optionally with SCIM for automatic user provisioning and deprovisioning based on the IdP's group membership

**Answer: D**
*Org-level SSO: users log in with corporate identity. SCIM enables automatic provisioning (new employee → added to Konnect) and deprovisioning (employee leaves → access removed).*

---

## Q30. A junior engineer needs view-only access to analytics dashboards but no ability to modify gateway configuration. What is the most appropriate configuration?

**A)** Add the junior engineer to the Org Admin team with a read-only mode flag
**B)** Give the junior engineer direct access to the Analytics section by sharing an analytics API key
**C)** Add the junior engineer to the Gateway Admin team and verbally instruct them not to make changes
**D)** Create a custom team, assign an analytics read-only role scoped to analytics resources, and add the junior engineer — with no gateway roles assigned

**Answer: D**
*Correct approach: custom team + analytics read-only role + no gateway roles. Implements least privilege precisely. Verbal restrictions and shared keys are not acceptable security controls.*
