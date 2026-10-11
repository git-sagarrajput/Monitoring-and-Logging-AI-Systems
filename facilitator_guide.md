# Facilitator Guide — Monitoring and Logging AI Systems

This guide is for the person delivering the 90-minute session. Learners do not need it; the self-contained learning resource is [monitoring_logging_ai_systems.md](monitoring_logging_ai_systems.md).

**Format:** theory-led, with one instructor-operated live demonstration. Participants watch and discuss; they do not need accounts or code.

---

## 1. Session Goals

By the end, participants should be able to:

1. Explain why "the API returned 200" is not evidence that an AI system is healthy.
2. Distinguish logs, metrics, traces, and evaluation, and say what each is for.
3. Name the additional signals RAG and agentic systems need.
4. Position OpenTelemetry, Langfuse, Phoenix, LangSmith, Datadog, and cloud-native tools correctly.
5. Apply the Observe → Localise → Inspect → Validate → Remediate → Verify method to an incident.
6. Identify the governance questions that AI telemetry raises.

## 2. Agenda (90 minutes)

| Time | Segment | Module sections | Format |
|---|---|---|---|
| 0–10 | **Why AI systems fail silently** | §1 | Open with the "is the system healthy?" scenario; take two or three answers from the room |
| 10–25 | **Foundations** — logs, metrics, traces, evaluation; trace anatomy | §2, §3 | Theory with the trace waterfall and the system-type comparison table |
| 25–38 | **What to measure** — latency percentiles, tokens and cost, quality, drift; RAG and agents | §4, §5 | Theory; use the "why averages mislead" example and the 2×2 retrieval/answer table |
| 38–50 | **Tools and platforms** — three layers, decision framework | §6 | Theory and discussion: "which row of the decision table describes your organisation?" |
| 50–70 | **Live demonstration** | Notebook | Instructor-operated; see the script below |
| 70–80 | **Debugging methodology, alerting, governance** | §7, §8, §9 | Walk the six steps using the demo's Scenario D as the case; then SLOs and privacy |
| 80–87 | **Architecture and checklists** | §10, §11 | Production reference diagram; minimum viable checklist |
| 87–90 | **Wrap-up and questions** | §11.3 | Key takeaways |

If time runs short, compress §6 (tools) to the comparison table and the decision framework; do not cut the debugging methodology.

## 3. Before the Session

- Complete the **preflight checklist** in [README.md](README.md) one or two days before and again 30 minutes before.
- Run the full notebook once on the day so the Langfuse project already contains a complete set of traces to browse.
- Save screenshots of a complete run (trace tree for A, failed observations for C1–C3, D1/D2 with scores). Label each **"previously captured — [date]"**.
- Open three browser tabs: the Colab notebook, the Langfuse trace list (filtered to the `workshop-demo` environment), and the learning module.
- Increase the browser zoom so code and traces are readable from the back of the room.
- Do not display the Colab Secrets panel while presenting.

## 4. Live Demonstration Script (≈20 minutes)

Steps 0–3 should already have been run before participants arrive. Start the live segment at Step 4.

| Minutes | Notebook step | Show | Key point to make |
|---|---|---|---|
| 0–2 | Step 4 (application) | The five-stage pipeline and the knowledge base; the 45-day / Returns Hub facts | "These two facts let us check correctness later." |
| 2–5 | Step 5 (instrumentation) | Scroll through `handle_support_request`; point to `traced_step`, `propagate_attributes`, `log_event` | Instrumentation is a deliberate design choice: what we record decides what we can debug. Answer text is not logged. |
| 5–8 | Scenario A | Run it. Show the JSON log line, the summary, then open the trace | Read the tree: which span dominates? Show model, parameters, token usage, version attributes. Explain local latency vs. provider-reported tokens. |
| 8–10 | Scenario B | Run; show the comparison table | Tokens rise with input; latency rises much less; cost is asymmetric. One measurement is not a trend. |
| 10–14 | Scenario C | Run C1, C2, C3; show the failures table; open the C2 trace | C2: the model succeeded and we paid for it, but the user got an error. Error rate alone cannot distinguish C2 from C3. C1 is a correct rejection, not a bug. |
| 14–18 | Scenario D | Run D1 and D2; show the quality table; open D2 in Langfuse, select `retrieve-context`, then show the score | Both "succeeded." Only evaluation separates them; only the trace explains why. Changing the model would not fix a retrieval fault. |
| 18–20 | Step 6 (dashboard) | Run the summary | This is how metrics are derived from logs. With seven requests, the percentiles are meaningless; the method is the point. |

