# Monitoring and Logging AI Systems — Workshop Materials

Learning materials and a live demonstration for a 90-minute, theory-led session in the **Advanced Gen AI** programme.

## Contents

| File | Purpose |
|---|---|
| [monitoring_logging_ai_systems.md](monitoring_logging_ai_systems.md) | **Primary learning resource.** Self-contained module covering concepts, metrics, RAG and agent monitoring, tools, debugging methodology, alerting, governance, architecture, and checklists. Suitable for self-study before or after the session. |
| [monitoring_logging_ai_systems.ipynb](monitoring_logging_ai_systems.ipynb) | **Live demonstration** for Google Colab. A small RAG-style support assistant calls a real LLM (Gemini) and sends real traces to Langfuse. Four reproducible scenarios: baseline, long input, controlled failures, and a quality-evaluation failure. |
| [references.md](references.md) | Annotated bibliography of official documentation, grouped by topic, with verification dates for pricing and free-tier claims. |
| [facilitator_guide.md](facilitator_guide.md) | Separate guide for whoever runs the session: agenda, timings, demo script, anticipated questions, and contingency plan. Not needed for self-study. |
| [requirements.txt](requirements.txt) | Python dependencies used by the notebook. |
| [problem-statement.md](problem-statement.md) | Original planning brief. |

## Demonstration Stack

| Component | Choice | Why | Cost (verified 11 Oct 2026) |
|---|---|---|---|
| Runtime | Google Colab | No local installation | Free tier sufficient |
| LLM | Gemini Developer API, `gemini-3.5-flash-lite`, via the Interactions API (`google-genai` SDK) | Genuine free tier for this model; simple SDK | Free tier: no charge. Free-tier content is used by Google to improve its products. |
| Observability | Langfuse Cloud, Python SDK v4 (built on OpenTelemetry) | Free managed plan, LLM-specific trace views, scores | Hobby plan: free, no credit card, 50k units/month, 30-day data access |
| Local analysis | pandas, matplotlib | Already in Colab | Free |

The full notebook run generates roughly 7 traces with 5–6 observations each plus 2 scores, so well under 100 Langfuse billable units.

