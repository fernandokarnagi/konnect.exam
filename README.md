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

**Total: 300 questions**

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
