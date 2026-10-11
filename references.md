# References and Further Reading

Companion bibliography for [monitoring_logging_ai_systems.md](monitoring_logging_ai_systems.md) and [monitoring_logging_ai_systems.ipynb](monitoring_logging_ai_systems.ipynb).

All links were checked on **11 October 2026**. Official documentation is preferred throughout. Pricing and free-tier details change frequently; the verification date is recorded wherever such details are cited.

---

## 1. Observability Foundations

| Resource | Why it is useful |
|---|---|
| [OpenTelemetry documentation](https://opentelemetry.io/docs/) | Entry point to the vendor-neutral standard for traces, metrics, and logs. |
| [OpenTelemetry: Observability primer](https://opentelemetry.io/docs/concepts/observability-primer/) | Concise definitions of observability, telemetry, reliability, SLIs, SLOs, logs, spans, and distributed traces. Recommended first read. |
| [OpenTelemetry: Traces](https://opentelemetry.io/docs/concepts/signals/traces/) | Span structure, attributes, events, status, and context propagation in detail. |
| [OpenTelemetry: Metrics](https://opentelemetry.io/docs/concepts/signals/metrics/) | Metric instruments (counters, gauges, histograms) and aggregation. |
| [OpenTelemetry: Logs](https://opentelemetry.io/docs/concepts/signals/logs/) | How logs relate to traces and how to correlate them. |
| [OpenTelemetry: Baggage](https://opentelemetry.io/docs/concepts/signals/baggage/) | Cross-service context propagation, and why sensitive values must not be placed in it. |
| [OpenTelemetry Collector](https://opentelemetry.io/docs/collector) | The component used in production to receive, process (redact, sample), and export telemetry. |
| [W3C Trace Context](https://www.w3.org/TR/trace-context/) | The standard for trace and span ID formats and the `traceparent` header. |

## 2. GenAI Instrumentation Conventions

| Resource | Why it is useful |
|---|---|
| [OpenTelemetry GenAI semantic conventions repository](https://github.com/open-telemetry/semantic-conventions-genai) | Standard attribute names, spans, metrics, and events for GenAI clients and agents. These conventions moved to this dedicated repository; the previous page on opentelemetry.io now redirects readers here. Still evolving. |
| [OpenInference (GitHub)](https://github.com/Arize-ai/openinference) | Conventions and auto-instrumentation libraries for AI applications, complementary to OpenTelemetry. Includes instrumentation for Google GenAI, OpenAI, Anthropic, LangChain, LlamaIndex, and others. |
| [OpenInference specification](https://arize-ai.github.io/openinference/spec/) | The attribute specification used by OpenInference instrumentations. |

## 3. Reliability Engineering, SLOs, and Alerting

| Resource | Why it is useful |
|---|---|
| [Google SRE Book, Ch. 6: Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/) | The four golden signals, symptoms versus causes, tail latency, and principles for actionable alerting. Foundational. |
| [Google SRE Book, Ch. 4: Service Level Objectives](https://sre.google/sre-book/service-level-objectives/) | Definitions and selection of SLIs, SLOs, and SLAs. |
| [Google SRE Workbook: Implementing SLOs](https://sre.google/workbook/implementing-slos/) | Practical guidance on choosing SLIs as ratios of good events to total events. |
| [Google SRE Workbook: Alerting on SLOs](https://sre.google/workbook/alerting-on-slos/) | Error budgets, burn rate, and multi-window multi-burn-rate alerting, including guidance for low-traffic services. |

## 4. LLM Observability Platforms

### Langfuse (used in the notebook)

| Resource | Why it is useful |
|---|---|
| [Langfuse documentation](https://langfuse.com/docs) | Overview of tracing, prompt management, evaluation, and metrics. |
| [Langfuse SDK overview](https://langfuse.com/docs/observability/sdk/overview) | Installation, credentials (`LANGFUSE_BASE_URL`, keys), and how Langfuse maps to OpenTelemetry concepts. |
| [Langfuse instrumentation](https://langfuse.com/docs/observability/sdk/instrumentation) | Context managers, decorators, nesting, `propagate_attributes`, trace IDs, and flushing. The notebook follows these patterns. |
| [Langfuse observation types](https://langfuse.com/docs/observability/features/observation-types) | Span, generation, retriever, tool, agent, evaluator, guardrail, and other typed observations. |
| [Langfuse data model](https://langfuse.com/docs/observability/data-model) | Traces, observations, sessions, and scores. |
| [Langfuse scores via SDK](https://langfuse.com/docs/evaluation/evaluation-methods/scores-via-sdk) | Attaching numeric, categorical, and boolean evaluation scores to traces (used in Scenario D). |
| [Langfuse LLM-as-a-judge](https://langfuse.com/docs/evaluation/evaluation-methods/llm-as-a-judge) | Configuring model-based evaluators on traces. |
| [Langfuse masking](https://langfuse.com/docs/observability/features/masking) | Redacting sensitive data before it leaves the application. |
| [Langfuse sampling](https://langfuse.com/docs/observability/features/sampling) | Client-side trace sampling to control volume. |
| [Langfuse Python SDK v3 → v4 migration](https://langfuse.com/docs/observability/sdk/upgrade-path/python-v3-to-v4) | Important if you find older code examples online; v4 renamed several methods. |
| [Langfuse self-hosting](https://langfuse.com/self-hosting) | Running Langfuse in your own infrastructure. |
| [Langfuse pricing](https://langfuse.com/pricing) | Plan comparison. **Verified 11 Oct 2026:** Hobby plan free, no credit card, 50k units/month, 30 days data access, 2 users. |

### Other platforms

| Resource | Why it is useful |
|---|---|
| [Arize Phoenix documentation](https://arize.com/docs/phoenix) | Open-source tracing, evaluation, datasets, experiments, and prompt iteration built on OpenTelemetry and OpenInference. |
| [LangSmith documentation](https://docs.langchain.com/langsmith/home) | Tracing, debugging, and evaluation, with tight LangChain/LangGraph integration. |
| [LangSmith evaluation concepts](https://docs.langchain.com/langsmith/evaluation-concepts) | Clear treatment of offline versus online evaluation, evaluator types, reference-free versus reference-based evaluation, and dataset practices. Useful regardless of platform. |
| [Datadog LLM Observability](https://docs.datadoghq.com/llm_observability/) | AI tracing integrated with enterprise APM, infrastructure, and security monitoring (presented as "Agent Observability" at verification time). |

## 5. Cloud-Native Observability

| Resource | Why it is useful |
|---|---|
| [Azure Monitor documentation](https://learn.microsoft.com/en-us/azure/azure-monitor/) | Application Insights, Log Analytics, metrics, alerts, and OpenTelemetry enablement on Azure. |
| [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) | Metrics, logs, traces, SLOs, native OTLP endpoints, and AI-agent monitoring features on AWS. |
| [Google Cloud Observability](https://cloud.google.com/stackdriver/docs) | Cloud Monitoring, Logging, and Trace on Google Cloud. |

## 6. Gemini API (used in the notebook)

| Resource | Why it is useful |
|---|---|
| [Gemini API documentation](https://ai.google.dev/gemini-api/docs) | Entry point. |
| [Get an API key (Google AI Studio)](https://aistudio.google.com/apikey) | Create the key used in the notebook. |
| [Interactions API overview](https://ai.google.dev/gemini-api/docs/interactions-overview) | The recommended API for new projects (generally available since June 2026), including `store=false` and data-retention behaviour. |
| [Text generation](https://ai.google.dev/gemini-api/docs/text-generation) | `system_instruction`, `generation_config`, and response helpers such as `output_text`. |
| [Understand and count tokens](https://ai.google.dev/gemini-api/docs/tokens) | Token counting and the `usage` fields reported per interaction. |
| [Models](https://ai.google.dev/gemini-api/docs/models) | Current model names; check here if the notebook's default model becomes unavailable. |
| [Pricing](https://ai.google.dev/gemini-api/docs/pricing) | **Verified 11 Oct 2026:** free tier available for listed models including `gemini-3.5-flash-lite`; free-tier content is used to improve Google products; paid list price for `gemini-3.5-flash-lite` USD 0.30 input / USD 2.50 output per 1M tokens. |
| [Rate limits](https://ai.google.dev/gemini-api/docs/rate-limits) | Limits apply per project and vary by model and tier; view active limits in AI Studio. |
| [API errors](https://ai.google.dev/gemini-api/docs/api-errors) | Error codes (for example 404 `model_not_found`, 429 `rate_limit_exceeded`) referenced in the troubleshooting table. |
| [Gemini API terms](https://ai.google.dev/gemini-api/terms) | Terms governing data use; read before sending any non-public data. |

## 7. Evaluation of LLM and RAG Systems

| Resource | Why it is useful |
|---|---|
| [LangSmith evaluation concepts](https://docs.langchain.com/langsmith/evaluation-concepts) | Vendor-agnostic framing of evaluation methods and lifecycle (listed above; repeated here for convenience). |
| [Ragas documentation](https://docs.ragas.io/) | Open-source library for evaluating LLM and RAG applications, including retrieval and faithfulness metrics. |
| [Arize Phoenix: evaluation](https://arize.com/docs/phoenix) | LLM-based, code-based, and human evaluation tied to traces. |

## 8. Security, Privacy, and Governance

| Resource | Why it is useful |
|---|---|
| [OWASP Top 10 for LLM and GenAI Applications](https://genai.owasp.org/llm-top-10/) | The 2025 list of major LLM application risks and mitigations. |
| [LLM02:2025 Sensitive Information Disclosure](https://genai.owasp.org/llmrisk/llm022025-sensitive-information-disclosure/) | Directly relevant to what is captured in prompts, responses, and traces. |
| [LLM06:2025 Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/) | Relevant to agent monitoring and limits. |
| [LLM07:2025 System Prompt Leakage](https://genai.owasp.org/llmrisk/llm072025-system-prompt-leakage/) | Relevant to who can view system prompts in telemetry. |
| [LLM10:2025 Unbounded Consumption](https://genai.owasp.org/llmrisk/llm102025-unbounded-consumption/) | Relevant to token budgets, rate limits, and cost monitoring. |
| [OpenTelemetry: Handling sensitive data](https://opentelemetry.io/docs/security/handling-sensitive-data/) | Data minimisation and Collector processors for deleting, hashing, and redacting attributes; caveats on hashing. |
| [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) | Voluntary framework for managing AI risks across the lifecycle. |
| [NIST AI 600-1: Generative AI Profile](https://doi.org/10.6028/NIST.AI.600-1) | GenAI-specific risks and suggested actions, complementing the AI RMF. |

## 9. Suggested Reading Order

For readers new to the topic:

1. OpenTelemetry Observability primer
2. Google SRE Book, Chapter 6
3. Sections 1–5 of the learning module
4. LangSmith evaluation concepts
5. Google SRE Workbook, Alerting on SLOs
6. OWASP LLM02 and OpenTelemetry's sensitive-data guidance
7. The documentation of whichever platform your organisation is evaluating
