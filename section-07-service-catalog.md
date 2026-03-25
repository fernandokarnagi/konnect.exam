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

---

## Q13. What is an "API scorecard" in the Service Catalog context, and what does a high score indicate?

**A)** A scorecard measures API throughput and latency at runtime
**B)** A scorecard is a governance assessment that measures how well an API service meets defined best-practice criteria (documentation, security, versioning, SLA definitions); a high score indicates strong compliance with organizational standards
**C)** A scorecard is a developer rating system for APIs in the Dev Portal
**D)** Scorecards measure the number of active consumers registered against an API

**Answer: B**
*Scorecards = governance metrics. High score = the service meets quality standards (well-documented, secured, SLA-defined). Low score = gaps to address before the service is production-ready.*

---

## Q14. How does Service Catalog help reduce the risk of "shadow APIs" — APIs deployed by teams without central visibility?

**A)** By blocking API deployments that are not registered in the catalog
**B)** By providing a central registration mechanism and discovery surface, the catalog encourages all teams to register their services — giving the platform team visibility even into informally managed APIs
**C)** By automatically scanning all network traffic and detecting unregistered services
**D)** Shadow APIs are not a concern because Kong Gateway blocks all unregistered traffic

**Answer: B**
*Catalog reduces shadow API risk through visibility incentives: teams register their services to gain catalog benefits (discoverability, scorecards, integrations) — driving organic adoption of central governance.*

---

## Q15. In which scenario would Service Catalog be MORE appropriate than Analytics?

**A)** A team wants to monitor 5xx error rates for production APIs
**B)** A platform architect wants to identify which team owns a specific service and whether it has documented its API, SLAs, and security policies
**C)** An operations team wants to create a custom dashboard for tracking API latency
**D)** A DevOps team wants to set up alerts for traffic spikes

**Answer: B**
*Ownership, documentation status, and governance compliance = Service Catalog. Traffic metrics, errors, and latency dashboards = Analytics.*

---

## Q16. What is the benefit of the GitHub integration in Service Catalog?

**A)** It allows the catalog to automatically deploy code to services
**B)** It links a catalog service entry to its source code repository — enabling engineers to quickly navigate from a service's catalog entry to its code, recent commits, and pull requests
**C)** It triggers CI/CD pipelines when catalog entries are updated
**D)** It automatically generates API documentation from GitHub README files

**Answer: B**
*GitHub integration connects "this service in the catalog" to "this repo in GitHub" — so engineers responding to incidents or reviewing service quality can immediately access the source code context.*

---

## Q17. What is the benefit of the PagerDuty integration in Service Catalog?

**A)** PagerDuty integration automatically resolves incidents based on catalog scorecard improvements
**B)** It surfaces active incidents and on-call information for a service directly in its catalog entry — so engineers investigating issues can immediately see whether there is an active incident and who is on-call
**C)** It creates PagerDuty incidents automatically when scorecard scores drop below a threshold
**D)** PagerDuty integration is used to schedule planned maintenance windows visible in the catalog

**Answer: B**
*PagerDuty integration = incident context in the catalog. "Is this service currently experiencing an incident? Who do I contact?" — answered without leaving the catalog.*

---

## Q18. How do Service Catalog governance criteria differ from Kong Gateway plugin policies?

**A)** They are the same — governance criteria are enforced as Kong plugins at runtime
**B)** Governance criteria in the catalog are advisory assessments (documenting whether a service meets standards); gateway plugin policies are actively enforced at runtime (blocking or transforming requests)
**C)** Governance criteria are enforced at deploy time; gateway plugins are enforced at runtime
**D)** They differ only in where they are configured — catalog criteria in YAML, gateway plugins in the UI

**Answer: B**
*Important distinction: catalog governance = visibility and reporting (are standards met?). Gateway plugins = active enforcement (block the request if the rule is violated).*

---

## Q19. A company has 200 microservices. Some are managed by Kong Gateway, some use other proxies, and some have no gateway at all. Which Konnect component is the right tool to inventory all 200 services?

**A)** API Gateway — by creating gateway services for each
**B)** Dev Portal — by publishing all services as APIs
**C)** Service Catalog — which is designed to track all technical assets regardless of how they are managed
**D)** Analytics — by aggregating traffic data from all services

**Answer: C**
*Service Catalog is explicitly for all technical assets — not just Kong-managed ones. This is its key differentiator. Register all 200 services regardless of their proxy or management tooling.*

---

## Q20. Which of the following BEST describes the relationship between scorecards and service ownership in Service Catalog?

**A)** Scorecards automatically assign ownership to services based on commit history
**B)** Scorecards provide objective, consistent governance metrics; service ownership records who is accountable for addressing scorecard gaps — together they create accountability for service quality
**C)** Service ownership is determined by scorecard score — the team with the highest score owns the service
**D)** Scorecards and ownership are independent features with no relationship

**Answer: B**
*Scorecards show gaps; ownership shows who to hold accountable. The combination creates an actionable governance model: we know what's wrong AND who needs to fix it.*

---

## Q21. What does the Datadog integration in Service Catalog enable?

**A)** It replaces Konnect Analytics for services tracked in the catalog
**B)** It surfaces Datadog monitoring data (metrics, APM traces, dashboards) directly within a service's catalog entry — providing operational health context alongside governance data
**C)** It automatically configures Datadog monitors for new services added to the catalog
**D)** It exports catalog data to Datadog for compliance reporting

