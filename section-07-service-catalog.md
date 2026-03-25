# Section 7: Service Catalog

> **Exam Theme:** Centralized inventory of services, scorecards, governance, and third-party integrations.

---

## Q1. What is the core purpose of Service Catalog?

**A)** To expose APIs to external developers through documentation and self-service
**B)** To provide a centralized inventory of all technical assets in the organization — whether managed by Kong or not
**C)** To manage routing rules and load balancing across upstream services
**D)** To monitor API traffic and provide dashboards for engineering leadership

**Answer: B**
*Service Catalog is the internal asset registry for the platform team. It tracks all services, not just Kong-managed ones — giving a complete picture of the organization's API landscape.*

---

## Q2. Why does the guide describe the catalog as a "single source of truth" for technical assets?

**A)** Because it is the only place where API documentation can be published
**B)** Because it aggregates metadata, ownership, operational status, and governance data for all services in one place, eliminating the need to check multiple tools
**C)** Because it stores runtime configuration for all gateway services
**D)** Because it replaces all third-party observability and incident management tools

**Answer: B**
*Single source of truth = one place to find who owns a service, what its current health is, whether it meets governance standards, and where to find related context (Datadog, PagerDuty, etc.).*

---

## Q3. Who is the Service Catalog mainly intended for?

**A)** External API consumers looking to discover and subscribe to APIs
**B)** Platform engineers, architects, and internal teams who need visibility, ownership tracking, and governance oversight of all technical assets
**C)** Business stakeholders who need executive-level API usage reports
**D)** DevOps engineers managing CI/CD pipelines for gateway deployments

**Answer: B**
*The catalog is an internal tool for platform and engineering teams. It answers: "What services exist? Who owns them? Do they meet standards?" — not for external API consumers.*

---

## Q4. How do scorecards support governance and best practices?

**A)** Scorecards automatically enforce policies by blocking non-compliant API calls
**B)** Scorecards provide a visual assessment of how well each service meets defined governance criteria (documentation, security, SLAs, etc.), making compliance gaps visible
**C)** Scorecards track API usage metrics and token consumption
**D)** Scorecards replace audit logs for compliance reporting

**Answer: B**
*Scorecards measure service quality against governance criteria. They don't automatically block anything — they surface gaps so teams can improve service maturity and compliance.*

---

## Q5. Why does embedding governance in the catalog help reduce tool sprawl?

**A)** It replaces all third-party tools with a single Kong-managed system
**B)** By centralizing governance standards (scorecards, criteria) within the same tool used for asset discovery, teams avoid maintaining separate governance tracking spreadsheets or tools
**C)** It automatically integrates with all third-party systems without any configuration
**D)** It removes the need for a Dev Portal by combining both functions

**Answer: B**
*Governance embedded in the catalog means teams don't need a separate wiki, spreadsheet, or tool to track compliance. Discovery and governance coexist in one place.*

---

## Q6. Which third-party systems are explicitly called out as Service Catalog integrations in the study guide?

**A)** AWS CloudWatch, Azure Monitor, GCP Logging, and Splunk
**B)** GitHub, Datadog, PagerDuty, and Slack
**C)** Jira, Confluence, ServiceNow, and OpsGenie
**D)** Prometheus, Grafana, Elasticsearch, and Kibana

**Answer: B**
*The four integrations named in the guide are: **GitHub** (source control), **Datadog** (monitoring), **PagerDuty** (incident management), and **Slack** (team communication).*

---

## Q7. What kind of operational benefit comes from seeing GitHub, Datadog, PagerDuty, and Slack context in one place?

**A)** It eliminates the need for dedicated monitoring and incident tools
**B)** Engineers can see a service's code changes, performance data, active incidents, and team conversations without leaving the catalog — reducing context-switching and mean time to resolution
**C)** It automatically resolves incidents detected in PagerDuty
**D)** It provides real-time traffic routing decisions based on Datadog metrics

**Answer: B**
*Unified context = faster diagnosis. When an incident fires, engineers can immediately cross-reference the related code changes (GitHub), metrics (Datadog), incidents (PagerDuty), and alerts (Slack) in one view.*

---

## Q8. How is Service Catalog different from Dev Portal?

**A)** Dev Portal is for internal teams; Service Catalog is for external developers
**B)** Service Catalog is an internal asset inventory with governance and ownership tracking; Dev Portal is the external-facing (or private) storefront for API consumers to discover and consume APIs
**C)** They are the same tool with different names
**D)** Service Catalog manages runtime traffic; Dev Portal manages configuration

**Answer: B**
*Catalog = internal visibility, governance, and ownership for platform/engineering teams. Portal = developer-facing documentation, Try It, and self-service consumption.*

---

## Q9. How is Service Catalog different from Analytics?

**A)** Analytics is for internal teams; Service Catalog is for external developers
**B)** Service Catalog provides a structural inventory of what services exist and their governance status; Analytics provides runtime traffic observability and error monitoring data
**C)** Analytics manages ownership and scorecards; Service Catalog tracks traffic metrics
**D)** They serve the same purpose and are interchangeable

**Answer: B**
*Catalog = structural view (what exists, who owns it, does it meet standards). Analytics = runtime view (how much traffic, what errors, what latency). Complementary, not duplicative.*

---

## Q10. In an exam question about ownership tracking and discoverability of internal technical assets, why is Service Catalog the best fit?

**A)** Because Service Catalog is the only Konnect component that supports authentication
**B)** Because Service Catalog is explicitly designed to serve as the centralized registry for tracking ownership, discoverability, and governance of all internal technical assets — regardless of whether they are managed by Kong
**C)** Because Dev Portal handles internal discoverability in addition to external exposure
**D)** Because Analytics tracks service ownership via traffic attribution

**Answer: B**
*The key differentiators: (1) internal asset inventory, (2) ownership tracking, (3) covers non-Kong services too. Any question combining these three points should map to Service Catalog.*

---

## Q11. A platform team wants to know whether all their services have API documentation and security policies in place, without checking each team individually. Which Service Catalog feature addresses this need?

**A)** Third-party integrations with GitHub and Datadog
**B)** Scorecards with governance criteria for documentation and security compliance
**C)** Advanced Analytics dashboards
**D)** Dev Portal visibility settings

**Answer: B**
*Scorecards aggregate governance checks — documentation coverage, security policies, SLAs — and surface the results in the catalog so the platform team has a single view of compliance gaps.*

---

## Q12. Which statement about Service Catalog's scope is TRUE?

**A)** Service Catalog only tracks services that have a Kong Gateway route configured
**B)** Service Catalog is limited to services deployed on Kubernetes
**C)** Service Catalog can track any technical asset across the organization, including services not managed or proxied by Kong
**D)** Service Catalog only tracks services that have been published to the Dev Portal

**Answer: C**
*This is a key differentiator: Service Catalog is not limited to Kong-managed services. It is a universal inventory — any service can be registered, regardless of how it is deployed or managed.*
