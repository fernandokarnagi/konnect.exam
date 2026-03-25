# Section 4: AI Gateway

> **Exam Theme:** Provider-agnostic AI traffic management, prompt governance, semantic caching, prompt optimization, observability, and secrets management.

---

## Q1. What is the core purpose of AI Gateway, and what is it explicitly NOT meant for?

**A)** AI Gateway is for hosting and training AI models; it is not for routing API traffic
**B)** AI Gateway is for governing, securing, and observing AI traffic through a unified proxy layer; it is NOT a model-hosting or model-training platform
**C)** AI Gateway is a replacement for Kong Gateway when working with LLMs
**D)** AI Gateway is exclusively for OpenAI integrations and does not support other providers

**Answer: B**
*AI Gateway sits in front of AI providers as a governance and observability proxy. It does not host, run, or train models — that is the responsibility of the AI provider.*

---

## Q2. Why is the "single, unified API layer" important in the AI Gateway section?

**A)** It reduces the number of LLM tokens consumed per request
**B)** It means all AI provider calls flow through one consistent entry point, enabling centralized governance, observability, and policy enforcement regardless of which provider is used
**C)** It eliminates the need for authentication on AI endpoints
**D)** It caches all AI responses by default to avoid repeated calls

**Answer: B**
*A single API layer means teams don't need separate integrations for each AI provider. Policies, logging, and security controls are applied uniformly at the gateway layer.*

---

## Q3. What does the provider-agnostic design prevent from a platform strategy perspective?

**A)** It prevents developers from using more than one AI model at a time
**B)** It prevents the need for any authentication on AI traffic
**C)** It prevents vendor lock-in by allowing organizations to switch or combine AI providers without rewriting application code
**D)** It prevents AI providers from accessing request payloads

**Answer: C**
*Provider-agnostic design means the application talks to one consistent interface (AI Gateway). Swapping or adding AI providers is a configuration change, not a code change.*

---

## Q4. When should AI Prompt Guard be used?

**A)** When you need to compress long prompts to reduce token usage
**B)** When you want to enforce rules that block or flag specific prompt patterns — such as jailbreak attempts, harmful content, or policy violations
**C)** When you need to route prompts to a specific AI provider based on cost
**D)** When you want to inject additional context into prompts automatically

**Answer: B**
*Prompt Guard is a governance plugin. It inspects incoming prompts against defined rules and blocks or flags requests that violate content or security policies.*

---

## Q5. When should AI PII Sanitizer be used?

**A)** When you need to cache semantically similar prompts
**B)** When you want to rate-limit AI requests per user
**C)** When prompts may contain personally identifiable information that should be detected and redacted before the prompt reaches the AI provider
**D)** When you want to compress prompts to reduce latency

**Answer: C**
*AI PII Sanitizer scans prompts for PII (names, email addresses, credit card numbers, etc.) and redacts them before forwarding to the upstream AI model.*

---

## Q6. How does semantic caching reduce cost without requiring identical prompt text?

**A)** It stores exact prompt/response pairs and only serves cached results for byte-for-byte identical prompts
**B)** It uses vector similarity to detect prompts that are semantically equivalent and returns cached responses without making a new call to the AI provider
**C)** It compresses prompts to reduce the token count sent to the provider
**D)** It batches multiple prompts into a single API call to reduce request overhead

**Answer: B**
*Semantic caching uses embeddings and vector similarity. If a new prompt is semantically close to a cached one, the cached response is returned — no new LLM call, no new token cost.*

---

## Q7. Which prompt management capabilities are called out in the guide, and what do they help optimize?

**A)** Prompt encryption, prompt versioning, and prompt rollback
**B)** Prompt decoration (injecting context), prompt compression (reducing token count), and RAG-based augmentation (adding retrieved knowledge)
**C)** Prompt caching, prompt routing, and prompt auditing
**D)** Prompt load balancing and prompt rate limiting only

**Answer: B**
*The three prompt optimization features are: decoration (add context/instructions), compression (reduce token usage), and RAG integration (inject retrieved knowledge into prompts).*

---

## Q8. What observability options are mentioned for monitoring AI traffic?

**A)** Only Prometheus metrics
**B)** Only Kong's built-in logging plugin
**C)** Token usage tracking, request/response logging, and integration with external observability platforms
**D)** Real-time model performance monitoring and GPU utilization metrics