**Answer: B**
*Datadog integration brings monitoring context into the catalog view. Engineers see "is this service healthy right now?" alongside "does it meet governance standards?" — in one place.*

---

## Q22. Which governance criterion would MOST likely appear on a service scorecard?

**A)** The number of active consumers registered in the Dev Portal
**B)** Whether the service has a documented OpenAPI specification with accurate endpoint descriptions, request/response schemas, and error codes
**C)** The geographic region where the service is deployed
**D)** The number of Kong plugins applied to the service's gateway route

**Answer: B**
*API documentation quality is a classic governance criterion. Scorecards check whether documentation exists, is accurate, and covers all endpoints — a foundational API quality standard.*

---

## Q23. How does Service Catalog support large organizations with many teams and hundreds of services?

**A)** By limiting each team to a maximum number of services
**B)** By providing a searchable, filterable catalog with ownership metadata, team attribution, governance scores, and integrated context — making it feasible to manage and discover hundreds of services without direct knowledge of each team's work
**C)** By automatically merging services that have identical functionality
**D)** By requiring teams to consolidate services before they can be cataloged

**Answer: B**
*At scale, discoverability and searchability are critical. Catalog's search, filtering, ownership metadata, and scorecards make navigating 200+ services manageable for any engineer.*

---

## Q24. What is the Slack integration in Service Catalog used for?

**A)** It allows engineers to manage catalog entries through Slack commands
**B)** It links a service's catalog entry to the relevant Slack channels — so engineers can immediately reach the service's team or monitoring alerts channel during incident response
**C)** It sends automatic scorecard reports to team Slack channels weekly
**D)** It creates Slack alerts when new services are added to the catalog

**Answer: B**
*Slack integration = communication context. "Who do I ping when this service is broken?" — the catalog entry links directly to the team's Slack channel.*

---

## Q25. Which Konnect component would a compliance team use to generate a report on which APIs have security policies defined?

**A)** Analytics — for traffic-based security event reporting
**B)** Service Catalog with scorecards — which track security governance criteria (e.g., authentication required, encryption enforced) per service
**C)** Dev Portal — for visible API security documentation
**D)** Gateway Manager — for plugin configuration exports

**Answer: B**
*Compliance reporting on security standards = scorecards. The catalog tracks whether each service meets security governance criteria, enabling compliance teams to identify and prioritize gaps.*

---

## Q26. How does Service Catalog help with incident response compared to having no catalog?

**A)** The catalog automatically creates incidents in PagerDuty when anomalies are detected
**B)** Without a catalog, engineers must ask around to find service owners, locate repositories, and identify monitoring dashboards. The catalog provides all this context immediately — reducing MTTR (mean time to resolution)
**C)** The catalog automatically resolves incidents by rolling back recent changes
**D)** The catalog provides real-time traffic data that helps diagnose incidents faster

**Answer: B**
*MTTR reduction is a key catalog benefit. Instead of spending time finding ownership, code, and monitoring during an incident, engineers have all context in the catalog entry.*

---

## Q27. What is the significance of Service Catalog supporting "non-Kong-managed" services?

**A)** It means the catalog can replace any API gateway for non-Kong services
**B)** It means the catalog has universal applicability — an organization does not need to migrate all services to Kong to gain governance and visibility benefits from the catalog
**C)** Non-Kong services receive limited catalog features compared to Kong-managed services
**D)** Non-Kong services can only be viewed in the catalog, not governed

**Answer: B**
*Universal scope is the catalog's differentiating value. You don't need 100% Kong adoption to benefit. Register your legacy services, partner APIs, and third-party integrations — governance applies everywhere.*

---

## Q28. A team is onboarding a new service and wants to ensure it meets organizational API standards before going to production. Which Service Catalog feature should they use?

**A)** Dev Portal — publish the API early for developer feedback
**B)** Analytics — monitor the new service's traffic patterns during staging
**C)** Scorecards — register the service in the catalog and use scorecard criteria to verify documentation, security, SLA definitions, and best-practice compliance before production
**D)** Gateway Manager — run load tests against the new service

**Answer: C**
*Scorecards as pre-production gates: the team registers the service, checks the scorecard against governance criteria, and addresses gaps before promoting to production.*

---

## Q29. How does the Service Catalog complement Kong Gateway in a full Konnect deployment?

**A)** Service Catalog replaces Gateway Manager for service configuration
**B)** Kong Gateway enforces policies at runtime for API traffic; Service Catalog provides governance, discoverability, and ownership tracking for those same services — and for services beyond Kong's scope
**C)** They are competing products that serve the same purpose
**D)** Service Catalog configures Kong Gateway automatically based on catalog metadata

**Answer: B**
*Gateway = runtime policy enforcement. Catalog = organizational knowledge and governance. Complementary: Gateway keeps traffic safe; Catalog keeps teams informed and accountable.*

---

## Q30. Which of the following scenarios would the Service Catalog LEAST address?

**A)** Identifying which team owns a microservice that is causing production errors
**B)** Tracking whether all services have documented their API contracts
**C)** Detecting a spike in 5xx errors on a specific API route in real time
**D)** Assessing the governance maturity of 150 services using automated scorecard criteria

**Answer: C**
*Real-time traffic monitoring (5xx error spikes) = Analytics. The Service Catalog is a governance and inventory tool, not a real-time monitoring tool.*
