# Section 8: Analytics

> **Exam Theme:** Centralized observability, ingestion controls, error monitoring, custom dashboards, retention, and telemetry buffering.

---

## Q1. What is meant by "centralized observability" in the Analytics section?

**A)** All gateway nodes send logs directly to a third-party tool like Grafana
**B)** Analytics aggregates traffic data, metrics, and error information from all Control Planes and Data Planes into a single Konnect-managed observability platform
**C)** Observability is only available on Dedicated Cloud Gateways
**D)** Centralized observability means all services must route through a single Data Plane for monitoring purposes

**Answer: B**
*Centralized = one place to observe all API traffic across all environments. Rather than checking each gateway or cloud separately, Konnect Analytics aggregates everything.*

---

## Q2. How is analytics ingestion controlled according to the guide?

**A)** Ingestion is always on and cannot be modified
**B)** Advanced Analytics ingestion can be enabled or disabled per Control Plane, giving teams control over which gateways contribute data to the analytics platform
**C)** Ingestion is controlled by the Data Plane independently of the Control Plane
**D)** Ingestion can only be controlled at the organization level, not per Control Plane

**Answer: B**
*Per-CP ingestion control allows organizations to selectively send analytics data — useful for cost management, compliance (not sending data from certain regions), or phased rollouts.*

---

## Q3. What does a sudden spike in 5xx errors most likely indicate?

**A)** A misconfiguration in the Control Plane
**B)** A client-side authentication error
**C)** An upstream service problem — the gateway is successfully routing traffic but the backend is returning server errors
**D)** A rate limiting policy is blocking too many requests

**Answer: C**
*5xx errors are server errors. In a gateway context, spikes typically mean the upstream service is failing (overloaded, crashed, misconfigured) — not a gateway issue.*

---

## Q4. Why are custom dashboards important in Konnect Analytics?

**A)** They replace the default metrics provided by Konnect
**B)** They allow teams to build views tailored to their specific KPIs — combining the metrics that matter most for their services, teams, or executives
**C)** They are required for Advanced Analytics to function
**D)** Custom dashboards automatically resolve errors detected in traffic data

**Answer: B**
*Out-of-the-box dashboards cover common cases. Custom dashboards let teams focus on their specific signals — per-service error rates, consumer adoption trends, SLA compliance, etc.*

---

## Q5. What is the default out-of-the-box analytics retention period?

**A)** 24 hours
**B)** 30 days
**C)** 7 days
**D)** 90 days

**Answer: C**
*The default analytics retention is **7 days**. This is a direct fact likely to be tested — know it precisely.*

---

## Q6. What happens to telemetry if a Data Plane loses connectivity to the Control Plane?

**A)** All telemetry data is permanently lost for the duration of the disconnect
**B)** Telemetry is buffered locally on the Data Plane and forwarded to the analytics system once connectivity is restored
**C)** The Data Plane stops proxying traffic to avoid generating untracked telemetry
**D)** Telemetry is forwarded directly to third-party tools, bypassing the Control Plane entirely

**Answer: B**
*Telemetry buffering ensures no observability gaps during transient network issues. Once the Data Plane reconnects, buffered data is flushed to the analytics platform.*

---

## Q7. Why should you avoid confusing analytics data with live API traffic handling?

**A)** Because analytics data and live traffic are processed by the same component
**B)** Because analytics is a reporting and observability function — it observes past traffic; live traffic handling is the real-time proxy function of the Data Plane. They are separate concerns
**C)** Because analytics data can be used to block traffic in real-time
**D)** Because live traffic is handled by the Control Plane, not the Data Plane

**Answer: B**
*Analytics = after-the-fact observability (what happened). Live traffic = real-time proxy (what is happening now). Mixing them up leads to incorrect architecture decisions.*

---

## Q8. How would you distinguish error monitoring from data ingestion controls?

**A)** Error monitoring controls which gateways send data; ingestion controls detect 5xx spikes
**B)** Error monitoring is about detecting and analyzing failure patterns in traffic (4xx, 5xx rates); ingestion controls determine whether and how analytics data is collected from a given Control Plane
**C)** They are the same feature with different names in the Konnect UI
**D)** Error monitoring is a third-party integration; ingestion control is a native Konnect feature

**Answer: B**
*Error monitoring = "what errors are happening in my traffic?" Ingestion control = "should this CP be sending analytics data, and at what rate?" Different layers, different purposes.*

