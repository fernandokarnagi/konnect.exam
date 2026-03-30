# Section 11: APIOps & GitOps

> **Exam Theme:** Applying DevOps principles to the API lifecycle — design-first with OpenAPI, automated pipelines using inso + decK + Terraform, multi-environment promotion, and the Kong Ingress Controller.

---

## Q1. Which foundational principle distinguishes APIOps from traditional API deployment approaches?

**A)** APIOps eliminates the need for testing by using AI to validate gateway deployments
**B)** APIs are deployed manually by a dedicated platform operations team on a schedule
**C)** APIOps applies only to internal APIs and not to public-facing partner APIs
**D)** API configuration, design, and deployment are treated as code — stored in version control, reviewed via pull requests, and deployed through automated pipelines

**Answer: D**
*APIOps = DevOps for APIs. Every API artifact (OAS spec, gateway config, plugin settings) is a code artifact — reviewed, versioned, tested, and deployed automatically.*

---

## Q2. What is the "design-first" (or "spec-first") approach in APIOps?

**A)** Designing the database schema before writing any API or backend code
**B)** Writing unit tests before writing the API implementation (TDD applied to APIs)
**C)** Creating the OpenAPI Specification before writing any backend code — using the spec as the contract both producer and consumer agree on upfront
**D)** Deploying code first and extracting the API spec from the running service post-deployment

**Answer: C**
*Design-first: the OAS is the source of truth. Frontend and backend teams work in parallel against the spec. Gateway configuration is generated from it — no ambiguity about the contract.*

---

## Q3. In an APIOps workflow, what artifact does `inso generate config` produce from an OpenAPI Specification?

**A)** Terraform provider configurations for provisioning Konnect Control Planes
**B)** Kubernetes namespace and RBAC definitions for the Kong Ingress Controller
**C)** A decK state file (kong.yaml) containing Kong services, routes, and plugins derived from the OAS
**D)** PostgreSQL database schemas for storing API configuration

**Answer: C**
*The OAS is the upstream source. `inso generate config` produces a `kong.yaml` decK state file. The pipeline then validates, diffs, and syncs it to the Control Plane.*

---

## Q4. What is the purpose of `x-kong-plugin-*` extensions in an OpenAPI Specification?

**A)** They configure Kong Manager UI labels and descriptions for each API endpoint
**B)** They embed Kong plugin configurations directly in the OAS so `inso generate config` includes those plugins in the generated decK state file
**C)** They document which Kong version is required to run the API at runtime
**D)** They mark specific endpoints as deprecated for display in the Dev Portal

**Answer: B**
*`x-kong-*` extensions bring gateway configuration into the spec. `x-kong-plugin-rate-limiting` on a path operation means the generated gateway config includes that plugin.*

---

## Q5. Which `inso` CLI command runs unit tests from an Insomnia collection against a live API endpoint?

**A)** `inso export spec`
**B)** `inso lint spec`
**C)** `inso run test`
**D)** `inso generate config`

**Answer: C**
*`inso run test` executes JavaScript-based test assertions (status codes, response body checks) against a running API. In a CI/CD pipeline this validates that the deployed API behaves as expected.*

---

## Q6. In a CI/CD pipeline using decK, what does `deck validate` check?

**A)** Whether all upstream services configured in the state file are healthy and reachable
**B)** Whether any secrets or credentials are exposed in plaintext in the state file
**C)** Whether the Konnect Control Plane is reachable and authenticated
**D)** The structural correctness of a decK state file against the Kong schema — without connecting to Kong

**Answer: D**
*`deck validate` is an offline check — it catches schema errors (wrong field types, missing required fields) before the pipeline connects to Kong, saving time and preventing bad syncs.*

---

## Q7. What is the purpose of `deck diff` in a CI/CD pipeline?

**A)** It exports the current running Kong configuration as a YAML file to the local filesystem
**B)** It deletes all entities from Kong that are absent from the state file immediately
**C)** It compares two local state files to identify configuration inconsistencies between environments
**D)** It shows a human-readable diff of what `deck sync` would change — used as a review/approval gate before applying changes to production

**Answer: D**
*`deck diff` is the "plan" step (analogous to `terraform plan`). Showing the output in a PR or CI run lets reviewers see exactly what will change before approving the deployment.*

---

## Q8. Which decK command should NEVER run in a production pipeline without a prior `deck diff` review?

**A)** `deck ping`
**B)** `deck dump`
**C)** `deck validate`
**D)** `deck sync`

