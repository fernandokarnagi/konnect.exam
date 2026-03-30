# Section 8: Analytics & Observability

> **Exam Theme:** Centralized observability, ingestion controls, error monitoring, custom dashboards, retention, telemetry buffering, and external plugin integrations.

---

## Q1. What is meant by "centralized observability" in the Analytics section?

**A)** Observability data is collected only from Dedicated Cloud Gateways, not hybrid deployments
**B)** All gateway nodes send logs directly to a third-party tool such as Grafana
**C)** Analytics aggregates traffic data, metrics, and error information from all Control Planes and Data Planes into a single Konnect-managed observability platform
**D)** Centralized observability means all services must route through a single Data Plane for monitoring purposes

**Answer: C**
*Centralized = one place to observe all API traffic across all environments. Rather than checking each gateway or cloud separately, Konnect Analytics aggregates everything.*

---

## Q2. How is analytics ingestion controlled in Konnect?

**A)** Ingestion can only be controlled at the organization level, not per Control Plane
**B)** Advanced Analytics ingestion can be enabled or disabled per Control Plane, giving teams control over which gateways contribute data to the analytics platform
**C)** Ingestion is always on and cannot be modified after a Control Plane is created
**D)** Ingestion is controlled by the Data Plane independently of any Control Plane setting

**Answer: B**
*Per-CP ingestion control allows selective analytics data sending — useful for cost management, compliance (not sending data from certain regions), or phased rollouts.*

---

## Q3. What does a sudden spike in 5xx errors most likely indicate?

**Select TWO correct statements.**

**A)** The spike indicates clients are submitting requests with invalid authentication credentials
**B)** The spike indicates the gateway is successfully routing traffic but the backend is returning server errors
**C)** A rate limiting policy has become too aggressive and is blocking legitimate requests
**D)** An upstream service is experiencing problems — it is overloaded, has crashed, or is misconfigured
**E)** The Control Plane is propagating incorrect routing configuration to the Data Plane

**Answer: B, D**
*5xx errors are server errors. In a gateway context, spikes typically mean the upstream service is failing (overloaded, crashed, misconfigured) — the gateway is routing correctly.*

---

## Q4. Why are custom dashboards important in Konnect Analytics?

**A)** They are required for Advanced Analytics to function
**B)** They automatically resolve errors detected in traffic data
**C)** They allow teams to build views tailored to their specific KPIs — combining the metrics that matter most for their services, teams, or executives
**D)** They replace the default metrics provided by Konnect entirely

**Answer: C**
*Out-of-the-box dashboards cover common cases. Custom dashboards let teams focus on their specific signals — per-service error rates, consumer adoption trends, SLA compliance.*

---

## Q5. What is the default out-of-the-box analytics retention period?

**A)** 30 days
**B)** 90 days
**C)** 24 hours
**D)** 7 days

**Answer: D**
*The default analytics retention is **7 days**. This is a direct fact likely to be tested — know it precisely.*

---

## Q6. What happens to telemetry if a Data Plane loses connectivity to the Control Plane?

**A)** The Data Plane stops proxying traffic to avoid generating untracked telemetry
**B)** All telemetry data is permanently lost for the duration of the disconnect
**C)** Telemetry is forwarded directly to third-party tools, bypassing the Control Plane entirely
**D)** Telemetry is buffered locally on the Data Plane and forwarded to the analytics system once connectivity is restored

**Answer: D**
*Telemetry buffering ensures no observability gaps during transient network issues. Once the Data Plane reconnects, buffered data is flushed to the analytics platform.*

---

## Q7. Why should you avoid confusing analytics data with live API traffic handling?

**A)** Because analytics data can be used to block traffic in real time based on error thresholds
**B)** Because live traffic is handled by the Control Plane, not the Data Plane
**C)** Because analytics and live traffic are processed by the same nginx worker process
**D)** Because analytics is a reporting and observability function that observes past traffic; live traffic handling is the real-time proxy function — they are separate concerns with separate failure modes

**Answer: D**
*Analytics = after-the-fact observability (what happened). Live traffic = real-time proxy (what is happening now). Mixing them up leads to incorrect architecture decisions.*

---

## Q8. How would you distinguish error monitoring from data ingestion controls?

**A)** Error monitoring controls which gateways send data; ingestion controls detect 5xx spikes
**B)** They are the same feature with different names in different areas of the Konnect UI
**C)** Error monitoring is a third-party integration; ingestion control is a native Konnect feature
**D)** Error monitoring detects and analyzes failure patterns in traffic (4xx, 5xx rates); ingestion controls determine whether and how analytics data is collected from a given Control Plane

