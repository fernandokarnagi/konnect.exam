# Section 17: Kong Gateway Installation Requirements and Steps

> **Exam Theme:** Hardware/software prerequisites, supported platforms, installation methods, and bootstrap steps for Kong Gateway.

---

## Q1. What database does Kong Gateway support for storing its configuration?

**A)** MySQL and MSSQL
**B)** PostgreSQL and Cassandra (Kong OSS); PostgreSQL (Kong Enterprise)
**C)** PostgreSQL only (all editions)
**D)** MongoDB and CouchDB

**Answer: B**
*Kong OSS supports PostgreSQL and Cassandra. Kong Gateway Enterprise supports PostgreSQL. DB-less mode (no database) is also supported using declarative configuration.*

---

## Q2. What is "DB-less mode" in Kong Gateway?

**A)** A mode where Kong stores config in memory only (lost on restart)
**B)** A deployment mode where Kong uses a declarative YAML config file instead of a database
**C)** A mode that disables the Admin API
**D)** A Kubernetes-only deployment mode

**Answer: B**
*In DB-less mode, Kong loads configuration from a declarative YAML file at startup. No database is needed, making it suitable for stateless, immutable deployments.*

---

## Q3. Which PostgreSQL version is the minimum recommended for Kong Gateway?

**A)** PostgreSQL 9.5
**B)** PostgreSQL 12
**C)** PostgreSQL 15
**D)** PostgreSQL 10

**Answer: B**
*Kong Gateway Enterprise recommends PostgreSQL 12 or higher. Always verify the specific version matrix in Kong's official documentation for the Kong version being installed.*

---

## Q4. What command initializes the Kong database schema during a fresh installation?

**A)** `kong start --init-db`
**B)** `kong migrations bootstrap`
**C)** `kong db-init`
**D)** `kong schema create`

**Answer: B**
*`kong migrations bootstrap` runs all pending migrations on a fresh database, creating the necessary tables and indexes. It must be run before the first `kong start`.*

---

## Q5. Which operating systems are officially supported for Kong Gateway installation?

**A)** Ubuntu only
**B)** Amazon Linux, CentOS/RHEL, Debian, Ubuntu, and macOS (for development)
**C)** Windows Server 2022 and Ubuntu
**D)** Any POSIX-compliant OS without restriction

**Answer: B**
*Kong officially supports Amazon Linux 2, CentOS/RHEL 7/8, Debian, Ubuntu (LTS releases), and macOS (for development). Docker images are also available.*

---

## Q6. What is the minimum recommended RAM for a Kong Gateway production node?

**A)** 512 MB
**B)** 1 GB
**C)** 2 GB
**D)** 8 GB

**Answer: C**
*Kong recommends at least 2 GB RAM for production nodes, though actual requirements depend on traffic volume, number of plugins, and configuration size.*

---

## Q7. Which installation method pulls the official Kong Docker image?

**A)** `apt install kong`
**B)** `brew install kong`
**C)** `docker pull kong/kong-gateway:<version>`
**D)** `yum install kong-enterprise`

**Answer: C**
*Kong's official Docker images are available on Docker Hub under `kong/kong-gateway`. The tag format is `kong/kong-gateway:<version>` for Enterprise.*

---

## Q8. What is the purpose of `kong.conf` (or `kong.conf.default`)?

**A)** Stores RBAC token values
**B)** The primary configuration file for Kong Gateway — defines ports, database, plugins, etc.
**C)** The decK state file
**D)** The nginx configuration overlay

**Answer: B**
*`kong.conf` is Kong's main configuration file, controlling database connections, listener ports, plugin lists, logging, security, and more.*

---

## Q9. Which environment variable is the standard way to override `kong.conf` settings in containerized deployments?

**A)** `KONG_CONFIG_<PARAM>`
**B)** `KONG_<PARAM>` (uppercase, underscores replace dots)
**C)** `GATEWAY_<PARAM>`
**D)** `KONG_ENV_<PARAM>`

**Answer: B**
*Every `kong.conf` parameter has a corresponding environment variable: `KONG_<PARAM_IN_UPPERCASE>`. For example, `database` → `KONG_DATABASE`, `proxy_listen` → `KONG_PROXY_LISTEN`.*

---

## Q10. After `kong migrations bootstrap`, what is the next step to start Kong?

**A)** `kong db-up`
**B)** `kong reload`
**C)** `kong start`
**D)** `service kong restart`