---

## Q9. When would analytics be part of an executive or business-level discussion instead of only an operations discussion?

**A)** When the 5xx error rate exceeds a threshold
**B)** When analytics data is used to track API adoption rates, consumer growth, revenue-driving API usage trends, and SLA compliance — metrics relevant to business decisions
**C)** When the retention period needs to be extended
**D)** When telemetry buffering is enabled on Data Planes

**Answer: B**
*Analytics elevates from ops to business when it answers: "Which APIs drive the most value? Are SLAs being met? Is API adoption growing?" — questions executives care about.*

---

## Q10. Which two retention or buffering facts are most likely to show up as direct exam checks?

**A)** Retention = 30 days; buffering is not supported
**B)** Retention = 7 days (default); telemetry is buffered locally during Data Plane connectivity loss
**C)** Retention = 24 hours; buffering is handled by the Control Plane
**D)** Retention = 90 days; buffering requires Advanced Analytics to be enabled

**Answer: B**
*The two exam-targeted facts: (1) **7-day default retention**, and (2) **local telemetry buffering** during CP disconnect. Memorize both precisely.*

---

## Q11. A team wants to create a dashboard showing only the error rates for three specific APIs across different environments. Which Konnect Analytics feature supports this?

**A)** Per-Control Plane ingestion control
**B)** Telemetry buffering
**C)** Custom dashboards
**D)** Default retention settings

**Answer: C**
*Custom dashboards allow teams to pick specific metrics (error rate), filter by specific APIs, and combine data from multiple environments into one focused view.*

---

## Q12. Which statement about Konnect Advanced Analytics ingestion is TRUE?

**A)** Ingestion is always enabled for all Control Planes and cannot be changed
**B)** Ingestion can be enabled or disabled per individual gateway route
**C)** Ingestion can be controlled at the Control Plane level, allowing organizations to choose which environments contribute analytics data
**D)** Ingestion settings are managed at the geo level, not the Control Plane level

**Answer: C**
*Per-CP ingestion control is the key fact. Organizations can selectively enable analytics for specific Control Planes — useful for compliance, cost management, or staged rollouts.*

---

## Q13. What is the difference between 4xx errors and 5xx errors in the context of API Analytics?

**A)** 4xx errors indicate gateway infrastructure failures; 5xx errors indicate client mistakes
**B)** 4xx errors are client-side errors (bad requests, unauthorized, not found); 5xx errors are server-side errors (upstream failures, gateway errors) — they require different investigation paths
**C)** 4xx errors are critical; 5xx errors are informational
**D)** Both 4xx and 5xx indicate upstream service failures

**Answer: B**
*4xx = client problem (check the request, auth, or route config). 5xx = server problem (check the upstream service health, gateway configuration, or backend logs).*

---

## Q14. How does Konnect Analytics support SLA (Service Level Agreement) compliance monitoring?

**A)** Konnect Analytics cannot track SLA metrics
**B)** By providing latency percentiles (p50, p95, p99), error rates, and availability metrics — which can be compared against defined SLA thresholds to identify and report compliance
**C)** SLA compliance is tracked only in the Service Catalog, not Analytics
**D)** SLA monitoring requires a third-party integration and cannot be done natively

**Answer: B**
*SLA monitoring requires latency and availability data. Analytics provides latency percentiles and error rates per API — the inputs needed to assess and report SLA compliance.*

---

## Q15. Why is per-Control Plane ingestion control important for compliance?

**A)** It reduces the risk of unauthorized access to analytics dashboards
**B)** For organizations with data residency requirements, it allows them to prevent analytics data from a specific regional Control Plane from being sent to an analytics system in a different region
**C)** It ensures that only production data is included in compliance reports
**D)** It prevents analytics from storing sensitive PII in traffic logs

**Answer: B**
*Data residency compliance may require that data from an EU Control Plane not be sent to a US analytics system. Per-CP ingestion control provides this granularity.*

---

## Q16. What does "traffic volume" as an analytics metric tell you?

**A)** The total number of Data Plane nodes currently active
**B)** The number of API requests processed over a given time period — useful for capacity planning, identifying peak times, and understanding API usage growth trends
**C)** The amount of data transferred between the Control Plane and Data Planes
**D)** The number of unique API consumers active in the current month

**Answer: B**
*Traffic volume = request count over time. It informs capacity planning (are we approaching node limits?), growth tracking (is adoption increasing?), and anomaly detection (unexpected traffic drops).*