**Answer: D**
*Error monitoring = "what errors are happening in my traffic?" Ingestion control = "should this CP be sending analytics data?" Different layers, different purposes.*

---

## Q9. When would analytics be part of an executive or business-level discussion?

**Select TWO correct answers.**

**A)** When the analytics retention period needs to be extended beyond 7 days
**B)** When analytics data is used to track API adoption rates and consumer growth trends
**C)** When telemetry buffering is enabled on Data Plane nodes
**D)** When analytics is used to demonstrate SLA compliance and revenue-driving API usage to business stakeholders
**E)** When the 5xx error rate exceeds an internal operations threshold

**Answer: B, D**
*Analytics elevates to business when it answers: "Which APIs drive value? Are SLAs being met? Is adoption growing?" — questions executives care about beyond pure operations.*

---

## Q10. Which two facts about retention and buffering are most likely to appear as direct exam questions?

**A)** Retention = 30 days; buffering is handled by the Control Plane
**B)** Retention = 7 days (default); telemetry is buffered locally on the Data Plane during connectivity loss
**C)** Retention = 24 hours; buffering requires Advanced Analytics to be enabled
**D)** Retention = 90 days; buffering is not supported in hybrid deployments

**Answer: B**
*The two exam-targeted facts: (1) **7-day default retention**, and (2) **local Data Plane telemetry buffering** during CP disconnect. Memorize both precisely.*

---

## Q11. A team wants a dashboard showing only error rates for three specific APIs across different environments. Which feature supports this?

**A)** Per-Control Plane ingestion control settings
**B)** Default retention period configuration
**C)** Custom dashboards — which let teams pick specific metrics, filter by API, and combine data from multiple environments
**D)** Telemetry buffering configuration on the Data Plane

**Answer: C**
*Custom dashboards allow teams to select specific metrics (error rate), filter by specific APIs, and combine data from multiple environments into one focused view.*

---

## Q12. Which statement about Konnect Advanced Analytics ingestion is TRUE?

**A)** Ingestion settings are managed at the geo level, not the Control Plane level
**B)** Ingestion can be enabled or disabled per individual gateway route
**C)** Ingestion is always enabled for all Control Planes and cannot be changed
**D)** Ingestion can be controlled at the Control Plane level, allowing organizations to choose which environments contribute analytics data

**Answer: D**
*Per-CP ingestion control is the key fact. Organizations can selectively enable analytics for specific Control Planes.*

---

## Q13. What is the difference between 4xx errors and 5xx errors in API Analytics?

**A)** 4xx errors indicate gateway infrastructure failures; 5xx errors indicate client-side mistakes
**B)** Both 4xx and 5xx indicate upstream service failures requiring the same investigation
**C)** 4xx errors are critical; 5xx errors are informational
**D)** 4xx errors are client-side errors (bad requests, unauthorized, not found); 5xx errors are server-side errors (upstream failures) — requiring different investigation paths

**Answer: D**
*4xx = client problem (check the request, auth, or route config). 5xx = server problem (check the upstream service health, gateway configuration, or backend logs).*

---

## Q14. How does Konnect Analytics support SLA compliance monitoring?

**A)** SLA compliance requires a third-party integration and cannot be done natively in Konnect
**B)** SLA compliance is tracked only in the Service Catalog scorecards, not Analytics
**C)** By providing latency percentiles (p50, p95, p99), error rates, and availability metrics — which can be compared against defined SLA thresholds to identify and report compliance
**D)** Konnect Analytics cannot track SLA metrics directly

**Answer: C**
*SLA monitoring requires latency and availability data. Analytics provides latency percentiles and error rates per API — the inputs needed to assess and report SLA compliance.*

---

## Q15. Why is per-Control Plane ingestion control important for data compliance?

**A)** It reduces the risk of unauthorized access to analytics dashboards across regions
**B)** It prevents analytics from storing sensitive PII in traffic logs regardless of region
**C)** It ensures only production data is included in compliance reports
**D)** For organizations with data residency requirements, it prevents analytics data from a specific regional Control Plane from being forwarded to an analytics system in a different region

**Answer: D**
*Data residency compliance may require that data from an EU Control Plane not be sent to a US analytics system. Per-CP ingestion control provides this granularity.*

---

## Q16. What does "traffic volume" as an analytics metric primarily tell you?

**Select TWO correct answers.**

