# Section 18: Kong Gateway Upgrade Approaches and Versioning

> **Exam Theme:** Understanding Kong's versioning scheme, upgrade strategies (in-place, blue/green, rolling), migration commands, and compatibility considerations.

---

## Q1. What versioning scheme does Kong Gateway use?

**A)** Calendar versioning (e.g., 2024.1)
**B)** Semantic versioning — MAJOR.MINOR.PATCH (e.g., 3.6.1)
**C)** Sequential build numbers only
**D)** Date-based release tags

**Answer: B**
*Kong follows Semantic Versioning: MAJOR for breaking changes, MINOR for new features (backward-compatible), PATCH for bug fixes. Example: `3.6.1`.*

---

## Q2. Which type of Kong version change (major, minor, patch) typically requires database migration?

**A)** Patch releases only
**B)** Minor and major releases often include database migrations; patches rarely do
**C)** Only major releases
**D)** All releases always require migrations

**Answer: B**
*Minor and major releases may introduce schema changes requiring `kong migrations up` or `kong migrations bootstrap`. Patch releases (x.y.Z) typically do not change the schema.*

---

## Q3. What is the recommended upgrade path when upgrading across multiple major versions (e.g., from 2.x to 3.x)?

**A)** Jump directly to the target version
**B)** Upgrade sequentially through each major version
**C)** Only patch-level upgrades are supported between majors
**D)** Re-install from scratch; migrations are not supported cross-major

**Answer: B**
*Kong recommends upgrading one major version at a time (2.x → 3.x), following the migration guide for each step. Skipping major versions is not supported.*

---

## Q4. What does `kong migrations up` do during an upgrade?

**A)** It starts Kong with the new binary
**B)** It applies new, additive (non-breaking) database migrations for the new version
**C)** It rolls back all migrations to the previous version
**D)** It restarts all Kong nodes in a cluster

**Answer: B**
*`kong migrations up` applies additive schema changes (new tables, columns) that are safe for both old and new Kong binaries to run against simultaneously.*

---

## Q5. What does `kong migrations finish` do?

**A)** It bootstraps the database for a fresh install
**B)** It applies the destructive migration steps once the new version is confirmed stable
**C)** It terminates the running Kong process cleanly
**D)** It removes the old binary after an upgrade

**Answer: B**
*`kong migrations finish` runs the second phase of migrations — dropping old columns, renaming tables — steps that are incompatible with the old binary. Run only after fully cutover.*

---

## Q6. In a blue/green upgrade strategy with Kong, what runs during the "green" phase?

**A)** The old version receives 100% of traffic while the new version is being deployed
**B)** Both old and new versions serve traffic equally
**C)** The new Kong version is deployed and ready, receiving traffic after cutover
**D)** The database is backed up before the green environment starts

**Answer: C**
*Blue/green: "blue" is the current production version. "Green" is the new version deployed in parallel. Traffic switches from blue to green after validation.*

---

## Q7. Which upgrade strategy involves gradually replacing old Kong nodes with new ones one at a time?

**A)** Blue/green
**B)** In-place
**C)** Rolling upgrade
**D)** Canary upgrade

**Answer: C**
*A rolling upgrade replaces nodes sequentially: stop one old node, start a new node, validate, repeat. Traffic is served by the remaining old nodes during the process.*

---

## Q8. During a rolling upgrade of a Kong cluster, which phase of migrations must be run BEFORE deploying the new binary on any node?

**A)** `kong migrations finish`
**B)** `kong migrations bootstrap`
**C)** `kong migrations up`
**D)** No migrations should run during a rolling upgrade

**Answer: C**
*`kong migrations up` is run once before deploying any new node. It applies additive migrations that both old and new binaries can work with, enabling the cluster to operate in mixed-version mode briefly.*

---

## Q9. What is the risk of running `kong migrations finish` while old-version nodes are still active?

**A)** No risk; it is safe to run at any time
**B)** It drops schema elements the old binary depends on, causing crashes on old nodes
**C)** It locks the database, preventing write access
**D)** It downgrades the database to the previous schema

**Answer: B**
*`kong migrations finish` removes schema elements (old columns, tables) that are no longer needed. Old-version nodes relying on those elements will fail. Ensure all nodes are upgraded first.*

---

## Q10. What is an in-place upgrade?

**A)** Upgrading the database schema without stopping Kong
**B)** Replacing the Kong binary on the same server and restarting the process
**C)** Updating kong.conf without upgrading the binary
**D)** Upgrading a Kubernetes Kong deployment without pod restarts