**Answer: D**
*`deck sync` applies changes — including deletions. Running it without reviewing the diff first risks accidentally removing services, routes, or plugins in production.*

---

## Q9. How does `deck dump` support the "shift-left" APIOps philosophy?

**A)** It compresses state files to reduce Git repository size
**B)** It generates OpenAPI specs from the running Kong configuration for documentation
**C)** It immediately deploys all pending configuration changes to the production gateway
**D)** It exports the current running Kong configuration into a state file — useful for bootstrapping GitOps from an existing manual deployment by committing the output to Git as the initial source of truth

**Answer: D**
*`deck dump` is often the starting point when migrating a manual deployment to GitOps: export → commit → that file becomes the source of truth going forward.*

---

## Q10. How should API keys and secrets be handled in a decK state file stored in a Git repository?

**A)** Store them in plaintext — Git repository access controls are sufficient protection
**B)** Encode them in Base64 before committing to obscure the values
**C)** Use decK's environment variable substitution (`$ENV_VAR`) so the Git file contains references only — actual secrets are injected at runtime from a CI/CD secret store
**D)** Store them in a separate private repository with restricted access and no CI/CD integration

**Answer: C**
*decK supports `$ENV_VAR` placeholders. The state file in Git references variable names. Actual values are injected at runtime from the CI/CD platform's secret vault.*

---

## Q11. In a multi-environment pipeline, what decK mechanism prevents a prod sync from accidentally modifying dev-tagged entities on the same Control Plane?

**A)** Running `deck validate` before every `deck sync` prevents cross-environment mutations
**B)** Using separate decK binaries compiled for each environment
**C)** `--select-tag env:prod` — scoping the sync so only entities tagged `env:prod` are managed by this pipeline run; entities with other tags are invisible to the sync
**D)** Deploying each environment in a separate geo to enforce physical separation

**Answer: C**
*`--select-tag` tells decK to treat only entities with the specified tag as "owned" by this pipeline run. Entities with other tags are invisible — no accidental cross-environment mutations.*

---

## Q12. An organization promotes API changes from staging to production. What is the safest pipeline step sequence?

**A)** `deck sync` → `inso run test` → rollback if tests fail → mark deploy complete
**B)** `deck dump` → `deck validate` → `deck sync` → promote to production
**C)** `inso lint spec` → `inso generate config` → `deck validate` → `deck diff` (review + approval) → `deck sync`
**D)** `deck sync` → `deck diff` → `inso lint spec` → approval gate

**Answer: C**
*Quality gates in order: lint spec → generate config → validate schema → diff review → human approval → sync. Each gate catches a different class of error before it reaches production.*

---

## Q13. What does the `--workspace` flag in decK enable for organizations with multiple Kong workspaces?

**A)** It maps workspace names to Kubernetes namespaces for KIC deployments
**B)** It creates a new workspace on the Control Plane on every sync operation
**C)** It scopes all decK operations (dump, diff, sync) to a specific named workspace — enabling per-workspace pipelines without cross-contamination between teams
**D)** It sets the workspace displayed in the Konnect UI for the current user session

**Answer: C**
*`--workspace payments` means decK only reads from and writes to the "payments" workspace. Teams own their workspace pipeline independently from other teams.*

---

## Q14. Which Konnect feature enables teams to use Kubernetes-native YAML (not decK YAML) to manage Kong configuration?

**A)** Helm charts for decK state file deployment
**B)** The Konnect Terraform provider with Kubernetes backend state
**C)** `deck convert` to transform decK YAML into Kubernetes manifest format
**D)** The Kong Ingress Controller (KIC) with CRDs such as `KongPlugin`, `KongConsumer`, and `KongIngress`

**Answer: D**
*KIC translates Kubernetes resources into Kong configuration. Teams write `KongPlugin` CRDs and standard `Ingress` objects — KIC reconciles these to the Kong Gateway automatically.*

---

## Q15. How does KIC handle a configuration change made directly via the Kong Admin API (bypassing Kubernetes)?

**A)** KIC merges Admin API changes with the Kubernetes configuration on the next reconcile cycle
**B)** KIC disables the Admin API to prevent any direct changes outside of Kubernetes
**C)** KIC periodically reconciles and will overwrite manual Admin API changes — Kubernetes is the source of truth in a KIC deployment
**D)** KIC ignores changes made outside of Kubernetes resources

**Answer: C**
*KIC enforces Kubernetes as the sole source of truth. If someone manually changes config via the Admin API, KIC's reconciliation loop reverts it to match the Kubernetes resource state.*

