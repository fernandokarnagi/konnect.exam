# Section 12: Configure Kong Gateway via Admin API, Kong Manager, and decK

> **Exam Theme:** The three configuration surfaces — REST Admin API, GUI Kong Manager, and declarative decK — their use cases, differences, and key behaviors.

---

## Q1. What is the default port for the Kong Admin API?

**A)** 8000
**B)** 8001
**C)** 8443
**D)** 9000

**Answer: B**
*Kong exposes the Admin API on port `8001` (HTTP) and `8444` (HTTPS) by default. Port `8000` is the proxy port.*

---

## Q2. Which HTTP method is used to partially update an existing Kong resource via the Admin API?

**A)** POST
**B)** PUT
**C)** PATCH
**D)** DELETE

**Answer: C**
*`PATCH` performs a partial update (only the fields provided are changed). `PUT` replaces the entire resource.*

---

## Q3. How do you create a Service using the Admin API?

**A)** `GET /services`
**B)** `POST /services` with a JSON body containing name, host, port
**C)** `PUT /config` with the full service definition
**D)** `PATCH /upstreams`

**Answer: B**
*Creating a resource uses `POST` to the collection endpoint. For a Service, `POST /services` with `{"name":"my-service","host":"backend.example.com","port":8080}`.*

---

## Q4. What does decK stand for and what is its primary purpose?

**A)** Declarative Kong — exports Kong logs to a file
**B)** Declarative Configuration Kit — manages Kong configuration as YAML/JSON state files
**C)** Dev Environment Kong — provisions local development environments
**D)** Dynamic Event Kong — streams analytics events

**Answer: B**
*decK (Declarative Configuration Kit) syncs a YAML or JSON state file to Kong's Admin API, enabling GitOps-style configuration management.*

---

## Q5. Which decK command applies a local state file to Kong?

**A)** `deck diff`
**B)** `deck validate`
**C)** `deck sync`
**D)** `deck export`

**Answer: C**
*`deck sync` compares the local state file with the running Kong configuration and applies the delta (creates, updates, deletes resources).*

---

## Q6. What does `deck diff` produce?

**A)** A list of all current Kong entities
**B)** A diff showing what changes would be made without applying them
**C)** A backup file of the current configuration
**D)** A validation report of YAML syntax

**Answer: B**
*`deck diff` is a dry-run: it shows additions, modifications, and deletions that `deck sync` would perform, without making any changes.*

---

## Q7. Which command exports the current Kong configuration into a decK state file?

**A)** `deck sync`
**B)** `deck dump`
**C)** `deck validate`
**D)** `deck convert`

**Answer: B**
*`deck dump` pulls the current running configuration from Kong's Admin API and writes it to a local YAML state file.*

---

## Q8. Kong Manager is best described as:

**A)** A CLI tool for managing Kong from a terminal
**B)** A web-based GUI for configuring and monitoring Kong Gateway
**C)** A plugin that adds a management dashboard to the proxy
**D)** The Admin API itself, accessed via browser

**Answer: B**
*Kong Manager is the built-in browser-based administration UI that provides forms and dashboards for configuring Services, Routes, Plugins, Consumers, and Upstreams.*

---

## Q9. Which of the following is a key advantage of decK over direct Admin API calls in a CI/CD pipeline?

**A)** It is faster than the Admin API
**B)** It provides idempotent, declarative configuration ensuring desired state
**C)** It does not require Kong to be running
**D)** It automatically generates plugin configurations

**Answer: B**
*decK is idempotent — running `deck sync` multiple times produces the same result. It converges Kong to the desired state without manual sequencing of API calls.*

---

## Q10. What happens when a resource exists in Kong but is absent from the decK state file during `deck sync`?

**A)** It is ignored
**B)** It is exported and added to the state file
**C)** It is deleted from Kong
**D)** The sync fails with a conflict error

**Answer: C**
*By default, decK treats the state file as the source of truth. Resources in Kong that are not in the state file are deleted during sync.*

---

## Q11. Which decK flag prevents deletion of resources not present in the state file?

