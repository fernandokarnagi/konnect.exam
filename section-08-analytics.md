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