---

## Q16. In the context of APIOps, what is "configuration drift"?

**A)** The gradual increase in API latency over time due to plugin chain growth
**B)** A state where the live Kong configuration differs from what is defined in the version-controlled state file — typically caused by manual Admin API or UI changes
**C)** The gradual outdating of OpenAPI spec documentation relative to the implementation
**D)** The difference in plugin versions between the dev and prod Control Planes

**Answer: B**
*Configuration drift breaks the GitOps guarantee that "what's in Git is what's running." `deck diff` detects drift; `deck sync` corrects it; RBAC (disabling direct manual changes) prevents it.*

---

## Q17. How does the Konnect Terraform provider differ from decK in what it manages?

**A)** They manage identical resources — use whichever the team prefers
**B)** Terraform manages Kong gateway entities (services, routes, plugins); decK manages Konnect infrastructure (control planes, teams, system accounts)
**C)** Terraform only works with Dedicated Cloud Gateways; decK only works with hybrid deployments
**D)** Terraform manages Konnect infrastructure resources (control planes, system accounts, team assignments); decK manages Kong gateway entities (services, routes, plugins, consumers)

**Answer: D**
*Clear separation: Terraform provisions the Konnect "platform." decK manages the "content" running on the platform. Both belong in the same IaC repository.*

---

## Q18. What Terraform resource creates a Konnect system account for CI/CD pipelines?

**A)** `konnect_user`
**B)** `konnect_ci_account`
**C)** `konnect_system_account`
**D)** `konnect_service_token`

**Answer: C**
*`konnect_system_account` creates a non-human identity for automation. System accounts can be granted roles and generate access tokens used by decK or other tools in pipelines.*

---

## Q19. Why is a system account token preferred over a personal access token for CI/CD pipelines?

**A)** System accounts have higher API rate limits than personal tokens
**B)** Personal tokens cannot be used with the decK CLI at all
**C)** System accounts bypass RBAC restrictions for CI/CD operations
**D)** System account tokens are not tied to an individual user — they don't expire when an employee leaves, can be scoped to minimum required permissions, and are auditable as a non-human identity

**Answer: D**
*Security best practice for automation: service identities (system accounts) rather than human identities. No dependency on employment status; permissions scoped to what the pipeline actually needs.*

---

## Q20. What is the benefit of committing a `deck dump` output to Git before starting a GitOps migration?

**A)** It automatically generates Terraform code from the exported Kong configuration
**B)** It eliminates the need for future `deck diff` reviews during the migration
**C)** It enables the Control Plane to push config to the DP without using a database
**D)** It creates a version-controlled snapshot of existing production configuration — providing a rollback point and a starting state for the GitOps pipeline

**Answer: D**
*Migration baseline: export existing config → commit → start GitOps. If something goes wrong early in the migration, the committed dump provides a known-good state to restore from.*

---

## Q21. A developer adds a new route directly in the Konnect UI without updating the decK state file. What problem does this create?

**A)** The route is tagged automatically as UI-created to prevent accidental deletion
**B)** No problem — UI changes are automatically detected and committed to Git by Konnect
**C)** decK detects the UI change on the next `deck dump` and adds it to the state file
**D)** The state file is now stale — `deck diff` will show the route as "to be deleted," and running `deck sync` will remove it

**Answer: D**
*GitOps discipline: the state file is the source of truth. UI changes not reflected in the file are "drift" — deleted by the next sync. Developers must make changes through the pipeline.*

---

## Q22. Which `inso` CLI flag allows running tests against a specific environment base URL?

**A)** `--workspace`
**B)** `--target-cp`
**C)** `--base-url`
**D)** `--env` — selecting an Insomnia environment that defines the `base_url` variable

**Answer: D**
*`inso run test --env staging` activates the "staging" environment in Insomnia, setting `base_url` to the staging gateway address. Same test suite runs against different environments by switching the env.*

---

## Q23. What is a "golden path" in the context of APIOps?

**A)** The highest-priority route in the Kong expressions router for latency-sensitive APIs
**B)** The sequence of plugin execution phases for a given request through the gateway
**C)** The fastest network path between the client and the upstream service
**D)** A pre-approved, standardized pipeline template that embodies best practices — teams follow it to deploy APIs consistently without reinventing the wheel each time

**Answer: D**
*A golden path APIOps pipeline template (lint → generate → validate → diff → sync → test) is a paved road: teams get a compliant deployment pattern by default, reducing errors and audit findings.*

