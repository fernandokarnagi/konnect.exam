# Section 7: Service Catalog

> **Exam Theme:** Centralized inventory of services, scorecards, governance, and third-party integrations.

---

## Q1. What is the core purpose of the Service Catalog?

**A)** To monitor API traffic and provide dashboards for engineering leadership
**B)** To manage routing rules and load balancing across upstream services
**C)** To expose APIs to external developers through documentation and self-service
**D)** To provide a centralized inventory of all technical assets in the organization — whether managed by Kong or not

**Answer: D**
*Service Catalog is the internal asset registry for the platform team. It tracks all services, not just Kong-managed ones — giving a complete picture of the organization's API landscape.*

---

## Q2. Why does the guide describe the catalog as a "single source of truth" for technical assets?

**A)** Because it stores runtime configuration for all gateway services
**B)** Because it aggregates metadata, ownership, operational status, and governance data for all services in one place, eliminating the need to check multiple tools
**C)** Because it replaces all third-party observability and incident management tools
**D)** Because it is the only place where API documentation can be published to developers

**Answer: B**
*Single source of truth = one place to find who owns a service, what its current health is, whether it meets governance standards, and related context (Datadog, PagerDuty, etc.).*

---

## Q3. Who is the Service Catalog mainly intended for?

**A)** External API consumers looking to discover and subscribe to APIs
**B)** Business stakeholders who need executive-level API usage reports
**C)** Platform engineers, architects, and internal teams who need visibility, ownership tracking, and governance oversight of all technical assets
**D)** DevOps engineers managing CI/CD pipelines for gateway deployments

**Answer: C**
*The catalog is an internal tool for platform and engineering teams. It answers: "What services exist? Who owns them? Do they meet standards?" — not for external API consumers.*

---

## Q4. How do scorecards support governance and best practices?

**Select TWO correct statements about scorecards.**

**A)** Scorecards automatically block non-compliant API calls at the gateway level
**B)** Scorecards provide a visual assessment of how well each service meets defined governance criteria, making compliance gaps visible
**C)** Scorecards replace audit logs for regulatory compliance reporting
**D)** Scorecards track API usage metrics and token consumption per consumer
**E)** Scorecards measure service quality against governance criteria without actively enforcing policies at runtime

**Answer: B, E**
*Scorecards measure service quality against governance criteria. They don't automatically block anything — they surface gaps so teams can improve service maturity and compliance.*

---

## Q5. Why does embedding governance in the catalog help reduce tool sprawl?

**A)** It automatically integrates with all third-party systems without any configuration
**B)** It replaces all third-party tools with a single Kong-managed governance platform
**C)** By centralizing governance standards (scorecards, criteria) within the same tool used for asset discovery, teams avoid maintaining separate governance tracking spreadsheets or tools
**D)** It removes the need for a Dev Portal by combining both functions into a single interface

**Answer: C**
*Governance embedded in the catalog means teams don't need a separate wiki, spreadsheet, or tool to track compliance. Discovery and governance coexist in one place.*

---

## Q6. Which third-party systems are explicitly called out as Service Catalog integrations?

**A)** Prometheus, Grafana, Elasticsearch, and Kibana
**B)** Jira, Confluence, ServiceNow, and OpsGenie
**C)** AWS CloudWatch, Azure Monitor, GCP Logging, and Splunk
**D)** GitHub, Datadog, PagerDuty, and Slack

**Answer: D**
*The four integrations named: **GitHub** (source control), **Datadog** (monitoring), **PagerDuty** (incident management), and **Slack** (team communication).*

---

## Q7. What operational benefit comes from seeing GitHub, Datadog, PagerDuty, and Slack context in one place?

**A)** It automatically resolves incidents detected in PagerDuty based on code changes
**B)** It provides real-time traffic routing decisions based on Datadog metrics
**C)** It eliminates the need for dedicated monitoring and incident management tools
**D)** Engineers can see code changes, performance data, active incidents, and team conversations without leaving the catalog — reducing context-switching and mean time to resolution

**Answer: D**
*Unified context = faster diagnosis. During incidents, engineers cross-reference code changes (GitHub), metrics (Datadog), incidents (PagerDuty), and alerts (Slack) in one view.*

---

## Q8. How is Service Catalog different from Dev Portal?

**Select TWO correct statements.**