**A)** The amount of data transferred between the Control Plane and Data Planes
**B)** The number of unique API consumers active in the current billing month
**C)** The number of API requests processed over a given time period
**D)** The current number of active Data Plane nodes handling requests
**E)** Trends useful for capacity planning and understanding API adoption growth

**Answer: C, E**
*Traffic volume = request count over time. It informs capacity planning (are we approaching limits?), growth tracking (is adoption increasing?), and anomaly detection (unexpected drops).*

---

## Q17. A platform team is notified that an API had zero traffic for 48 hours. Which Analytics capability helps investigate?

**A)** Scorecard review in the Service Catalog for documentation issues
**B)** Per-CP ingestion control settings to check whether data is being collected
**C)** Custom dashboards with time-series traffic volume graphs — showing exactly when traffic dropped and enabling correlation with deployment or configuration changes
**D)** Telemetry buffering logs from the Data Plane node

**Answer: C**
*Custom dashboards with time-series data let teams see the exact moment traffic dropped. Correlating with deployment events helps identify whether a misconfiguration caused the drop.*

---

## Q18. What is the purpose of "latency percentiles" (p50, p95, p99) in API analytics?

**A)** They indicate what percentage of requests are successfully authenticated
**B)** They are used to configure rate limiting thresholds based on response time
**C)** They measure what percentage of requests result in errors at each severity level
**D)** They describe the distribution of response times — p99 means 99% of requests completed faster than this value, making it useful for understanding worst-case latency

**Answer: D**
*Percentile latency captures the tail of the distribution. p99 shows what the slowest 1% of users experience — important for SLAs, as averages can hide severe outlier latency.*

---

## Q19. How does telemetry buffering protect the accuracy of analytics data?

**A)** It filters out test traffic from analytics data automatically
**B)** It prevents inaccurate data from entering the analytics system by validating each record
**C)** It compresses telemetry to reduce storage costs and improve retention
**D)** It ensures traffic data generated during a Control Plane disconnect is not lost — stored locally and replayed after reconnection, maintaining complete time-series data

**Answer: D**
*Without buffering, a CP disconnect creates gaps in analytics data. Buffering preserves the complete record — critical for accurate SLA reporting and incident post-mortems.*

---

## Q20. In a Konnect deployment with 10 Control Planes across 3 clouds and 2 on-premises environments, what is the key operational benefit of centralized analytics?

**A)** All Control Planes merge into a single Control Plane for analytics purposes
**B)** Analytics automatically scales Data Plane nodes based on traffic data from all environments
**C)** A single analytics dashboard provides visibility across all 10 environments simultaneously, eliminating the need to switch between separate monitoring tools for each environment
**D)** Centralized analytics reduces the total number of Control Planes needed across the deployment

**Answer: C**
*The core benefit at scale: one view across all environments. Without centralized analytics, monitoring 10 separate environments requires 10 separate tools.*

---

## Q21. What does a sustained increase in 4xx error rates typically suggest?

**A)** An upstream service is becoming overloaded and returning client errors
**B)** Rate limiting is too aggressive and should be adjusted upward
**C)** Clients are making incorrect requests — possibly due to API changes, expired credentials, changed authentication requirements, or misconfigured client applications
**D)** The Control Plane is propagating incorrect configuration to the Data Plane

**Answer: C**
*4xx = client-side problems. Sustained increases suggest something changed: the API contract, authentication requirements, or client applications were misconfigured.*

---

## Q22. Which Analytics feature helps identify which API consumer generates the most traffic or errors?

**Select TWO correct answers.**

**A)** Per-CP ingestion control scoped to individual consumers
**B)** Telemetry buffering reports that include consumer-level breakdowns
**C)** Consumer-level analytics — filtering traffic data by consumer identity to identify top consumers
**D)** Custom dashboard widgets scoped to consumer groups or individual consumer IDs
**E)** Dev Portal application registration logs

**Answer: C, D**
*Consumer-level analytics enables identifying high-traffic consumers, consumers generating errors, or consumers approaching rate limits — useful for billing, debugging, and capacity planning.*

---

## Q23. What is the operational significance of the 7-day default retention period?

**A)** Teams have 7 days to download analytics data before it is deleted permanently
**B)** After 7 days, analytics data is archived but remains accessible in a read-only compressed form
**C)** The 7-day period is the minimum; data is always retained for at least 7 days by default
**D)** Historical analysis beyond 7 days requires either extended retention configuration or integration with an external data warehouse — teams must plan for this if weekly trend comparisons are needed

**Answer: D**
*Default 7-day retention = short-term operational data. For trend analysis, capacity planning, or compliance requiring longer history, extended retention or external export is needed.*