**Answer: C**
*`kong start` launches the Kong Gateway process (nginx workers + background processes) using the active configuration.*

---

## Q11. Which Kong command validates the configuration file without starting Kong?

**A)** `kong check`
**B)** `kong config test`
**C)** `kong validate`
**D)** `kong status`

**Answer: A**
*`kong check` reads `kong.conf` (and environment variables) and validates the configuration, reporting errors without starting the gateway.*

---

## Q12. Which port is Kong's proxy listener bound to for plain HTTP by default?

**A)** 80
**B)** 443
**C)** 8000
**D)** 8001

**Answer: C**
*By default, Kong's HTTP proxy listener is on port `8000`. The HTTPS proxy is on `8443`. Ports `8001`/`8444` are the Admin API.*

---

## Q13. In a Kubernetes deployment, what is the recommended way to install Kong Gateway?

**A)** `kubectl apply -f kong.yaml` from the official GitHub
**B)** Using the Kong Helm chart via `helm install`
**C)** Running Kong as a DaemonSet without configuration
**D)** Using `apt` inside a Kubernetes init container

**Answer: B**
*The Kong Helm chart is the recommended Kubernetes installation method. It handles deployment, services, configmaps, secrets, and integrates with Kong Ingress Controller.*

---

## Q14. What is the Kong Ingress Controller (KIC)?

**A)** A plugin that controls ingress traffic rate
**B)** A Kubernetes controller that configures Kong based on Kubernetes Ingress and custom resources
**C)** A tool for migrating from NGINX Ingress to Kong
**D)** A Kong Enterprise feature for managing ingress certificates

**Answer: B**
*KIC (Kong Ingress Controller) watches Kubernetes API objects (Ingress, KongPlugin, KongConsumer, etc.) and automatically configures Kong Gateway to match, eliminating manual Admin API calls.*

---

## Q15. Which Kong configuration parameter controls which plugins are loaded at startup?

**A)** `plugins`
**B)** `enabled_plugins`
**C)** `bundled_plugins`
**D)** `plugin_load_order`

**Answer: A**
*The `plugins` parameter in `kong.conf` (or `KONG_PLUGINS` env var) lists which plugins to load. Default is `bundled` (all bundled plugins). Custom plugins must be added here.*

---

## Q16. What is the correct sequence for a fresh Kong installation with a database?

**A)** Start Kong → Run migrations → Configure kong.conf
**B)** Configure kong.conf → Run `kong migrations bootstrap` → Run `kong start`
**C)** Run `kong start` → Configure kong.conf → Run migrations
**D)** Run migrations → Start Kong → Configure kong.conf

**Answer: B**
*Install → configure `kong.conf` (DB connection, ports, etc.) → `kong migrations bootstrap` (initialize DB) → `kong start` (launch the process).*

---

## Q17. Where does Kong store its default configuration file template?

**A)** `/etc/kong/kong.conf`
**B)** `/usr/local/share/lua/kong.conf.default` (or equivalent based on install method)
**C)** `~/.kong/config`
**D)** `/var/kong/conf`

**Answer: B**
*`kong.conf.default` is the template with all available parameters commented out. Copy it to `/etc/kong/kong.conf` and uncomment/modify settings as needed.*

---

## Q18. Which Kong command applies pending database migrations without restarting Kong (for minor schema updates)?

**A)** `kong migrations up`
**B)** `kong db-migrate`
**C)** `kong reload`
**D)** `kong migrations bootstrap`

**Answer: A**
*`kong migrations up` applies new migrations (added by an upgrade) to an existing database. It is non-destructive and safe to run on a running cluster.*

---

## Q19. What does the `nginx_worker_processes` setting in `kong.conf` control?

**A)** The number of Kong instances in a cluster
**B)** The number of nginx worker processes handling proxy requests
**C)** The number of database connection pools
**D)** The number of plugin execution threads

**Answer: B**
*`nginx_worker_processes` (default: `auto`, which uses the number of CPU cores) sets how many nginx workers handle incoming requests in parallel.*

---

## Q20. Which component of Kong Gateway handles the data plane traffic (proxying requests)?

**A)** lua-nginx-module embedded in nginx
**B)** A separate Go binary
**C)** The Admin API server
**D)** PostgreSQL

**Answer: A**
*Kong runs as an nginx instance with the lua-nginx-module. The Lua runtime executes Kong's core logic (routing, plugins) within nginx request-handling workers.*