**A)** Service Catalog is an internal asset inventory with governance, ownership tracking, and scorecard assessments
**B)** Service Catalog is the external-facing developer portal for API consumers to subscribe to APIs
**C)** Dev Portal manages gateway runtime traffic; Service Catalog manages configuration
**D)** Dev Portal is the consumer-facing storefront for discovering and consuming APIs with self-service application registration
**E)** Dev Portal and Service Catalog are the same product with different names

**Answer: A, D**
*Catalog = internal visibility, governance, and ownership for platform/engineering teams. Portal = developer-facing documentation, Try It, and self-service consumption.*

---

## Q9. How is Service Catalog different from Analytics?

**A)** Analytics provides a structural inventory of what services exist; Service Catalog tracks traffic metrics
**B)** They serve the same purpose and are interchangeable for governance use cases
**C)** Service Catalog provides a structural inventory of what services exist and their governance status; Analytics provides runtime traffic observability and error monitoring
**D)** Analytics manages ownership and scorecards; Service Catalog tracks latency metrics

**Answer: C**
*Catalog = structural view (what exists, who owns it, does it meet standards). Analytics = runtime view (how much traffic, what errors, what latency). Complementary, not duplicative.*

---

## Q10. In an exam question about ownership tracking and discoverability of internal technical assets, why is Service Catalog the best fit?

**A)** Because Analytics tracks service ownership via traffic attribution patterns
**B)** Because Dev Portal handles internal discoverability in addition to external exposure
**C)** Because Service Catalog is the only Konnect component that supports SSO authentication
**D)** Because Service Catalog is explicitly designed as a centralized registry for tracking ownership, discoverability, and governance of all internal technical assets — including non-Kong-managed services

**Answer: D**
*The key differentiators: (1) internal asset inventory, (2) ownership tracking, (3) covers non-Kong services too. Any question combining these three points maps to Service Catalog.*

---

## Q11. A platform team wants to know whether all services have API documentation and security policies, without checking each team individually. Which feature addresses this?

**A)** Advanced Analytics dashboards filtered by service
**B)** Dev Portal visibility settings and application approval queues
**C)** Third-party integrations with GitHub and Datadog
**D)** Scorecards with governance criteria for documentation and security compliance

**Answer: D**
*Scorecards aggregate governance checks — documentation coverage, security policies, SLAs — and surface results so the platform team has a single view of compliance gaps.*

---

## Q12. Which statement about Service Catalog's scope is TRUE?

**A)** Service Catalog only tracks services that have been published to the Dev Portal
**B)** Service Catalog is limited to services deployed on Kubernetes clusters
**C)** Service Catalog can track any technical asset across the organization, including services not managed or proxied by Kong
**D)** Service Catalog only tracks services that have a Kong Gateway route configured

**Answer: C**
*This is the key differentiator: Service Catalog is not limited to Kong-managed services. It is a universal inventory — any service can be registered regardless of how it is deployed.*

---

## Q13. What is an "API scorecard" and what does a high score indicate?

**A)** A scorecard measures the number of active consumers registered against an API in the Dev Portal
**B)** A scorecard is a developer rating system where consumers score APIs they use in the portal
**C)** A scorecard measures API throughput and latency at runtime for SLA reporting
**D)** A scorecard is a governance assessment measuring how well a service meets defined best-practice criteria; a high score indicates strong compliance with organizational standards

**Answer: D**
*Scorecards = governance metrics. High score = the service meets quality standards (well-documented, secured, SLA-defined). Low score = gaps to address before the service is production-ready.*

---

## Q14. How does Service Catalog help reduce the risk of "shadow APIs"?

**Select TWO correct statements.**

**A)** Service Catalog automatically scans network traffic to detect unregistered services
**B)** The catalog provides a central registration mechanism that encourages all teams to register their services, giving the platform team visibility into informally managed APIs
**C)** Kong Gateway blocks all traffic from unregistered shadow APIs automatically
**D)** Teams are incentivized to register services to gain catalog benefits like discoverability, scorecards, and integrations — driving organic adoption of central governance
**E)** Shadow APIs are eliminated by requiring Admin API approval before any service can be deployed

**Answer: B, D**
*Catalog reduces shadow API risk through visibility incentives: teams register their services to gain catalog benefits, driving organic adoption of central governance.*

---

## Q15. When would Service Catalog be MORE appropriate than Analytics?

**A)** A team wants to set up alerts for traffic spikes on a critical payment API
**B)** A platform architect wants to identify service ownership and whether APIs have documented SLAs and security policies
**C)** An operations team wants to create a custom latency dashboard across 10 services
**D)** A DevOps team wants to monitor 5xx error rates for production deployments