**Answer: C**
*AI Gateway observability covers token consumption (cost visibility), full request/response logging (audit/debug), and forwarding telemetry to external observability tools.*

---

## Q9. Why does the Konnect Config Store matter in AI Gateway scenarios?

**A)** It stores the vector embeddings used by semantic caching
**B)** It provides a secure vault for storing AI provider API keys and secrets, which are referenced in plugin configuration without being exposed in plain text
**C)** It manages the AI model weights and inference configuration
**D)** It stores the cached AI responses used by semantic caching

**Answer: B**
*Config Store (secrets management) keeps sensitive credentials — like AI provider API keys — out of plugin configuration files. Plugins reference the secret by name, not by value.*

---

## Q10. How would you explain the difference between AI governance features and AI performance optimization features?

**A)** Governance features reduce cost; performance features improve security
**B)** Governance features (Prompt Guard, PII Sanitizer) control what goes through the gateway for safety and compliance; performance features (semantic caching, prompt compression) reduce latency and token costs
**C)** There is no meaningful distinction — all AI plugins serve both purposes equally
**D)** Performance features apply only to responses; governance features apply only to prompts

**Answer: B**
*Governance = safety and compliance controls (block bad prompts, redact PII). Performance = efficiency and cost optimization (cache similar queries, reduce token count).*

---

## Q11. A team is building an application that calls both OpenAI and Anthropic models. Which AI Gateway capability most directly addresses switching or combining providers without code changes?

**A)** Semantic caching
**B)** Provider-agnostic single API layer
**C)** AI PII Sanitizer
**D)** Prompt decoration

**Answer: B**
*The provider-agnostic layer means the application sends requests to one endpoint (AI Gateway). Kong routes to the configured provider — switching providers is a gateway config change.*

---

## Q12. Which of the following BEST describes the role of AI Prompt Guard in a production AI deployment?

**A)** It improves response quality by enriching prompts with additional context
**B)** It ensures AI responses are grammatically correct before returning to the client
**C)** It acts as a content policy enforcement layer, detecting and blocking prompts that violate defined rules such as jailbreak attempts or prohibited content
**D)** It caches frequent prompts to reduce AI provider costs

**Answer: C**
*Prompt Guard is a policy enforcement plugin — it inspects prompts against security and content rules before they reach the model, acting as a first line of AI safety defense.*

---

## Q13. A legal team requires that no personal data (names, emails, social security numbers) be sent to external AI models. Which AI Gateway plugin directly addresses this requirement?

**A)** AI Prompt Guard
**B)** AI PII Sanitizer — which detects and redacts personally identifiable information from prompts before they are forwarded to the AI provider
**C)** Semantic caching
**D)** Config Store

**Answer: B**
*PII Sanitizer is specifically designed to detect and redact personal data in prompts. This directly satisfies the legal requirement of not sending PII to external AI providers.*

---

## Q14. What is "prompt decoration" in the context of AI Gateway optimization?

**A)** Adding visual formatting to AI-generated responses
**B)** Injecting system instructions, context, or additional information into a prompt before sending it to the AI provider — without the client application needing to include this context itself
**C)** Compressing a prompt by removing redundant tokens
**D)** Caching a prompt's response for future similar requests

**Answer: B**
*Prompt decoration = enriching prompts at the gateway layer. The application sends a simple prompt; the gateway decorates it with system context, role definitions, or additional instructions.*

---

## Q15. How does prompt compression help reduce AI operational costs?

**A)** It caches responses to avoid duplicate AI calls
**B)** It routes prompts to the cheapest available AI provider automatically
**C)** It reduces the token count of prompts before sending them to the AI provider, directly reducing the cost charged per-token by the provider
**D)** It batches multiple prompts into one API call

**Answer: C**
*Most AI providers charge per token. Prompt compression shrinks the prompt without losing semantic meaning, resulting in fewer tokens billed per request.*

---

## Q16. What is the relationship between AI Gateway and Kong Gateway in the Konnect platform?