**Answer: B**
*An in-place upgrade installs the new Kong package over the existing installation and restarts the process. Simplest approach but involves a brief downtime window.*

---

## Q11. Which Kong component manages the sequence and state of database migrations?

**A)** nginx
**B)** The `kong migrations` CLI subcommands and the `schema_meta` database table
**C)** The Admin API `/migrations` endpoint
**D)** decK

**Answer: B**
*Kong tracks which migrations have been applied in the `schema_meta` table. The `kong migrations` commands read and update this table to determine what needs to run.*

---

## Q12. What is the purpose of checking `kong migrations list` before an upgrade?

**A)** It shows the list of available Kong plugins
**B)** It lists pending and executed migrations, confirming the current schema state
**C)** It displays the current Kong version
**D)** It validates the kong.conf file

**Answer: B**
*`kong migrations list` shows which migrations have been executed and which are pending, helping operators verify the database is at the expected state before and after upgrades.*

---

## Q13. What does it mean when a Kong release is marked as an LTS (Long-Term Support) version?

**A)** It includes more plugins than standard releases
**B)** It receives extended security and bug-fix patches for a longer period
**C)** It cannot be upgraded to the next major version
**D)** It is only available for Enterprise customers

**Answer: B**
*LTS versions receive backported security fixes and critical bug fixes for an extended period (typically 2+ years), making them preferred for stable production deployments.*

---

## Q14. Before upgrading Kong, what is the most important preparatory step?

**A)** Disabling all plugins
**B)** Backing up the database
**C)** Stopping all traffic to the proxy
**D)** Deleting existing Routes

**Answer: B**
*Always back up the Kong database before upgrading. If the migration fails or introduces issues, a database backup allows rollback to the previous state.*

---

## Q15. In a Hybrid mode cluster, which node must be upgraded first — the Control Plane or the Data Plane?

**A)** Data Plane first
**B)** Control Plane first
**C)** Both simultaneously
**D)** Either order is acceptable

**Answer: B**
*The Control Plane must be upgraded first. Kong maintains backward compatibility so that a newer CP can communicate with DPs running the previous minor version during a rolling upgrade.*

---

## Q16. How long does Kong guarantee Control Plane / Data Plane compatibility across versions?

**A)** The CP and DP must always be on the exact same version
**B)** Kong supports CP being up to one minor version ahead of DP
**C)** Kong supports CP being up to one major version ahead of DP
**D)** There are no version compatibility requirements between CP and DP

**Answer: B**
*Kong guarantees CP/DP compatibility within one minor version. The CP can run a newer minor version while DPs run the previous one during a rolling upgrade window.*

---

## Q17. Which file or tool can be used to roll back a Kong configuration (not schema) after a bad upgrade?

**A)** `kong rollback`
**B)** A decK state file backed up before the upgrade, applied with `deck sync`
**C)** `kong migrations down`
**D)** `kong restore`

**Answer: B**
*Configuration rollback uses decK. If a post-upgrade config sync introduced problems, restoring the previous decK state file with `deck sync` reverts configuration.*

---

## Q18. When does the Kong changelog mark a change as a "breaking change"?

**A)** When a plugin's default config value changes
**B)** When the change alters API behavior, removes endpoints, or changes data formats in a non-backward-compatible way
**C)** When the change affects only enterprise features
**D)** When the change requires a new license

**Answer: B**
*Breaking changes modify or remove existing behavior such that existing integrations (Admin API clients, decK files, or plugin configs) must be updated to work correctly.*

---

## Q19. Where are Kong's official upgrade guides published?

**A)** In the Kong Insomnia repository
**B)** In the official Kong documentation at docs.konghq.com under "Upgrade"
**C)** In Kong Manager's built-in help section
**D)** In the decK GitHub repository

**Answer: B**
*Kong publishes version-specific upgrade guides at docs.konghq.com. Each major/minor release includes a dedicated upgrade guide with migration notes and breaking changes.*

---

## Q20. Which is the correct order for a zero-downtime blue/green upgrade of Kong with a database?

**A)** Deploy green → `migrations up` → switch traffic → `migrations finish` → decommission blue
**B)** `migrations up` → deploy green → switch traffic → `migrations finish` → decommission blue
**C)** Switch traffic → `migrations up` → deploy green → `migrations finish`
**D)** `migrations finish` → deploy green → `migrations up` → switch traffic

**Answer: B**
*Run `migrations up` first (safe additive changes), then deploy the new version ("green"), validate, switch traffic, then run `migrations finish` once fully cutover.*

---

## Q21. What does the `X-Kong-Version` response header indicate?

