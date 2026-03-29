# Section 11: APIOps & GitOps

> **Exam Theme:** Applying DevOps principles to the API lifecycle — design-first with OpenAPI, automated pipelines using inso + decK + Terraform, multi-environment promotion, and the Kong Ingress Controller.

---

## Q1. Which foundational principle distinguishes APIOps from traditional API deployment approaches?

**A)** APIs are deployed manually by a dedicated operations team
**B)** API configuration, design, and deployment are treated as code — stored in version control, reviewed via pull requests, and deployed through automated pipelines
**C)** APIOps eliminates the need for testing by using AI to validate deployments
**D)** APIOps applies only to internal APIs and not to public-facing ones

**Answer: B**
*APIOps = DevOps for APIs. Every API artifact (OAS spec, gateway config, plugin settings) is a code artifact — reviewed, versioned, tested, and deployed automatically.*

---

## Q2. What is the "design-first" (or "spec-first") approach in APIOps?

**A)** Deploying code first and extracting the API spec from the running service
**B)** Creating the OpenAPI Specification before writing any backend code — using the spec as the contract that both producer and consumer agree on upfront
**C)** Designing the database schema before writing API code
**D)** Writing unit tests before writing the API implementation

**Answer: B**
*Design-first: the OAS is the source of truth. Frontend and backend teams can work in parallel against the spec. Gateway configuration is generated from it — no ambiguity about the contract.*

---

## Q3. In an APIOps workflow, what artifact does the OpenAPI Specification generate downstream?

**A)** PostgreSQL database schemas
**B)** A decK state file (kong.yaml) containing Kong services, routes, and plugins — via `inso generate config`
**C)** Kubernetes namespace definitions
**D)** Terraform provider configurations

**Answer: B**
*The OAS is the upstream source. `inso generate config` downstream produces a `kong.yaml` decK state file. The pipeline then validates, diffs, and syncs it to the Control Plane.*

---

## Q4. What is the purpose of `x-kong-plugin-*` extensions in an OpenAPI Specification?

**A)** They document which Kong version is required to run the API
**B)** They embed Kong plugin configurations directly in the OAS, so `inso generate config` can include those plugins in the generated decK state file
**C)** They mark endpoints as deprecated for the Dev Portal
**D)** They configure Kong Manager UI labels for each endpoint

**Answer: B**
*`x-kong-*` extensions bring gateway configuration into the spec. Adding `x-kong-plugin-rate-limiting` to a path operation means the generated gateway config includes that plugin — keeping spec and gateway in sync.*

---

## Q5. Which `inso` CLI command runs unit tests defined in an Insomnia collection against a live API endpoint?

**A)** `inso lint spec`
**B)** `inso generate config`
**C)** `inso run test`
**D)** `inso export spec`

**Answer: C**
*`inso run test` executes JavaScript-based test assertions (status codes, response body checks) against a running API. In a CI/CD pipeline this validates that the deployed API behaves as expected.*

---

## Q6. In a CI/CD pipeline using decK, what does `deck validate` check?

**A)** Whether the Konnect Control Plane is reachable
**B)** The structural correctness of a decK state file against the Kong schema — without connecting to Kong
**C)** Whether all upstream services are healthy
**D)** Whether any secrets are exposed in the state file

**Answer: B**
*`deck validate` is an offline check — it catches schema errors (wrong field types, missing required fields) before the pipeline connects to Kong, saving time and preventing bad syncs.*

---

## Q7. What is the purpose of `deck diff` in a CI/CD pipeline?

**A)** It deletes all entities not in the state file
**B)** It shows a human-readable diff of what `deck sync` would change — used as a review/approval gate before applying changes to production
**C)** It compares two different state files
**D)** It exports the current configuration as a YAML file

**Answer: B**
*`deck diff` is the "plan" step (analogous to `terraform plan`). Showing it in a pull request or CI output lets reviewers see exactly what will change before approving the deployment.*

---

## Q8. Which decK command should NEVER be run in a production pipeline without a prior `deck diff` review?

**A)** `deck validate`
**B)** `deck dump`
**C)** `deck sync`
**D)** `deck ping`

**Answer: C**
*`deck sync` applies changes — including deletions. Running it without reviewing the diff first risks accidentally removing services, routes, or plugins in production.*