**A)** `--no-delete`
**B)** `--select-tag`
**C)** `--skip-delete`
**D)** `--dry-run`

**Answer: C**
*`--skip-delete` tells decK to only add or update resources, never delete ones absent from the state file.*

---

## Q12. How does decK support managing multiple environments (dev, staging, prod) from a single repository?

**A)** By using separate decK binaries per environment
**B)** By using `--select-tag` to scope operations to tagged resources
**C)** By connecting to different Admin API ports
**D)** By embedding environment names in plugin names

**Answer: B**
*`--select-tag` lets decK operate on a subset of Kong resources that share a specific tag, enabling per-environment management within one Kong instance or across instances.*

---

## Q13. Which Admin API endpoint lists all currently loaded plugins across all scopes?

**A)** `GET /services/{id}/plugins`
**B)** `GET /plugins`
**C)** `GET /routes/{id}/plugins`
**D)** `GET /consumers/{id}/plugins`

**Answer: B**
*`GET /plugins` returns all plugin instances regardless of scope. Scoped variants (`/services/{id}/plugins`) filter by parent entity.*

---

## Q14. In Kong Manager, which section do you navigate to in order to associate a plugin with a specific Route?

**A)** Upstreams → Targets
**B)** Routes → (select route) → Plugins
**C)** Consumers → Credentials
**D)** Dev Portal → Settings

**Answer: B**
*In Kong Manager, after selecting a Route, a "Plugins" tab allows adding and configuring plugins scoped to that Route.*

---

## Q15. What format does decK use for its state files?

**A)** JSON only
**B)** XML only
**C)** YAML (primary) or JSON
**D)** TOML

**Answer: C**
*decK state files are primarily written in YAML, though JSON is also supported. YAML is conventional for readability in GitOps workflows.*

---

## Q16. Which Admin API endpoint would you call to check the health of the Kong node?

**A)** `GET /status`
**B)** `GET /health`
**C)** `GET /ping`
**D)** `GET /info`

**Answer: A**
*`GET /status` returns node health information including database connectivity and memory usage. `GET /` returns basic node metadata.*

---

## Q17. When using decK with Konnect (cloud), which additional flag is required?

**A)** `--kong-addr`
**B)** `--konnect-token` and `--konnect-control-plane-name`
**C)** `--admin-key`
**D)** `--tls-client-cert`

**Answer: B**
*For Konnect, decK uses `--konnect-token` (personal or system access token) and `--konnect-control-plane-name` to target the correct Konnect Control Plane.*

---

## Q18. What is the purpose of the `_format_version` field in a decK state file?

**A)** It specifies the Kong Gateway version
**B)** It declares the schema version decK uses to parse the state file
**C)** It controls whether YAML or JSON output is used
**D)** It sets the API version for the Admin API calls

**Answer: B**
*`_format_version` (e.g., `"3.0"`) tells decK which schema to use when parsing the file. It must match the Kong version's supported format.*

---

## Q19. Which Admin API call retrieves a specific plugin configuration by its UUID?

**A)** `GET /plugins?name={plugin-name}`
**B)** `GET /plugins/{plugin-id}`
**C)** `GET /services/{service-id}/plugins/{plugin-name}`
**D)** `POST /plugins/{plugin-id}`

**Answer: B**
*`GET /plugins/{id}` retrieves the full configuration of a plugin instance by its UUID.*

---

## Q20. Kong Manager requires which Kong Gateway feature to be enabled?

**A)** The Proxy Listener on port 8000
**B)** The Admin API and the `admin_gui_url` configuration
**C)** A Konnect subscription
**D)** The Dev Portal module

**Answer: B**
*Kong Manager is served via the Admin API. It requires `admin_gui_url` (and optionally `admin_gui_api_url`) to be configured so the browser can reach both the GUI and the Admin API.*

---

## Q21. Which decK subcommand validates the syntax and schema of a state file without connecting to Kong?

**A)** `deck diff`
**B)** `deck lint`
**C)** `deck validate`
**D)** `deck check`