---

## Q24. How do tags prevent one team's `deck sync` from deleting another team's entities on the same Control Plane?

**A)** Tags are cosmetic labels and have no effect on decK behavior
**B)** Tags prevent entities from being modified but not from being deleted during syncs
**C)** Tags act as namespaces — entities from different teams cannot share the same name
**D)** Each team's pipeline uses `--select-tag team:<name>` — decK only manages entities with that specific tag, leaving other teams' entities completely untouched

**Answer: D**
*Tag-based ownership: each team "owns" their tagged entities. Their pipeline only touches entities with their tag. Cross-team entity deletion is structurally impossible when `--select-tag` is enforced.*

---

## Q25. What is the purpose of running `deck ping` in a CI/CD pipeline?

**A)** To measure the round-trip latency of the Konnect Control Plane API
**B)** To send a health check probe to all upstream services configured in the state file
**C)** To verify that the decK CLI can successfully reach and authenticate to the Konnect Control Plane before attempting a sync
**D)** To wake up idle Data Plane nodes before applying a new configuration

**Answer: C**
*`deck ping` validates connectivity and authentication early in the pipeline. If the token is invalid or the CP is unreachable, the pipeline fails fast — before running generate/validate/diff steps.*

---

## Q26. In an OAS file, where would you add `x-kong-service-defaults` to set global timeouts for the generated Kong service?

**A)** Inside each `securitySchemes` definition under `components`
**B)** As a top-level OAS extension — alongside `info`, `paths`, `servers`
**C)** Under each individual path item to apply per-route settings
**D)** Inside the `info.x-kong` nested object within the API metadata block

**Answer: B**
*Top-level `x-kong-*` extensions apply to the entire generated Service object. `x-kong-service-defaults` sets properties like `connect_timeout`, `read_timeout`, and `write_timeout` for the service.*

---

## Q27. How does APIOps improve the auditability of API changes compared to manual Admin API or UI deployments?

**A)** APIOps stores audit records directly in the decK state file alongside configuration
**B)** APIOps disables the Konnect audit log to avoid redundant change records
**C)** APIOps removes the need for RBAC by enforcing pipeline-only access
**D)** Every API change is a Git commit with author, timestamp, PR discussion, and CI results — providing a complete, immutable audit trail richer than any system audit log alone

**Answer: D**
*Git history = perfect audit trail. PR #123 by alice at 2026-03-01 added rate limiting to the payments API, reviewed and approved by bob. This is richer than any system audit log.*

---

## Q28. What happens to a plugin added to a route in Kong Manager when the next `deck sync` runs (assuming the plugin is not in the state file)?

**A)** decK skips deletion of plugins created through the UI — they are protected
**B)** The plugin is added to the state file automatically by decK during the sync
**C)** The plugin is deleted, because it is not present in the state file and decK treats the file as the desired state
**D)** The sync fails with a conflict error until the plugin is either added to the state file or removed manually

**Answer: C**
*decK enforces desired state. A plugin missing from the state file is "extra" in Kong — `deck sync` deletes it. All changes must flow through the GitOps pipeline.*

---

## Q29. Which combination of tools covers the full APIOps lifecycle from API design to production deployment?

**A)** Kong Manager + Admin API + decK
**B)** Terraform + Helm + KIC
**C)** decK + Prometheus + Grafana
**D)** Insomnia (design + test) → `inso` CLI (lint + generate + run tests in CI) → decK (validate + diff + sync) → Konnect (runtime)

**Answer: D**
*The complete APIOps toolchain: Insomnia (design & test) → inso (pipeline automation) → decK (config deployment) → Konnect (runtime gateway). Each tool has a distinct, non-overlapping role.*

---

## Q30. An organization wants all API changes to require a second engineer's approval before reaching production. How does an APIOps pipeline enforce this?

**Select TWO correct statements.**

**A)** All changes must go through a pull request in Git — the merge requirement enforces the approval gate
**B)** decK is configured with a `--require-approval` flag that pauses the sync for human review
**C)** The CI pipeline runs `deck diff` on the PR, making the changeset visible for review before merge triggers `deck sync`
**D)** Kong Manager is configured with a dual-admin approval mode that gates all configuration changes
**E)** The APIOps pipeline uses `inso lint spec` as the approval gate — only linted specs can be synced

**Answer: A, C**
*Four-eyes principle via GitOps: the PR is the change request; `deck diff` in CI is the visible changeset; required reviewer approval is the gate; merge triggers the prod sync.*