---

## Q9. How does `deck dump` support the "shift-left" APIOps philosophy?

**A)** It immediately deploys all configuration changes to production
**B)** It exports the current running configuration from Kong into a state file, which can then be committed to Git as a snapshot — useful for bootstrapping GitOps from an existing manual deployment
**C)** It compresses state files to reduce repository size
**D)** It generates OpenAPI specs from the running Kong configuration

**Answer: B**
*`deck dump` is often the starting point when migrating an existing manual deployment to GitOps: export the current config → commit to Git → that file becomes the source of truth going forward.*

---

## Q10. How should API provider API keys and secrets be handled in a decK state file stored in a Git repository?

**A)** Store them in plaintext for convenience — Git access controls are sufficient
**B)** Use decK's environment variable substitution (`$ENV_VAR`) and store the actual secrets in CI/CD secret stores (not in the file), so the Git file never contains secret values
**C)** Encode them in base64 before committing
**D)** Store them in a separate private repository with no CI/CD access

**Answer: B**
*decK supports `$ENV_VAR` placeholders. The state file in Git references `$KONG_ADMIN_KEY` or `$PLUGIN_SECRET` — actual values are injected at runtime from the CI/CD platform's secret vault.*

---

## Q11. In a multi-environment pipeline (dev → staging → prod), what decK mechanism prevents a prod sync from accidentally modifying dev-only entities on the same Control Plane?

**A)** Separate decK binaries per environment
**B)** `--select-tag env:prod` — scoping the sync to only entities tagged `env:prod`
**C)** Running `deck validate` before `deck sync`
**D)** Using separate decK state files with no overlapping entity names

**Answer: B**
*`--select-tag` tells decK to treat only entities with the specified tag as "owned" by this pipeline run. Entities with other tags are invisible to the sync — no accidental cross-environment mutations.*

---

## Q12. An organization promotes API changes from staging to production. What is the safest pipeline step sequence?

**A)** `deck sync` → `deck diff` → `inso lint spec`
**B)** `inso lint spec` → `inso generate config` → `deck validate` → `deck diff` (review) → manual approval → `deck sync`
**C)** `deck sync` → `inso run test` → rollback if tests fail
**D)** `deck dump` → `deck validate` → `deck sync`

**Answer: B**
*Quality gates in order: lint spec → generate config → validate schema → diff review → human approval → sync. Each gate catches a different class of error before it reaches production.*

---

## Q13. What does the `--workspace` flag in decK enable for organizations with multiple Kong workspaces?

**A)** It creates a new workspace on every sync
**B)** It scopes all decK operations (dump, diff, sync) to a specific named workspace — enabling per-workspace pipelines without cross-contamination
**C)** It sets the workspace used by the Konnect UI for display
**D)** It maps workspace names to Kubernetes namespaces

**Answer: B**
*`--workspace payments` means decK only reads from and writes to the "payments" workspace. Teams own their workspace pipeline independently from other teams.*

---

## Q14. Which Konnect feature enables teams to use Kubernetes-native YAML (not decK YAML) to manage Kong configuration?

**A)** Helm charts for decK
**B)** The Kong Ingress Controller (KIC) with CRDs such as `KongPlugin`, `KongConsumer`, and `KongIngress`
**C)** The Konnect Terraform provider
**D)** `deck convert` to Kubernetes format

**Answer: B**
*KIC translates Kubernetes resources into Kong configuration. Teams write `KongPlugin` CRDs and standard `Ingress` objects — KIC reconciles these to the Kong Gateway automatically.*

---

## Q15. How does KIC handle a configuration change made directly via the Kong Admin API (bypassing Kubernetes)?

**A)** KIC ignores changes made outside Kubernetes
**B)** KIC periodically reconciles and may overwrite manual Admin API changes — Kubernetes is the source of truth in a KIC deployment
**C)** KIC merges Admin API changes with the Kubernetes configuration
**D)** KIC disables the Admin API to prevent direct changes

**Answer: B**
*KIC enforces Kubernetes as the sole source of truth. If someone manually changes config via the Admin API, KIC's reconciliation loop will revert it to match the Kubernetes resource state.*

---

## Q16. In the context of APIOps, what is "configuration drift"?