---

## Q24. How can Konnect Analytics be used to justify API deprecation decisions?

**A)** Analytics automatically flags APIs for deprecation after 30 days of below-threshold traffic
**B)** Deprecation decisions are based on Service Catalog scorecard data, not Analytics
**C)** Analytics cannot provide sufficient data to support deprecation decisions safely
**D)** By showing traffic trends for an API over time — zero or declining traffic provides evidence of no active consumers, supporting safe deprecation without consumer impact

**Answer: D**
*Data-driven deprecation: analytics shows zero traffic → safe to deprecate. Traffic data prevents accidental deprecation of APIs with active (but infrequent) consumers.*

---

## Q25. What is the relationship between Konnect Analytics and third-party observability tools?

**A)** Konnect Analytics must be replaced with third-party tools for the platform to function correctly
**B)** Konnect Analytics is a built-in observability layer for API traffic; organizations complement it with third-party tools by forwarding gateway logs and metrics — the tools serve different layers of the stack
**C)** Third-party tools such as Datadog automatically replace Konnect Analytics for customers who use them
**D)** Konnect Analytics is only available when Datadog is configured as the primary monitoring backend

**Answer: B**
*Konnect Analytics = API-layer observability. Third-party tools (Datadog, Prometheus) = broader infrastructure and application monitoring. Complementary, not replacements.*

---

## Q26. Which metric is most useful for detecting a DDoS attack or traffic anomaly?

**A)** SLA compliance percentage across service tiers
**B)** Custom dashboard widget count across the organization
**C)** 5xx error rate from specific upstream services
**D)** Sudden, unusual spike in request volume from specific IP ranges or consumers, visible in traffic volume dashboards

**Answer: D**
*Traffic volume spikes from unexpected sources are the primary DDoS/scraping indicator. Analytics traffic dashboards provide visibility needed to detect and investigate anomalies.*

---

## Q27. Why is it important to distinguish between "gateway errors" and "upstream errors" in analytics?

**A)** Gateway errors are always more severe than upstream errors and require executive escalation
**B)** Only upstream errors appear in Konnect Analytics; gateway errors are in error.log only
**C)** They are the same type of error and require identical investigation and remediation steps
**D)** Gateway errors indicate problems with Kong configuration or network connectivity; upstream errors indicate problems with the backend service — the investigation paths are completely different

**Answer: D**
*Root cause routing: gateway 5xx = check plugin config, TLS, network to upstream. Backend 5xx = check backend service health, capacity, or application logic.*

---

## Q28. How does Konnect Analytics support capacity planning?

**A)** Analytics automatically provisions additional Data Plane nodes when traffic exceeds thresholds
**B)** Analytics only supports reactive capacity responses, not proactive planning
**C)** By providing traffic volume trends and peak load data, teams can forecast future capacity needs, identify seasonal patterns, and plan Data Plane scaling before demand exceeds current capacity
**D)** Capacity planning is handled by the Control Plane scheduler, not Analytics

**Answer: C**
*Analytics-driven capacity planning: look at growth trends and peak traffic → predict when current capacity will be insufficient → scale proactively before users experience degradation.*

---

## Q29. A team wants alerts when error rate on a critical API exceeds 1% for more than 5 minutes. Which approach should they use?

**A)** Configure a rate limiting plugin to block traffic when error rate exceeds 1%
**B)** Set the analytics retention period to 5 minutes to detect the issue faster
**C)** Use the Control Plane audit log to detect error rate threshold breaches
**D)** Use Konnect Analytics data with an integrated alerting tool (forward metrics to Datadog or Prometheus/Alertmanager) or configure native analytics-based alerts on error rate thresholds

**Answer: D**
*Analytics provides the data; alerting thresholds require Konnect's native alerting features or forwarding metrics to an external alerting system for proactive incident response.*

---

## Q30. Which statement about the Data Plane telemetry flow is TRUE?

**Select TWO correct answers.**

**A)** The Control Plane collects all telemetry from Data Planes before forwarding to the analytics system
**B)** Telemetry data flows from the analytics platform to the Data Planes to configure monitoring
**C)** Data Planes generate telemetry from proxied traffic and forward it to the Konnect analytics system
**D)** Telemetry is buffered locally on the Data Plane if connectivity to the analytics system is lost
**E)** Telemetry only flows during active traffic periods and stops when no requests are being processed

**Answer: C, D**
*The telemetry flow: Data Plane generates and forwards telemetry → ingested by Konnect analytics → buffered locally on DP during connectivity loss → flushed upon reconnect.*