**A)** AI Gateway replaces Kong Gateway when AI workloads are involved
**B)** AI Gateway is a specialized extension of Kong Gateway with AI-specific plugins — the same gateway runtime handles both regular API traffic and AI traffic
**C)** AI Gateway runs as a separate, independent service unrelated to Kong Gateway
**D)** AI Gateway is only available on Dedicated Cloud Gateways

**Answer: B**
*AI Gateway capabilities are delivered through Kong's plugin system on top of the same Kong Gateway runtime. It's not a separate product — it's AI-specific plugins on the Kong platform.*

---

## Q17. Which of the following scenarios would semantic caching provide the MOST value for cost reduction?

**A)** An API that serves unique, never-repeated requests (e.g., real-time financial data queries)
**B)** A customer support chatbot where many users ask variations of the same common questions (e.g., "What are your business hours?", "When are you open?")
**C)** A gateway that processes batch data transformations
**D)** An API with highly sensitive PII data that cannot be cached

**Answer: B**
*Semantic caching shines when similar questions recur. If many users ask semantically equivalent questions, the cached response is served without hitting the LLM — maximizing cost savings.*

---

## Q18. Why is "provider-agnostic" specifically called out as an important AI Gateway property?

**A)** Because it means the gateway works without any AI provider credentials
**B)** Because the AI provider market is rapidly evolving — organizations need the flexibility to adopt new models or providers without reengineering their applications
**C)** Because it means the gateway does not need to understand the content of AI requests
**D)** Because it prevents any single AI provider from becoming the primary model vendor

**Answer: B**
*The AI provider landscape is fragmented and fast-moving. Provider-agnostic design future-proofs the platform — adopt Claude today, add Gemini tomorrow, switch from GPT-4 next year — without touching application code.*

---

## Q19. In an AI Gateway deployment, where should API keys for external AI providers be stored?

**A)** Hardcoded in the Kong plugin configuration YAML
**B)** In environment variables on each Data Plane node
**C)** In the Konnect Config Store, referenced by name in plugin configuration
**D)** In the Dev Portal application credentials

**Answer: C**
*Config Store is the secrets management solution. AI provider keys stored there are never exposed in plain text in configuration files — plugins reference secrets by name only.*

---

## Q20. What does RAG stand for in the context of AI Gateway prompt optimization, and what does it do?

**A)** Rate And Govern — it enforces rate limiting on AI requests
**B)** Retrieval-Augmented Generation — it retrieves relevant knowledge from external sources and injects it into the prompt before sending to the AI provider, improving response accuracy
**C)** Routing And Governance — it routes prompts to the most appropriate AI provider
**D)** Response Aggregation Gateway — it combines responses from multiple AI providers

**Answer: B**
*RAG = Retrieval-Augmented Generation. At the gateway layer, this means the gateway retrieves context (from a vector database or knowledge source) and augments the prompt before it reaches the LLM.*

---

## Q21. A team wants to track how many tokens are consumed per API consumer per day for billing and cost allocation purposes. Which AI Gateway capability enables this?

**A)** Semantic caching
**B)** Prompt Guard
**C)** AI observability — token usage tracking per consumer, which can be reported and exported
**D)** Config Store

**Answer: C**
*Token usage tracking is an observability feature of AI Gateway. It records per-request token consumption, enabling per-consumer cost reporting and chargeback.*

---

## Q22. Which of the following is the correct order of AI governance checks that should happen before a prompt reaches the AI provider?

**A)** AI provider → PII Sanitizer → Prompt Guard → Client
**B)** Client → AI provider → Prompt Guard → PII Sanitizer
**C)** Client → Prompt Guard (block bad prompts) → PII Sanitizer (redact PII) → AI provider
**D)** Client → semantic cache check → AI provider → Prompt Guard

**Answer: C**
*Best practice order: (1) Prompt Guard blocks policy-violating prompts, (2) PII Sanitizer redacts personal data, (3) clean prompt forwarded to AI provider. Both operate before the LLM sees the data.*

---

## Q23. How does AI Gateway handle a request when semantic caching finds a sufficiently similar cached prompt?

**A)** It forwards the request to the AI provider and compares the new response to the cached one
**B)** It returns the cached response immediately without making a call to the AI provider, saving both latency and cost
**C)** It returns the cached response but also makes an asynchronous call to the AI provider to refresh the cache
**D)** It routes the request to a cheaper AI provider instead of using the cache