**A)** The gradual increase in API latency over time
**B)** A state where the live Kong configuration differs from what is defined in the version-controlled state file — typically caused by manual Admin API or UI changes
**C)** The gradual outdating of OpenAPI spec documentation
**D)** The difference in plugin versions between environments

**Answer: B**
*Configuration drift breaks the GitOps guarantee that "what's in Git is what's running." `deck diff` detects drift; `deck sync` corrects it; proper RBAC (disabling direct manual changes) prevents it.*

---

## Q17. How does the Konnect Terraform provider differ from decK in what it manages?

**A)** Terraform manages gateway entities (services, routes, plugins); decK manages infrastructure (control planes, teams)
**B)** Terraform manages Konnect infrastructure resources (control planes, system accounts, team assignments); decK manages Kong gateway entities (services, routes, plugins, consumers)
**C)** They manage identical resources — use whichever is preferred
**D)** Terraform only works with Dedicated Cloud Gateways; decK only works with hybrid deployments

**Answer: B**
*Clear separation: Terraform provisions the Konnect "platform" (CP creation, org setup, teams). decK manages the "content" running on the platform (gateway configuration). Both belong in the same IaC repository.*

---

## Q18. What Terraform resource would you use to create a Konnect system account for CI/CD pipelines?

**A)** `konnect_user`
**B)** `konnect_system_account`
**C)** `konnect_service_token`
**D)** `konnect_ci_account`

**Answer: B**
*`konnect_system_account` creates a non-human identity for automation. System accounts can be granted roles and generate access tokens used by decK or other tools in pipelines.*

---

## Q19. Why is a system account token preferred over a personal access token for CI/CD pipelines?

**A)** System accounts have higher API rate limits
**B)** System account tokens are not tied to an individual user — they don't expire when an employee leaves, can be scoped to minimum required permissions, and are auditable as a non-human identity
**C)** Personal tokens cannot be used with decK
**D)** System accounts bypass RBAC restrictions for CI/CD operations

**Answer: B**
*Security best practice for automation: service identities (system accounts) rather than human identities. No dependency on an individual's employment status, and permissions are scoped to what the pipeline actually needs.*

---

## Q20. What is the benefit of committing a `deck dump` output to Git as a baseline before starting a GitOps migration?

**A)** It eliminates the need for future `deck diff` reviews
**B)** It creates a version-controlled snapshot of the existing production configuration — providing a rollback point and a starting state for the GitOps pipeline
**C)** It automatically generates Terraform code from the Kong config
**D)** It enables the Control Plane to push config to the DP without a database

**Answer: B**
*Migration baseline: export existing config → commit → start GitOps. If something goes wrong early in the migration, the committed dump provides a known-good state to restore from.*

---

## Q21. A developer adds a new route directly in the Konnect UI without updating the decK state file. What problem does this create in a GitOps workflow?

**A)** No problem — UI changes are automatically committed to Git
**B)** The state file is now stale — `deck diff` will show the route as "to be deleted" on the next sync, and running `deck sync` will remove it
**C)** The route is tagged automatically to prevent deletion
**D)** decK detects the UI change and adds it to the state file automatically

**Answer: B**
*GitOps discipline: the state file is the source of truth. UI changes that aren't reflected in the file are "drift" — they will be deleted by the next sync. Developers must make changes through the pipeline, not the UI.*

---

## Q22. Which `inso` CLI flag allows running tests against a specific environment base URL?

**A)** `--base-url`
**B)** `--env` (selecting an Insomnia environment that defines the `base_url` variable)
**C)** `--target-cp`
**D)** `--workspace`

**Answer: B**
*`inso run test --env staging` activates the "staging" environment in Insomnia, which sets `base_url` to the staging gateway address. The same test suite runs against different environments by switching the env.*

---

## Q23. What is a "golden path" in the context of APIOps?

**A)** The fastest network path between the client and upstream
**B)** A pre-approved, standardized pipeline template that embodies best practices — teams follow it to deploy APIs consistently without reinventing the wheel each time
**C)** The sequence of plugin execution for a given request
**D)** The highest-priority route in the Kong expressions router

**Answer: B**
*A golden path APIOps pipeline template (lint → generate → validate → diff → sync → test) is a paved road: teams get a compliant deployment pattern by default, reducing errors and audit findings.*