---

## Q21. What is the minimum supported version of OpenSSL for Kong Gateway installations?

**A)** OpenSSL 1.0.1
**B)** OpenSSL 1.1.1
**C)** OpenSSL 3.0
**D)** LibreSSL 2.9

**Answer: B**
*Kong Gateway requires OpenSSL 1.1.1 or later. This is bundled with Kong packages on most platforms, so manual installation is typically unnecessary.*

---

## Q22. In a Hybrid mode deployment, what must be installed on the Data Plane nodes?

**A)** Kong Gateway + PostgreSQL
**B)** Kong Gateway (data plane role) + the cluster certificate pair
**C)** Kong Manager + the Admin API
**D)** decK + the state file

**Answer: B**
*Data Plane nodes run Kong in `role = data_plane` mode and use a mTLS certificate pair to authenticate with the Control Plane. No database is needed on the DP.*

---

## Q23. Which file extension is used for Kong's SSL certificates and keys in `kong.conf`?

**A)** `.pfx`
**B)** `.pem` (PEM format for certs and keys)
**C)** `.der`
**D)** `.p12`

**Answer: B**
*Kong uses PEM-format certificates (`.pem` or `.crt`) and keys (`.pem` or `.key`). Parameters like `ssl_cert` and `ssl_cert_key` point to these files.*

---

## Q24. You are installing Kong Enterprise. What additional item is required compared to Kong OSS?

**A)** A separate nginx binary
**B)** A valid Enterprise license file
**C)** A dedicated PostgreSQL cluster
**D)** Kong Manager as a separate download

**Answer: B**
*Kong Enterprise requires a license file. It is set via `KONG_LICENSE_DATA` (inline JSON) or `KONG_LICENSE_PATH` (path to a file) environment variables.*

---

## Q25. What does the `lua_package_path` setting configure?

**A)** The path to the Kong binary
**B)** Where Lua searches for custom plugin modules
**C)** The path to PostgreSQL Lua drivers
**D)** The nginx Lua cache directory

**Answer: B**
*`lua_package_path` adds directories to Lua's module search path, enabling Kong to find custom plugin files placed outside the default locations.*

---

## Q26. Which command gracefully reloads Kong Gateway after a configuration change without dropping connections?

**A)** `kong restart`
**B)** `kong reload`
**C)** `kong restart --graceful`
**D)** `nginx -s reload`

**Answer: B**
*`kong reload` sends a graceful reload signal to nginx. In-flight connections complete before workers restart with the new configuration — zero downtime for existing requests.*

---

## Q27. What is the purpose of `kong migrations finish` in the context of a blue/green upgrade?

**A)** It bootstraps a fresh database
**B)** It completes deferred (destructive) migration steps after the new version is stable
**C)** It terminates all running migrations
**D)** It rolls back the last migration

**Answer: B**
*During a two-phase upgrade, `kong migrations up` applies safe (additive) changes. Once the new version is confirmed stable, `kong migrations finish` applies the remaining (destructive/breaking) steps.*

---

## Q28. To list all available configuration parameters, which command would you run?

**A)** `kong config list`
**B)** `kong help config`
**C)** Review `/usr/local/share/kong/kong.conf.default` (commented reference)
**D)** `kong --params`

**Answer: C**
*`kong.conf.default` contains every configuration parameter with its description, default value, and accepted values as comments — it is the canonical reference.*

---

## Q29. Which mode allows Kong to run without any database, loading config from a declarative file, suited for read-only Data Plane nodes?

**A)** `role = traditional`
**B)** `database = off` (DB-less mode) or `role = data_plane`
**C)** `database = none`
**D)** `mode = stateless`

**Answer: B**
*`database = off` enables DB-less mode (standalone). For Hybrid mode Data Planes, `role = data_plane` also avoids a local database, receiving config from the Control Plane.*

---

## Q30. What is the effect of setting `proxy_listen = 0.0.0.0:8000, 0.0.0.0:8443 ssl` in `kong.conf`?

**A)** Kong listens for proxy traffic on HTTP port 8000 and HTTPS port 8443 on all interfaces
**B)** Kong listens only on localhost for security
**C)** Kong disables the Admin API
**D)** Kong enables TCP proxy mode

**Answer: A**
*`0.0.0.0` means all network interfaces. This configuration accepts HTTP on 8000 and HTTPS (TLS) on 8443, binding to all available IPs on the host.*