Leave Step 7 (debugging exercise) for the methodology segment immediately afterwards.

### If something unexpected happens during the demo

- **D1 fails the check or D2 passes.** Treat it as a teaching moment: LLM output is non-deterministic, and a single rule is brittle. Show the actual answer text and ask whether the rule or the answer is wrong. Re-running once is acceptable; repeated re-running until the "right" result appears is not.
- **A request is slow.** Point out that this is exactly what p95 is for, and that one slow request is not an incident.
- **A 429 appears.** This is a real provider rate-limit error; show how it appears in the trace and the log, then wait a minute.

## 5. Contingency Plan

| Failure | Action |
|---|---|
| Gemini unavailable or quota exhausted | Switch to the saved screenshots, stating clearly that they were captured earlier. Continue explaining the telemetry. Do not present earlier traces as new live calls. |
| Langfuse unavailable | Run the notebook anyway: logs, tables, and the Step 6 dashboard work locally. Use screenshots for the trace views. |
| Colab unavailable or runtime resets | Re-run from Step 1 (about two minutes). If Colab is down, use screenshots. |
| Projector or network failure | Teach the demo from the module's worked examples in §5.2, §7.4, and §7.5, which mirror the scenarios. |

## 6. Anticipated Questions

**"Can we just use our existing APM tool?"**
Often yes, for infrastructure and request-level signals, and several APM vendors now have LLM features. The questions are whether it shows prompts, responses, retrieved context, token usage, and evaluation scores, and whether it meets your data-handling requirements (§6.10).

**"Is OpenTelemetry a product we buy?"**
No. It is an open standard and set of libraries for producing and moving telemetry. You still need a backend to store and view it (§6.2).

**"Should we log every prompt and response?"**
Not by default. Capture metadata for all traffic, full text for a sample and for all errors and quality failures, with redaction, access control, and retention limits (§9.5, §9.11).

**"How do we measure correctness without a right answer?"**
Offline, on curated datasets with reference answers. Online, with reference-free evaluators (groundedness against retrieved context, safety, format), user feedback, and escalation rates. Validate any LLM-as-judge against human labels (§4.5, §8.6).

**"What thresholds should we alert on?"**
None universally. Derive them from user requirements and observed baselines, tie them to SLOs, and alert on burn rate rather than single events (§4.7, §8.4).

**"How much does this cost?"**
Two costs: the observability platform (often priced by events or spans ingested) and the extra model calls for LLM-as-judge evaluation. Sampling controls both (§9.5).

**"Isn't the free Gemini tier fine for production?"**
No. It has lower rate limits, and at verification time Google's pricing page states free-tier content is used to improve its products. Use it only for learning with non-sensitive data (§9.8).

**"Who should own AI observability?"**
The team that owns the AI service owns its alerts and runbooks; a platform team typically owns the shared telemetry pipeline; product owners own quality criteria (§8.5).

## 7. Key Messages to Reinforce

1. Running is not working.
2. Metrics detect, traces localise, logs explain, evaluations judge.
3. Version everything: model, prompt, retrieval configuration, index.
4. Evaluate retrieval and answers separately.
5. Evidence before changes: never "fix" the prompt or model on a hunch.
6. AI telemetry is sensitive data.