---

## Q17. A platform team is notified that an API had zero traffic for the past 48 hours, which is unusual. Which Analytics capability would help them investigate?

**A)** Scorecard review in the Service Catalog
**B)** Custom dashboards with time-series traffic volume graphs — allowing them to see exactly when traffic dropped and correlate with deployment or configuration changes
**C)** Telemetry buffering logs from the Data Plane
**D)** Per-CP ingestion control settings

**Answer: B**
*Custom dashboards with time-series data let teams see the exact moment traffic dropped. Correlating with deployment events helps identify whether a misconfiguration caused the drop.*

---

## Q18. What is the purpose of "latency percentiles" (p50, p95, p99) in API analytics?

**A)** They indicate what percentage of requests are authenticated
**B)** They describe the distribution of response times — p99 = 99% of requests completed faster than this threshold, making it useful for understanding worst-case latency experienced by users
**C)** They measure the percentage of requests that result in errors
**D)** They are used to configure rate limiting thresholds

**Answer: B**
*Percentile latency captures the tail of the distribution. p99 shows what the slowest 1% of users experience — important for SLAs, as averages can hide severe outlier latency.*

---

## Q19. How does telemetry buffering protect the accuracy of analytics data?

**A)** It prevents inaccurate data from entering the analytics system by validating each telemetry record
**B)** It ensures that traffic data generated during a Control Plane disconnect is not lost — it is stored locally and replayed after reconnection, maintaining complete time-series data
**C)** It compresses telemetry to reduce storage costs
**D)** It filters out test traffic from analytics data automatically

**Answer: B**
*Without buffering, a CP disconnect would create gaps in analytics data. Buffering preserves the complete record — critical for accurate SLA reporting and incident post-mortems.*

---

## Q20. In a Konnect deployment with 10 Control Planes across 3 clouds and 2 on-premises environments, what is the key operational benefit of centralized analytics?

**A)** All Control Planes merge into a single Control Plane for analytics purposes
**B)** A single analytics dashboard provides visibility across all 10 environments simultaneously, eliminating the need to switch between separate monitoring tools for each environment
**C)** Analytics automatically scales the Data Plane nodes based on traffic data from all environments
**D)** Centralized analytics reduces the number of Control Planes needed

**Answer: B**
*The core benefit at scale: one view across all environments. Without centralized analytics, monitoring 10 separate environments requires 10 separate tools — increasing complexity and cognitive load.*

---

## Q21. What does a sustained increase in 4xx error rates typically suggest about API traffic?

**A)** An upstream service is becoming overloaded
**B)** Clients are making incorrect requests — possibly due to API changes, expired credentials, changed authentication requirements, or misconfigured client applications
**C)** The Control Plane is propagating incorrect configuration
**D)** Rate limiting is too aggressive and should be loosened

**Answer: B**
*4xx = client-side problems. Sustained increases suggest something changed: the API contract, authentication requirements, or a client application was misconfigured or credentials expired.*

---

## Q22. Which Konnect Analytics feature would help identify which API consumer is generating the most traffic or errors?

**A)** Per-CP ingestion control
**B)** Consumer-level analytics — filtering and grouping traffic data by consumer identity to identify top consumers or problem clients
**C)** Telemetry buffering reports
**D)** Custom dashboard widgets for Control Plane health

**Answer: B**
*Consumer-level analytics enables identifying high-traffic consumers, consumers generating errors, or consumers at risk of hitting rate limits — useful for billing, debugging, and capacity planning.*

---

## Q23. What is the significance of the 7-day default retention period for operational planning?

**A)** Teams have 7 days to download analytics data before it is deleted permanently
**B)** Historical analysis beyond 7 days requires either a longer retention configuration or integration with an external data warehouse — teams must plan for this if weekly trend comparisons are needed
**C)** The 7-day period is the minimum; data is always retained for at least 7 days
**D)** After 7 days, analytics data is archived but remains accessible in read-only mode

**Answer: B**
*Default 7-day retention means out-of-the-box analytics is short-term operational data. For trend analysis, capacity planning, or compliance requiring longer history, extended retention or export is needed.*

---

## Q24. How can Konnect Analytics be used to justify API deprecation decisions?

