# Kong Konnect Associate Certification — Question Bank

Practice exam questions aligned to the Kong Konnect Associate Certification study guide.
Each file covers one section with 30 multiple-choice questions and detailed answer explanations.

---

## Sections

| # | File | Topic | Questions |
|---|------|--------|-----------|
| 1 | [section-01-business-value-proposition.md](section-01-business-value-proposition.md) | Business Value Proposition | 30 |
| 2 | [section-02-high-level-architecture.md](section-02-high-level-architecture.md) | High-Level Architecture | 30 |
| 3 | [section-03-api-gateway.md](section-03-api-gateway.md) | API Gateway | 30 |
| 4 | [section-04-ai-gateway.md](section-04-ai-gateway.md) | AI Gateway | 30 |
| 5 | [section-05-service-mesh.md](section-05-service-mesh.md) | Service Mesh | 30 |
| 6 | [section-06-dev-portal.md](section-06-dev-portal.md) | Dev Portal | 30 |
| 7 | [section-07-service-catalog.md](section-07-service-catalog.md) | Service Catalog | 30 |
| 8 | [section-08-analytics.md](section-08-analytics.md) | Analytics | 30 |
| 9 | [section-09-organizations-and-teams.md](section-09-organizations-and-teams.md) | Organizations & Teams | 30 |
| 10 | [section-10-cross-platform-scenarios.md](section-10-cross-platform-scenarios.md) | Cross-Platform Scenarios | 30 |
| 11 | [section-11-api-request-response-flow.md](section-11-api-request-response-flow.md) | API Request & Response Flow | 30 |
| 12 | [section-12-admin-api-kong-manager-deck.md](section-12-admin-api-kong-manager-deck.md) | Admin API, Kong Manager & decK | 30 |
| 13 | [section-13-services-routes-plugins-consumers.md](section-13-services-routes-plugins-consumers.md) | Services, Routes, Plugins & Consumers | 30 |
| 14 | [section-14-upstreams-and-load-balancing.md](section-14-upstreams-and-load-balancing.md) | Upstreams & Load Balancing | 30 |
| 15 | [section-15-rbac-roles-workspaces-teams.md](section-15-rbac-roles-workspaces-teams.md) | RBAC, Roles, Workspaces & Teams | 30 |
| 16 | [section-16-insomnia-openapi.md](section-16-insomnia-openapi.md) | Insomnia & OpenAPI Specification | 30 |
| 17 | [section-17-installation-requirements.md](section-17-installation-requirements.md) | Installation Requirements & Steps | 30 |
| 18 | [section-18-upgrade-approaches-versioning.md](section-18-upgrade-approaches-versioning.md) | Upgrade Approaches & Versioning | 30 |
| 19 | [section-19-common-plugins.md](section-19-common-plugins.md) | Common Plugins | 30 |
| 20 | [section-20-troubleshooting.md](section-20-troubleshooting.md) | Troubleshooting Kong Gateway | 30 |

**Total: 600 questions**

---

## Highest-Priority Topics (from study guide)

| Topic | Where to Focus |
|-------|---------------|
| Control Plane vs Data Plane | Section 2, Section 10 |
| Hosting models (Hybrid vs Dedicated Cloud) | Section 2, Section 10 |
| API Gateway fundamentals | Section 3 |
| AI Gateway governance & optimization | Section 4 |
| Dev Portal secured Try It sequence | Section 6 |
| Organizations, teams, geos | Section 9 |
| Cross-platform component mapping | Section 10 |
| Request/response lifecycle & plugin phases | Section 11 |
| Admin API, Kong Manager, and decK | Section 12 |
| Services, Routes, Plugins, Consumers | Section 13 |
| Upstreams, targets, and load balancing | Section 14 |
| RBAC, Workspaces, and Teams (Enterprise) | Section 15 |
| Insomnia & OpenAPI Specification | Section 16 |
| Installation, DB modes, migration commands | Section 17 |
| Upgrade strategies and migration phases | Section 18 |
| Common plugins by category | Section 19 |
| Troubleshooting methodology | Section 20 |

---

## Key Facts to Memorize

- **CP/DP disconnect**: Data Plane continues serving traffic with last known good config
- **Analytics retention**: 7 days default
- **Telemetry during disconnect**: Buffered locally, flushed on reconnect
- **5xx spike**: Points to upstream (backend) problem, not gateway
- **4xx spike**: Points to client-side problem (bad request, expired credentials)
- **Predefined teams**: Cannot be modified or deleted
- **Secured Try It sequence**: Publish → Link → Configure app auth → Enable user auth → Register app → Use credential (PLCER)
- **Service Catalog integrations**: GitHub, Datadog, PagerDuty, Slack
- **Geo**: Separate regional CP deployment; selection is permanent
- **Automation tools**: Terraform, decK, Konnect APIs, Kong Operator
- **East-west traffic**: Service Mesh. North-south traffic: API Gateway
- **mTLS**: Mutual authentication — both sides present certificates
- **MeshTrafficPermission**: Explicit allow rules in default-deny mesh
- **Semantic caching**: Vector similarity match, not exact string match
- **AI PII Sanitizer**: Redacts personal data before reaching AI provider
- **Prompt Guard**: Blocks jailbreaks and policy-violating prompts
- **Config Store**: Secrets vault — AI keys referenced by name, not value
- **Org Admin**: Only role that can manage users, teams, and roles
- **Hybrid mode**: Customer-managed DP + Kong-managed CP
- **Dedicated Cloud Gateway**: Fully managed by Kong, dedicated infrastructure
- **Plugin phase order**: rewrite → access → (upstream) → header_filter → body_filter → log
- **Plugin scope priority (most specific first)**: Consumer > Route > Service > Global
- **strip_path=true**: Removes matched route prefix before forwarding to upstream
- **Admin API default port**: 8001 (HTTP), 8444 (HTTPS)
- **Proxy default port**: 8000 (HTTP), 8443 (HTTPS)
- **decK sync**: Applies state file to Kong (deletes unlisted resources by default)
- **decK dump**: Exports current Kong config to a state file
- **kong migrations bootstrap**: Initialize fresh DB schema
- **kong migrations up**: Apply additive migrations (safe for rolling upgrades)
- **kong migrations finish**: Apply destructive migrations (only after full cutover)
- **kong reload**: Graceful reload — no dropped connections
- **DB-less mode**: `database = off` — config from declarative YAML file
- **Upstream weight=0**: Drains target from rotation without deleting it
- **503**: All upstream targets unhealthy; **502**: Upstream returned invalid response
- **401**: Missing/invalid credentials; **403**: Authenticated but not authorized (ACL)
- **RBAC**: Enterprise only — `enforce_rbac = on` in kong.conf
- **Workspace**: Isolated config partition (Enterprise); `default` workspace always exists
- **inso CLI**: Insomnia CLI for linting specs, running tests, generating decK configs
- **x-kong- extensions**: OAS extensions for embedding Kong gateway config in an API spec
- **LTS releases**: Extended security/bug-fix support (~2 years)