**Answer: C**
*`deck validate` checks the state file for structural correctness against the decK schema without requiring a Kong Admin API connection.*

---

## Q22. You want to add a rate-limiting plugin to all routes of a service using the Admin API. Which endpoint do you POST to?

**A)** `POST /plugins`
**B)** `POST /services/{service-id}/plugins`
**C)** `POST /routes/{route-id}/plugins`
**D)** `POST /consumers/{consumer-id}/plugins`

**Answer: B**
*Posting to `/services/{service-id}/plugins` scopes the plugin to that Service, applying it to all requests that match any of its Routes.*

---

## Q23. When Kong Manager's RBAC is enabled, what determines which entities a user can see and modify?

**A)** The user's IP address
**B)** The roles and permissions assigned to the user's RBAC token
**C)** The workspace the user logs in from
**D)** The number of active plugins

**Answer: B**
*RBAC roles carry endpoint permissions (read/write/delete on specific endpoints or entity types). Admin tokens map to roles that gate access in Kong Manager.*

---

## Q24. In decK, what is the purpose of the `--workspace` flag?

**A)** It creates a new workspace in Kong
**B)** It scopes the sync operation to a specific Kong workspace
**C)** It sets the local file directory for state files
**D)** It enables workspace isolation for plugins

**Answer: B**
*`--workspace` targets a specific Kong workspace so that decK reads from and writes to only that workspace's entities.*

---

## Q25. Which Admin API response code indicates a resource was successfully created?

**A)** 200 OK
**B)** 201 Created
**C)** 204 No Content
**D)** 202 Accepted

**Answer: B**
*Kong's Admin API returns `201 Created` when a new resource (Service, Route, Plugin, etc.) is successfully created via POST.*

---

## Q26. What does `deck reset` do?

**A)** Restarts the Kong Gateway process
**B)** Deletes all Kong entities managed by decK (full wipe)
**C)** Resets decK's local cache
**D)** Reverts the last `deck sync` operation

**Answer: B**
*`deck reset` deletes all entities from Kong that fall within decK's scope. Use with caution — it is equivalent to wiping the configuration.*

---

## Q27. Which feature allows decK to render Secrets or environment variables into a state file at sync time?

**A)** decK Tags
**B)** decK Schemas
**C)** decK Environment Variables substitution (`$ENV_VAR`)
**D)** decK Plugins

**Answer: C**
*decK supports `$ENV_VAR` syntax in state files. At sync time it substitutes environment variables, allowing secrets like API keys to be injected without hardcoding them.*

---

## Q28. To enable HTTPS on the Admin API, which Kong configuration parameters are relevant?

**A)** `proxy_listen` and `proxy_ssl_cert`
**B)** `admin_listen` with `ssl` and `admin_ssl_cert` / `admin_ssl_cert_key`
**C)** `cluster_listen` and `cluster_cert`
**D)** `status_listen` and `status_ssl`

**Answer: B**
*The Admin API listener is controlled by `admin_listen`. Adding `ssl` to the listener and providing `admin_ssl_cert`/`admin_ssl_cert_key` enables HTTPS on port 8444.*

---

## Q29. In Kong Manager, which view gives an at-a-glance summary of requests, errors, and latency across all services?

**A)** The Consumers list
**B)** The Overview / Dashboard
**C)** The Upstreams page
**D)** The Plugins catalog

**Answer: B**
*The Overview (Dashboard) in Kong Manager shows high-level traffic metrics including total requests, error rates, and P99 latency, pulled from the Analytics module.*

---

## Q30. Which approach is recommended for production GitOps workflows with Kong?

**A)** Direct Admin API calls from developers' laptops
**B)** Manual Kong Manager edits promoted to production
**C)** decK state files committed to a Git repository, with CI/CD running `deck sync`
**D)** Exporting Kong Manager screenshots as documentation

**Answer: C**
*GitOps best practice: decK state files are the source of truth in Git. Pull requests review config changes, and a CI/CD pipeline runs `deck validate` + `deck diff` + `deck sync` on merge.*