**A)** Analytics cannot provide data to support deprecation decisions
**B)** By showing traffic trends for an API over time — zero or declining traffic provides evidence that the API has no active consumers, supporting a safe deprecation without consumer impact
**C)** By automatically flagging APIs for deprecation after 30 days of low traffic
**D)** Deprecation decisions are based on Service Catalog scorecard data, not analytics

**Answer: B**
*Data-driven deprecation: analytics shows zero traffic for an API → safe to deprecate. Showing stakeholders traffic data prevents accidental deprecation of APIs with active (but infrequent) consumers.*

---

## Q25. What is the relationship between Konnect Analytics and third-party observability tools like Datadog or Prometheus?

**A)** Konnect Analytics replaces all third-party observability tools
**B)** Konnect Analytics is a built-in observability layer for API traffic; organizations can complement it with third-party tools by forwarding gateway logs and metrics — the tools serve different layers of the observability stack
**C)** Konnect Analytics is only available when Datadog is configured as the primary monitoring tool
**D)** Third-party tools must be replaced with Konnect Analytics for the platform to function correctly

**Answer: B**
*Konnect Analytics = API-layer observability (requests, errors, latency). Third-party tools (Datadog, Prometheus) = broader infrastructure and application monitoring. They complement, not replace, each other.*

---

## Q26. Which metric would be most useful for detecting a DDoS attack or traffic anomaly in Konnect Analytics?

**A)** SLA compliance percentage
**B)** Sudden, unusual spike in request volume from specific IP ranges or consumers, visible in traffic volume dashboards
**C)** 5xx error rate from upstream services
**D)** Custom dashboard widget count

**Answer: B**
*Traffic volume spikes from unexpected sources are the primary indicator of DDoS or scraping attacks. Analytics traffic dashboards provide the visibility needed to detect and investigate anomalies.*

---

## Q27. Why is it important to distinguish between "gateway errors" (5xx generated by the gateway itself) and "upstream errors" (5xx returned by backend services) in analytics?

**A)** They are the same type of error and require the same investigation steps
**B)** Gateway errors indicate problems with Kong configuration, plugins, or network connectivity; upstream errors indicate problems with the backend service — the investigation and remediation paths are completely different
**C)** Gateway errors are always more severe than upstream errors
**D)** Only upstream errors appear in Konnect Analytics

**Answer: B**
*Root cause routing: gateway 5xx = check plugin config, TLS, network to upstream. Backend 5xx = check backend service health, capacity, or application logic. Conflating them wastes investigation time.*

---

## Q28. How does Konnect Analytics support capacity planning for API infrastructure?

**A)** Analytics automatically provisions additional Data Plane nodes when traffic exceeds thresholds
**B)** By providing traffic volume trends and peak load data, teams can forecast future capacity needs, identify seasonal traffic patterns, and plan Data Plane node scaling before demand exceeds current capacity
**C)** Capacity planning is handled by the Control Plane, not Analytics
**D)** Analytics only supports reactive capacity responses, not proactive planning

**Answer: B**
*Analytics-driven capacity planning: look at growth trends and peak traffic patterns → predict when current capacity will be insufficient → scale proactively before users experience degradation.*

---

## Q29. A team wants to receive alerts when the error rate on a critical API exceeds 1% for more than 5 minutes. Which approach should they use with Konnect Analytics?

**A)** Configure a rate limiting plugin to block traffic when error rate exceeds 1%
**B)** Use Konnect Analytics data with an integrated alerting tool (e.g., forward metrics to Datadog or Prometheus/Alertmanager) or configure analytics-based alerts to trigger notifications based on error rate thresholds
**C)** Set the analytics retention to 5 minutes to detect the issue faster
**D)** Use the Control Plane audit log to detect error rate thresholds

**Answer: B**
*Analytics provides the data; alerting thresholds require either Konnect's native alerting features or forwarding metrics to an external alerting system. The integration supports proactive incident response.*

---

## Q30. Which statement about Konnect Analytics and Data Plane telemetry flow is TRUE?

**A)** Data Planes send telemetry directly to the analytics platform, bypassing the Control Plane
**B)** The Control Plane collects all telemetry from Data Planes before forwarding it to the analytics system
**C)** Data Planes generate and forward telemetry data, which is ingested by the Konnect analytics system — the telemetry is buffered locally if connectivity is lost and flushed when restored
**D)** Telemetry data flows from the analytics platform to the Data Planes to configure monitoring

**Answer: C**
*The telemetry flow: Data Plane generates metrics and logs from proxied traffic → forwards to analytics system → buffered locally during connectivity loss → flushed upon reconnect.*