---

## Q31. Which Kong plugin exposes gateway metrics in Prometheus format?

**A)** `prometheus` — it makes Kong metrics available at the `/metrics` endpoint on the Status API port (default 8100) for Prometheus scraping
**B)** `statsd` — it exposes a `/metrics` HTTP endpoint alongside its UDP push capability
**C)** `http-log` — it exposes gateway metrics in JSON format on a configurable port
**D)** `datadog` — it exposes metrics via a Prometheus-compatible pull endpoint

**Answer: A**
*The `prometheus` plugin makes Kong metrics available at `/metrics` on the Status API port, ready for Prometheus scraping. No other plugin provides a Prometheus-format pull endpoint.*

---

## Q32. What is the primary use case for the `http-log` plugin?

**A)** To emit StatsD metrics to a Graphite backend
**B)** To write structured logs to the local filesystem on the Data Plane node
**C)** To expose metrics for Prometheus scraping
**D)** To forward full request/response log payloads as HTTP POST calls to an external log aggregator such as Splunk, Elastic, or a custom SIEM

**Answer: D**
*`http-log` pushes structured JSON logs to any HTTP endpoint — the bridge from Kong to external log management platforms.*

---

## Q33. How does the OpenTelemetry plugin extend observability beyond Konnect Analytics?

**A)** OTel integration is only available with Service Mesh deployments, not API Gateway
**B)** OTel replaces Konnect Analytics entirely for teams using CNCF-compatible backends
**C)** OTel collects only error logs, not request traces
**D)** Kong's OpenTelemetry plugin forwards distributed traces (spans) to an OTel Collector, enabling end-to-end request tracing across Kong and upstream microservices in any OTel-compatible backend (Jaeger, Tempo, Datadog APM)

**Answer: D**
*The `opentelemetry` plugin emits spans carrying trace context across Kong and into upstream services — enabling distributed tracing that follows a request through the entire system.*

---

## Q34. A team wants to alert when P99 latency exceeds 500ms for more than 2 minutes. What is the recommended approach?

**A)** Enable telemetry buffering for latency data only on affected routes
**B)** Configure a `request-termination` plugin to block requests exceeding 500ms
**C)** Set the Analytics retention period to 2 minutes for faster threshold detection
**D)** Use the `prometheus` plugin to expose latency metrics, then configure alerting rules in Prometheus Alertmanager or Datadog/Grafana based on the `kong_latency_bucket` histogram

**Answer: D**
*Konnect Analytics provides visualization; external alerting requires exporting metrics. The `prometheus` plugin feeds latency histograms to Prometheus where Alertmanager rules trigger notifications.*

---

## Q35. What does the `statsd` plugin enable that the `prometheus` plugin does not?

**A)** Integration with the native Konnect Analytics platform
**B)** Consumer-level metric tagging unavailable in the Prometheus format
**C)** Higher-resolution latency metrics than Prometheus histograms provide
**D)** Pushing metrics to a StatsD-compatible server (Graphite, InfluxDB, Telegraf) using a push model — as opposed to Prometheus' pull/scrape model

**Answer: D**
*`statsd` pushes metrics to a StatsD listener. `prometheus` exposes metrics for a scraper to pull. The choice depends on the observability stack architecture.*

---

## Q36. How does the `datadog` plugin complement Konnect Analytics?

**A)** It requires a Datadog APM license before any metrics can be emitted
**B)** It replaces Konnect Analytics entirely for teams standardized on Datadog
**C)** It pulls analytics data from Konnect and forwards it to Datadog for enrichment
**D)** It sends Kong request metrics (latency, status codes, consumer tags) to a Datadog Agent via UDP, enriching Datadog dashboards with gateway-layer data alongside infrastructure and application metrics

**Answer: D**
*The `datadog` plugin emits StatsD-format metrics to a local Datadog Agent. This feeds Datadog's monitoring stack with Kong-specific signals.*

---

## Q37. Which combination provides the most complete three-pillar observability coverage (metrics, logs, traces)?

**A)** Konnect Analytics alone covers all three pillars natively
**B)** `file-log` plugin alone handles all three observability pillars
**C)** Konnect Analytics + `statsd` plugin covers metrics and logs sufficiently
**D)** `prometheus` plugin (metrics) + `http-log` plugin (logs) + `opentelemetry` plugin (traces) — each forwarded to appropriate backends

**Answer: D**
*Three pillars require three complementary plugins: metrics via `prometheus`, logs via `http-log` (or `file-log`/`syslog`), and distributed traces via `opentelemetry`.*