**Answer: B**
*Cache hit = immediate response from cache, no provider call. This is the core cost and latency benefit of semantic caching.*

---

## Q24. What type of similarity does semantic caching use to determine if a new prompt matches a cached prompt?

**A)** Exact byte-for-byte string matching
**B)** Keyword frequency matching (TF-IDF)
**C)** Vector similarity — comparing the embedding of the new prompt to embeddings of cached prompts
**D)** Regular expression pattern matching

**Answer: C**
*Semantic caching converts prompts to vector embeddings. A new prompt's embedding is compared to cached embeddings using a similarity threshold — semantically similar prompts match even with different wording.*

---

## Q25. An organization wants to ensure that AI-generated responses are also scanned before being returned to users. Which AI Gateway capability applies to the response path?

**A)** Prompt Guard only applies to requests, not responses
**B)** AI Response Guardrails — extending governance plugins to also inspect and filter AI-generated responses for harmful, sensitive, or policy-violating content
**C)** PII Sanitizer only applies to prompts, not responses
**D)** Semantic caching filters responses automatically

**Answer: B**
*Governance applies bidirectionally. Response guardrails inspect the AI provider's output before it reaches the user — catching harmful content or inadvertent PII in model responses.*

---

## Q26. How does Kong's Config Store improve the security posture of an AI Gateway deployment compared to storing secrets in plugin configuration directly?

**A)** Config Store encrypts secrets with AES-256 automatically, while direct configuration stores them in base64 only
**B)** Secrets in Config Store are never included in configuration exports, audit logs, or API responses — only their reference names are visible, minimizing secret exposure risk
**C)** Config Store stores secrets on the Data Plane so they are available without Control Plane connectivity
**D)** Config Store automatically rotates API keys on a schedule

**Answer: B**
*The security value: Config Store decouples secret values from configuration. Even if configuration files, decK state files, or API responses are leaked, the actual secret values are not exposed.*

---

## Q27. A company uses multiple AI providers: OpenAI for text generation, Cohere for embeddings, and Anthropic for analysis. They want all three to appear as a single internal API. Which AI Gateway feature supports this architecture?

**A)** Semantic caching — caching responses from all three providers
**B)** Provider-agnostic routing — AI Gateway can route different request types to different providers behind a single unified API endpoint
**C)** Prompt decoration — adding provider-specific context to each request
**D)** AI PII Sanitizer — sanitizing prompts before they reach any provider

**Answer: B**
*Provider-agnostic routing allows a single AI Gateway endpoint to fan out to different underlying providers based on routing rules — presenting one API surface to internal teams.*

---

## Q28. What is the primary risk that AI Prompt Guard mitigates in a production AI application?

**A)** Excessive token consumption from long prompts
**B)** PII leakage to external AI providers
**C)** Adversarial prompt injection, jailbreaks, and policy-violating content reaching the AI model
**D)** Vendor lock-in to a single AI provider

**Answer: C**
*Prompt Guard is the defense against prompt injection attacks, jailbreak attempts, and content that violates organizational policy — it's the security firewall for AI inputs.*

---

## Q29. Why is AI Gateway observability particularly important for FinOps (cloud financial operations) teams?

**A)** Because AI Gateway handles financial transactions
**B)** Because token usage directly maps to AI provider costs — observability data on token consumption per consumer, per endpoint, or per time period enables cost attribution and optimization
**C)** Because observability data is required to configure semantic caching
**D)** Because FinOps teams manage the Config Store secrets

**Answer: B**
*LLM costs are token-based. FinOps teams need per-consumer, per-service token usage data to allocate costs, identify waste, and optimize AI spend — exactly what AI Gateway observability provides.*

---

## Q30. Which combination of AI Gateway features would an enterprise need to satisfy both security compliance (no PII to external models) and cost optimization (reduce redundant LLM calls)?

**A)** Prompt Guard + Config Store
**B)** AI PII Sanitizer (compliance) + Semantic Caching (cost optimization)
**C)** Prompt compression + Prompt decoration
**D)** Token usage tracking + RAG augmentation

**Answer: B**
*PII Sanitizer = security compliance (redact personal data before it leaves the org). Semantic Caching = cost optimization (return cached responses for similar queries). Together they address both concerns.*