**Answer: B**
*Ownership, documentation status, and governance compliance = Service Catalog. Traffic metrics, errors, and latency dashboards = Analytics.*

---

## Q16. What is the benefit of the GitHub integration in Service Catalog?

**A)** It triggers CI/CD pipelines automatically when catalog entries are updated
**B)** It automatically generates API documentation from GitHub README files
**C)** It allows the catalog to automatically deploy new code versions to registered services
**D)** It links a catalog service entry to its source code repository — enabling engineers to navigate from a service's catalog entry to its code, recent commits, and pull requests

**Answer: D**
*GitHub integration connects "this service in the catalog" to "this repo in GitHub" — so engineers responding to incidents can immediately access source code context.*

---

## Q17. What is the benefit of the PagerDuty integration in Service Catalog?

**A)** It creates PagerDuty incidents automatically when scorecard scores drop below a threshold
**B)** It surfaces active incidents and on-call information for a service directly in its catalog entry — so engineers can immediately see whether there is an active incident and who is on-call
**C)** It schedules planned maintenance windows visible in the catalog entry
**D)** It automatically resolves incidents based on catalog scorecard improvements

**Answer: B**
*PagerDuty integration = incident context in the catalog. "Is this service currently experiencing an incident? Who do I contact?" — answered without leaving the catalog.*

---

## Q18. How do Service Catalog governance criteria differ from Kong Gateway plugin policies?

**A)** Governance criteria are enforced at deploy time; gateway plugins are enforced at runtime
**B)** They differ only in where they are configured — catalog criteria in YAML, gateway plugins in the UI
**C)** They are the same — governance criteria are implemented as Kong plugins at runtime
**D)** Governance criteria in the catalog are advisory assessments (documenting whether a service meets standards); gateway plugin policies are actively enforced at runtime (blocking or transforming requests)

**Answer: D**
*Catalog governance = visibility and reporting (are standards met?). Gateway plugins = active enforcement (block the request if violated). Conflating them leads to incorrect architecture decisions.*

---

## Q19. A company has 200 microservices — some managed by Kong, some by other proxies, some with no gateway. Which component should inventory all 200?

**A)** API Gateway — by creating gateway services for each microservice
**B)** Analytics — by aggregating traffic data from all services into unified dashboards
**C)** Service Catalog — designed to track all technical assets regardless of how they are managed
**D)** Dev Portal — by publishing all services as APIs for internal developers

**Answer: C**
*Service Catalog is explicitly for all technical assets — not just Kong-managed ones. Register all 200 services regardless of proxy or management tooling.*

---

## Q20. Which statement BEST describes the relationship between scorecards and service ownership?

**Select TWO correct statements.**

**A)** Scorecards automatically assign ownership to services based on who created them in the catalog
**B)** Service ownership records who is accountable for addressing scorecard compliance gaps
**C)** Scorecards and ownership are independent features with no functional relationship
**D)** Scorecards provide objective, consistent governance metrics that reveal what needs to be fixed
**E)** The team with the highest scorecard score automatically becomes the owner of that service

**Answer: B, D**
*Scorecards show gaps; ownership shows who to hold accountable. Together they create an actionable governance model: we know what's wrong AND who needs to fix it.*

---

## Q21. What does the Datadog integration in Service Catalog enable?

**A)** It automatically configures Datadog monitors for every new service added to the catalog
**B)** It exports catalog data to Datadog for compliance and governance reporting
**C)** It replaces Konnect Analytics for services tracked in the catalog
**D)** It surfaces Datadog monitoring data directly within a service's catalog entry — providing operational health context alongside governance data

**Answer: D**
*Datadog integration brings monitoring context into the catalog view. Engineers see "is this service healthy right now?" alongside "does it meet governance standards?" — in one place.*

---

## Q22. Which governance criterion would MOST likely appear on a service scorecard?

**A)** The number of Kong plugins applied to the service's gateway route
**B)** Whether the service has a documented OpenAPI specification with accurate endpoint descriptions, request/response schemas, and error codes
**C)** The geographic region where the service's Data Plane nodes are deployed
**D)** The number of active consumers registered in the Dev Portal against this API

**Answer: B**
*API documentation quality is a classic governance criterion. Scorecards check whether documentation exists, is accurate, and covers all endpoints — a foundational API quality standard.*

---

## Q23. How does Service Catalog support large organizations managing hundreds of services?

**A)** By automatically merging services that have identical functionality to reduce duplication
**B)** By limiting each team to a maximum number of registered services
**C)** By requiring teams to consolidate services before they can be registered in the catalog
**D)** By providing a searchable, filterable catalog with ownership metadata, team attribution, governance scores, and integrated context — making it feasible to discover and manage hundreds of services