> **Important —** Free tiers, model availability, and limits change. Re-check the [Gemini pricing page](https://ai.google.dev/gemini-api/docs/pricing), your [AI Studio rate limits](https://aistudio.google.com/rate-limit), and [Langfuse pricing](https://langfuse.com/pricing) shortly before each session.

## Prerequisites

1. A Google account (for Colab and Google AI Studio).
2. A **Gemini API key** from [Google AI Studio](https://aistudio.google.com/apikey).
3. A **Langfuse Cloud** account at [cloud.langfuse.com](https://cloud.langfuse.com) (EU) or [us.cloud.langfuse.com](https://us.cloud.langfuse.com) (US), with a project and a public/secret API key pair.

No credit card, Docker, local Python, or paid subscription is required for the core demonstration.

## Setup and Execution

### 1. Open the notebook in Colab

Either:

- **Upload:** in [Google Colab](https://colab.research.google.com/), choose **File → Upload notebook** and select `monitoring_logging_ai_systems.ipynb`; or
- **From GitHub:** push this folder to a GitHub repository and open `https://colab.research.google.com/github/<owner>/<repo>/blob/<branch>/monitoring_logging_ai_systems.ipynb`.

### 2. Add Colab Secrets

Click the **key icon** in Colab's left sidebar and add three secrets, enabling **Notebook access** for each:

| Secret name | Value |
|---|---|
| `GEMINI_API_KEY` | Your Gemini API key |
| `LANGFUSE_PUBLIC_KEY` | `pk-lf-...` |
| `LANGFUSE_SECRET_KEY` | `sk-lf-...` |

If your Langfuse project is in the **US** region, change `LANGFUSE_BASE_URL` in Step 2 of the notebook to `https://us.cloud.langfuse.com`.

### 3. Run top to bottom

Run each cell in order. The notebook sends six text-generation requests plus one deliberately rejected request. Each scenario prints a structured log line, a summary, and a link to its trace in Langfuse.

### 4. Clean up

The final cell flushes telemetry and shuts down the client. Delete the Langfuse project or traces if you do not want to keep them, and rotate keys if they were exposed.

## Preflight Checklist (run 1–2 days before, and again 30 minutes before the session)

- [ ] Colab opens the notebook and Step 1 installs without errors.
- [ ] Step 3 prints `langfuse_auth True` and `gemini_model_available True`.
- [ ] Scenario A returns an answer mentioning the 45-day window and the Returns Hub.
- [ ] The Scenario A trace appears in Langfuse with five child observations and token usage on `llm-generate`.
- [ ] Scenario D1 passes and D2 fails the evaluation; both scores appear in Langfuse.
- [ ] Screenshots of a complete rehearsal run are saved and labelled **"previously captured — [date]"** as a fallback.
- [ ] The Gemini rate-limit page shows headroom for at least two full runs on the day.
- [ ] The model named in `MODEL_ID` is still listed as free-tier on the pricing page.

## Troubleshooting

| Symptom | Likely cause | Remedy |
|---|---|---|
| Prompt asks for keys, or `SecretNotFoundError` | Secret missing, misnamed, or notebook access disabled | Match the names exactly; enable notebook access |
| Langfuse authentication fails | Wrong keys or wrong region URL | Re-copy keys; set `LANGFUSE_BASE_URL` for your region |
| Model not found | Model renamed or not available to the project | Pick a current free-tier model from the [models page](https://ai.google.dev/gemini-api/docs/models) and update `MODEL_ID` |
| `429` / `RESOURCE_EXHAUSTED` | Free-tier rate limit or daily quota | Wait and retry; limits are per project; daily quotas reset at midnight Pacific time |
| `5xx` errors | Provider-side issue | Check [AI Studio status](https://aistudio.google.com/status); retry later |
| `AttributeError` on `interactions` or `start_as_current_observation` | Outdated SDK | Re-run Step 1, restart the Colab session, run from Step 2 |
| Trace link shows nothing yet | Ingestion delay | Wait 10–30 seconds and refresh |
| Variables undefined | Colab runtime reset | Re-run from Step 1 |

The notebook contains a fuller troubleshooting table and a contingency plan for service outages.

## Validation Status

What has been checked:

| Check | Result |
|---|---|
| Notebook is valid JSON and passes `nbformat` schema validation | Passed |
| All code cells execute end-to-end with the **real** `langfuse` 4.17.0 SDK and the **real** `google-genai` 2.29.0 SDK installed, where Gemini network calls were replaced by a local stub and Langfuse export was pointed at an unreachable address | Passed. All scenario assertions (failure stages, evaluation pass/fail, no answer text in logs) held; no SDK validation warnings were raised |
| Method signatures used by the notebook (`start_as_current_observation`, `update`, `propagate_attributes`, `create_score`, `get_trace_url`, `interactions.create` parameters, `Interaction.usage` fields) | Confirmed by introspecting the installed SDKs |
| Pricing, free-tier, and API details | Checked against official documentation on 11 Oct 2026 |

What has **not** been verified:

- An end-to-end run against the live Gemini API and Langfuse Cloud with real credentials. This requires your keys; run the preflight checklist above before the session.
- The exact wording of model responses. Because LLM output is non-deterministic, Scenario D is designed so that the expected outcome is highly likely but not guaranteed; the notebook explains how to interpret an unexpected result.
- The appearance of the Langfuse UI, which may change between releases.

## Known Limitations

- **Retrieval is a stand-in.** A keyword router over three in-memory documents replaces a vector store, to avoid extra dependencies. The telemetry patterns are the same as for real retrieval.
- **Latency is measured locally** in Colab and includes network time; it is not the provider's processing time.
- **Cost figures are illustrative**, computed from a published paid-tier list price. Actual free-tier charges are zero; real costs depend on tier, discounts, caching, and batch usage.
- **The quality check is a single rule-based evaluator**, suitable for demonstrating the concept only.
- **No streaming**, so time to first token is not measured.
- **Do not send confidential or personal data** through the free tiers used here.