---

## Q24. When using APIOps with multiple teams, each managing their own API segment, how do tags prevent one team's sync from deleting another team's entities?

**A)** Tags act as namespaces — entities from different teams cannot have the same name
**B)** Each team's pipeline uses `--select-tag team:payments` (or similar) — decK only manages and reconciles entities with that specific tag, leaving other teams' entities untouched
**C)** Tags are cosmetic and have no effect on decK behavior
**D)** Tags prevent entities from being modified but not from being deleted

**Answer: B**
*Tag-based ownership: each team "owns" their tagged entities. Their pipeline only touches entities with their tag. Cross-team entity deletion is structurally impossible when `--select-tag` is enforced.*

---

## Q25. What is the purpose of running `deck ping` in a CI/CD pipeline?

**A)** To send a health check to all upstream services
**B)** To verify that the decK CLI can successfully reach and authenticate to the Konnect Control Plane before attempting a sync
**C)** To measure the latency of the Control Plane API
**D)** To wake up idle Data Plane nodes

**Answer: B**
*`deck ping` validates connectivity and authentication early in the pipeline. If the Konnect token is invalid or the CP is unreachable, the pipeline fails fast — before running expensive generate/validate/diff steps.*

---

## Q26. In an OAS file, where would you add `x-kong-service-defaults` to set global timeouts for the generated Kong service?

**A)** Under each individual path item
**B)** At the top-level `info` object
**C)** As a top-level OAS extension (alongside `info`, `paths`, `servers`)
**D)** Inside each `securitySchemes` definition

**Answer: C**
*Top-level `x-kong-*` extensions apply to the entire generated Service object. `x-kong-service-defaults` sets properties like `connect_timeout`, `read_timeout`, and `write_timeout` for the service.*

---

## Q27. How does APIOps improve the auditability of API changes compared to manual Admin API or UI deployments?

**A)** APIOps disables the audit log to reduce storage overhead
**B)** Every API change is a Git commit with author, timestamp, PR discussion, and CI results — providing a complete, immutable audit trail far richer than Konnect's audit log alone
**C)** APIOps removes the need for RBAC by enforcing pipeline-only access
**D)** APIOps stores audit records in the decK state file

**Answer: B**
*Git history = perfect audit trail. PR #123 by alice@company.com at 2026-03-01 added rate limiting to the payments API, reviewed by bob@company.com. This is richer than any system audit log.*

---

## Q28. What happens to a plugin added to a route in Kong Manager when the next `deck sync` runs (assuming the plugin is not in the state file)?

**A)** The plugin is ignored — UI-created entities are protected
**B)** The plugin is deleted, because it is not present in the state file and decK treats the file as the desired state
**C)** The plugin is added to the state file automatically
**D)** decK skips deletion of UI-created entities

**Answer: B**
*decK enforces desired state. If the plugin is missing from the state file, it is "extra" in Kong — and `deck sync` deletes it. This is why all changes must flow through the GitOps pipeline.*

---

## Q29. Which combination of tools covers the full APIOps lifecycle from API design to production deployment?

**A)** Kong Manager + Admin API
**B)** Insomnia (design + test) → `inso` CLI (lint + generate + run tests in CI) → decK (validate + diff + sync) → Konnect (runtime)
**C)** Terraform + Helm + KIC
**D)** decK + Prometheus + Grafana

**Answer: B**
*The complete APIOps toolchain: Insomnia (design & test) → inso (pipeline automation) → decK (config deployment) → Konnect (runtime gateway). Each tool has a distinct, non-overlapping role.*

---

## Q30. An organization wants all API changes to require a second engineer's approval before reaching production. How does an APIOps pipeline enforce this?

**A)** By configuring a `request-termination` plugin as a deployment gate
**B)** By requiring all changes to go through a pull request workflow in Git — the CI pipeline runs `deck diff` on the PR for review, and merge (triggering `deck sync`) requires at least one approver
**C)** By enabling dual-admin mode in Konnect RBAC
**D)** By configuring decK with `--require-approval` flag

**Answer: B**
*Four-eyes principle via GitOps: the PR is the change request; `deck diff` in CI is the changeset; the required reviewer approval is the approval gate; merge triggers the prod sync. Standard software engineering practices applied to API config.*