**Answer: D**
*At scale, discoverability and searchability are critical. Catalog's search, filtering, ownership metadata, and scorecards make navigating 200+ services manageable.*

---

## Q24. What is the Slack integration in Service Catalog used for?

**A)** It sends automatic scorecard reports to team Slack channels on a weekly schedule
**B)** It allows engineers to manage and update catalog entries through Slack slash commands
**C)** It links a service's catalog entry to relevant Slack channels — so engineers can immediately reach the service's team or monitoring alert channel during incident response
**D)** It creates Slack notifications when new services are added to the catalog

**Answer: C**
*Slack integration = communication context. "Who do I contact when this service is broken?" — the catalog entry links directly to the team's Slack channel.*

---

## Q25. Which Konnect component would a compliance team use to report which APIs have security policies defined?

**A)** Gateway Manager — for plugin configuration exports
**B)** Dev Portal — for visible API security documentation published to consumers
**C)** Analytics — for traffic-based security event and anomaly reporting
**D)** Service Catalog with scorecards — which track security governance criteria per service

**Answer: D**
*Compliance reporting on security standards = scorecards. The catalog tracks whether each service meets security governance criteria, enabling compliance teams to identify and prioritize gaps.*

---

## Q26. How does Service Catalog help with incident response compared to having no catalog?

**Select TWO correct statements.**

**A)** The catalog automatically creates and resolves incidents in PagerDuty when anomalies are detected
**B)** The catalog reduces MTTR by providing immediate access to service ownership, code repository, and monitoring data without asking around
**C)** The catalog automatically rolls back recent deployments when an incident is detected
**D)** Without a catalog, engineers waste time finding service owners, repositories, and monitoring dashboards during incidents
**E)** The catalog provides real-time traffic analysis that automatically identifies the root cause of incidents

**Answer: B, D**
*MTTR reduction: instead of spending time finding ownership, code, and monitoring during an incident, engineers have all context in the catalog entry.*

---

## Q27. What is the significance of Service Catalog supporting "non-Kong-managed" services?

**A)** Non-Kong services receive limited catalog features compared to Kong-managed services
**B)** Non-Kong services can only be viewed in the catalog, not governed with scorecards
**C)** It means the catalog has universal applicability — organizations do not need to migrate all services to Kong to gain governance and visibility benefits
**D)** It means the catalog can replace any API gateway for non-Kong services

**Answer: C**
*Universal scope is the catalog's differentiating value. You don't need 100% Kong adoption to benefit. Register legacy services, partner APIs, and third-party integrations — governance applies everywhere.*

---

## Q28. A team onboarding a new service wants to verify it meets organizational API standards before production. Which Service Catalog feature should they use?

**A)** Scorecards — register the service in the catalog and use scorecard criteria to verify documentation, security, SLA definitions, and best-practice compliance before production promotion
**B)** Analytics — monitor the new service's traffic patterns during staging
**C)** Gateway Manager — run load tests against the new service via the Admin API
**D)** Dev Portal — publish the API early to gather developer feedback on the design

**Answer: A**
*Scorecards as pre-production gates: register the service, check the scorecard against governance criteria, and address gaps before promoting to production.*

---

## Q29. How does the Service Catalog complement Kong Gateway in a full Konnect deployment?

**A)** They are competing products that serve the same purpose at different scales
**B)** Service Catalog configures Kong Gateway automatically based on catalog metadata
**C)** Service Catalog replaces Gateway Manager for service-level configuration
**D)** Kong Gateway enforces policies at runtime for API traffic; Service Catalog provides governance, discoverability, and ownership tracking for those same services — and for services beyond Kong's scope

**Answer: D**
*Gateway = runtime policy enforcement. Catalog = organizational knowledge and governance. Complementary: Gateway keeps traffic safe; Catalog keeps teams informed and accountable.*

---

## Q30. Which of the following scenarios would the Service Catalog LEAST address?

**A)** Assessing governance maturity of 150 services using automated scorecard criteria
**B)** Tracking whether all services have documented their API contracts with OpenAPI specs
**C)** Detecting a spike in 5xx errors on a specific API route in real time
**D)** Identifying which team owns a microservice that is causing production errors

**Answer: C**
*Real-time traffic monitoring (5xx error spikes) = Analytics. The Service Catalog is a governance and inventory tool, not a real-time monitoring tool.*