---

## Q31. Which Kong plugin exposes gateway metrics in Prometheus format for scraping by an external Prometheus server?

**A)** `http-log`
**B)** `statsd`
**C)** `prometheus`
**D)** `datadog`

**Answer: C**
*The `prometheus` plugin makes Kong metrics (request count, latency, upstream health, consumer usage) available at the `/metrics` endpoint on the Status API port (default 8100), ready for Prometheus scraping.*

---

## Q32. What is the primary use case for the `http-log` plugin in an observability stack?

**A)** To expose metrics for Prometheus scraping
**B)** To forward full request/response log payloads as HTTP POST calls to an external log aggregator (Splunk, Elastic, custom SIEM)
**C)** To write logs to the local filesystem
**D)** To emit StatsD metrics to Graphite

**Answer: B**
*`http-log` pushes structured JSON logs to any HTTP endpoint — making it the bridge from Kong to external log management platforms (Splunk, Datadog, Elastic, etc.).*

---

## Q33. How does OpenTelemetry (OTel) integration with Kong Gateway extend observability beyond Konnect Analytics?

**A)** OTel replaces Konnect Analytics entirely
**B)** Kong's OpenTelemetry plugin forwards distributed traces (spans) to an OTel Collector, enabling end-to-end request tracing across Kong and upstream microservices in any OTel-compatible backend (Jaeger, Tempo, Datadog APM)
**C)** OTel integration is only available with Service Mesh deployments
**D)** OTel collects only error logs, not request traces

**Answer: B**
*The `opentelemetry` plugin emits spans that carry trace context across Kong and into upstream services — enabling distributed tracing that follows a request through the entire system, not just the gateway hop.*

---

## Q34. A team wants to alert when P99 latency for a specific API exceeds 500ms for more than 2 minutes. What is the recommended approach with Konnect?

**A)** Configure a `request-termination` plugin to block slow requests
**B)** Use the `prometheus` plugin to expose latency metrics, then configure alerting rules in Prometheus Alertmanager (or Datadog/Grafana) based on the `kong_latency_bucket` metric
**C)** Set the Analytics retention period to 2 minutes
**D)** Enable telemetry buffering for latency data only

**Answer: B**
*Konnect Analytics provides visualization; external alerting requires exporting metrics. The `prometheus` plugin feeds latency histograms to Prometheus, where Alertmanager rules trigger notifications when SLA thresholds are breached.*

---

## Q35. What does the `statsd` plugin enable that the `prometheus` plugin does not?

**A)** Higher-resolution latency metrics
**B)** Pushing metrics to a StatsD-compatible server (Graphite, InfluxDB, Telegraf) using a push model, as opposed to Prometheus' pull/scrape model
**C)** Consumer-level metric tagging
**D)** Integration with the Konnect Analytics platform

**Answer: B**
*`statsd` pushes metrics to a StatsD listener. `prometheus` exposes metrics for a scraper to pull. The choice depends on the observability stack: push (statsd) vs. pull (prometheus).*

---

## Q36. How does the `datadog` plugin complement Konnect Analytics?

**A)** It replaces Konnect Analytics for Datadog users
**B)** It sends Kong request metrics (latency, status codes, consumer tags) to a Datadog Agent via UDP, enriching Datadog dashboards and alerts with gateway-layer data alongside infrastructure and application metrics
**C)** It pulls analytics data from Konnect and sends it to Datadog
**D)** It requires a Datadog APM license to function

**Answer: B**
*The `datadog` plugin emits StatsD-format metrics to a local Datadog Agent. This feeds Datadog's monitoring stack with Kong-specific signals — useful for teams that centralize all observability in Datadog.*

---

## Q37. In a Konnect deployment, which combination provides the most complete observability coverage across the three pillars (metrics, logs, traces)?

**A)** Konnect Analytics alone
**B)** `prometheus` plugin (metrics) + `http-log` plugin (logs) + `opentelemetry` plugin (traces) — each forwarded to appropriate backends
**C)** `file-log` plugin (all three pillars)
**D)** Konnect Analytics + `statsd` plugin

**Answer: B**
*The three observability pillars require three complementary plugins: metrics via `prometheus`, logs via `http-log` (or `file-log`/`syslog`), and distributed traces via `opentelemetry`. Konnect Analytics covers the metrics/dashboards layer natively.*