**A)** The version of the plugin that processed the request
**B)** The Kong Gateway version of the Data Plane node that served the request
**C)** The decK version used for the last sync
**D)** The version of the upstream API

**Answer: B**
*Kong adds `X-Kong-Version` (or makes the version available via `GET /` on the Admin API) to identify the running version, useful for debugging multi-version rolling upgrades.*

---

## Q22. During an upgrade, you notice `kong migrations up` exits with "no pending migrations." What does this mean?

**A)** The database is corrupt
**B)** The database is already at the schema level required by the new Kong version
**C)** The upgrade failed
**D)** The `kong.conf` is misconfigured

**Answer: B**
*"No pending migrations" means the schema is already compatible — either the database was already up to date or no schema changes were introduced in this release.*

---

## Q23. What configuration management practice reduces risk during Kong upgrades?

**A)** Making live edits via Kong Manager only
**B)** Storing all configuration in a decK state file under version control, enabling comparison before and after upgrades
**C)** Disabling RBAC during upgrades
**D)** Using the default workspace for all entities

**Answer: B**
*Version-controlled decK files provide a clear diff of what changed across upgrade cycles and a reliable way to restore configuration if something goes wrong.*

---

## Q24. Which Kong metric would signal that an upgrade introduced a regression in proxy performance?

**A)** Increasing `X-Kong-Proxy-Latency` values post-upgrade
**B)** A decrease in the number of active routes
**C)** An increase in Admin API response times
**D)** A reduction in database connection count

**Answer: A**
*Rising `X-Kong-Proxy-Latency` (or P99 latency in analytics) after an upgrade indicates that the new version is processing requests more slowly — a key regression indicator.*

---

## Q25. In Kubernetes, how is a Kong rolling upgrade typically performed?

**A)** By manually stopping and restarting each pod
**B)** By updating the Kong Docker image tag in the Helm values and running `helm upgrade`
**C)** By deleting all pods and redeploying
**D)** By running `kong migrations up` from a pod exec

**Answer: B**
*`helm upgrade` updates the Deployment's image tag and Kubernetes performs a rolling update — gradually replacing old pods with new ones, maintaining availability throughout.*

---

## Q26. What is the significance of the "deprecation" period in Kong's release cycle?

**A)** Deprecated features are removed immediately in the next patch
**B)** Deprecated features are supported for at least one major version before removal, giving operators time to migrate
**C)** Deprecation only applies to plugins, not core features
**D)** Deprecated features move to Kong OSS from Enterprise

**Answer: B**
*Kong provides deprecation notices and a grace period (typically one major version) before removing features, so operators can plan migrations without emergency changes.*

---

## Q27. After a failed upgrade, you decide to roll back the Kong binary to the previous version. What must you check regarding the database?

**A)** Nothing — the database is always compatible with older versions
**B)** Whether `migrations finish` was run; if so, the schema may have destructive changes incompatible with the old binary
**C)** Whether PostgreSQL was also upgraded
**D)** Whether decK was synchronized

**Answer: B**
*If `migrations finish` was executed, the schema has been modified (old columns/tables removed). The old binary may fail against this schema, requiring a database restore from backup.*

---

## Q28. Where can you find the list of deprecated and removed features for a specific Kong version?

**A)** In the `kong.conf.default` file
**B)** In the Kong Changelog and version-specific upgrade guide on docs.konghq.com
**C)** In the RBAC audit log
**D)** In the Admin API `/deprecated` endpoint

**Answer: B**
*Kong maintains a Changelog (CHANGELOG.md) and version upgrade guides that list deprecated, changed, and removed features for each release.*

---

## Q29. During a Hybrid mode upgrade, Data Plane nodes running the previous minor version receive configuration from a Control Plane running the new minor version. What behavior is expected?

**A)** DP nodes crash immediately due to version mismatch
**B)** DP nodes continue to run with the last received configuration until upgraded
**C)** DP nodes automatically upgrade themselves
**D)** DP nodes reject all new configuration from the upgraded CP

**Answer: B**
*Kong's CP/DP compatibility guarantees that DPs running the previous minor version continue to operate and receive updates from a newer CP. They process the configuration elements they understand.*

---

## Q30. What tool can automatically test that existing API traffic behaves correctly after a Kong upgrade?

**A)** `kong migrations check`
**B)** `inso run test` against a test suite in Insomnia / CI pipeline
**C)** `deck diff` against the live config
**D)** `kong reload`

**Answer: B**
*Running `inso run test` (Insomnia unit tests) or automated integration tests against the upgraded environment validates that APIs still return expected responses after the upgrade.*
