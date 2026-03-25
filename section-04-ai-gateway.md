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
