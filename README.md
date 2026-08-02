# MAI 600 — Module 4 Assignment: Prompt Library & Evaluation Set

**Author:** Luciana de Souza Guedes
**Course:** MAI 600 — Natural Language Processing, Atlantis University
**Instructor:** Professor Ivan Suarez

---

## 1. Project Title
Prompt Library & Evaluation Set for CGMP Regulatory Guidance

## 2. Problem Description
Researchers and quality professionals in the pharmaceutical field often need quick,
reliable answers about FDA Current Good Manufacturing Practice (CGMP) regulations
without reading the full source article each time. This project builds a reusable
prompt library and evaluation set to test how well an LLM can answer CGMP-related
questions accurately, safely, and in a consistent format — while flagging known
failure modes like hallucination and format drift.

## 3. Task Selected
The task used across all prompts: **explain what happens when a pharmaceutical
company fails to comply with CGMP regulations**, based solely on the FDA source
article: [Facts About the Current Good Manufacturing Practice (CGMP)](https://www.fda.gov/drugs/pharmaceutical-quality-resources/facts-about-current-good-manufacturing-practice-cgmp).

## 4. Dataset / Input Set Description
The single source document is FDA's official CGMP facts page. From it, 5 evaluation
scenarios were derived (see `evaluation_set.csv`): T1 (batch/calibration failure),
T3 (fine-amount hallucination risk), T5 (patient medical-advice risk), T7
(seizure vs. injunction comparison), and T9 (quality-by-design rationale). These
cover a range of formats (table, bullet list, paragraph) and known risks
(hallucination, missing detail, unsafe advice, format drift).

## 5. Prompting Approach
Three prompt variants were built with increasing levels of structure and safeguards,
each run against all 5 test cases (15 combinations total), manually via ChatGPT:

| ID | Type | Purpose |
|---|---|---|
| P1 | Zero-shot | Basic instruction, no examples |
| P2 | Structured | Adds role, context, output format |
| P5 | Final improved | Adds audience, tone, and evidence-uncertainty guardrails |

Full prompt text is in [`prompt_library.md`](./prompt_library.md).

## 6. Evaluation Criteria
Each of the 15 outputs was scored 1–5 on:
- **Accuracy** — factually consistent with the FDA source
- **Helpfulness** — usefully addresses the scenario for its intended user
- **Format adherence** — matches the target output format
- **Completeness** — covers what the expected behavior requires
- **Evidence/grounding** — avoids unsupported claims; flags uncertainty appropriately
- **Safety/professionalism** — professional tone, no inappropriate advice
- **Clarity** — easy to read, unambiguous

## 7. Results Summary
All three prompts performed strongly (average scores between 4.91 and 5.00 out of
5), but a clear ranking emerged:

| Prompt | Accuracy | Helpfulness | Format | Completeness | Evidence | Safety | Clarity | **Overall** |
|---|---|---|---|---|---|---|---|---|
| **P5_final_improved** | 5.0 | 5.0 | 5.0 | 5.0 | 5.0 | 5.0 | 5.0 | **5.00** |
| P2_structured | 5.0 | 5.0 | 5.0 | 5.0 | 4.8 | 5.0 | 5.0 | 4.97 |
| P1_zero_shot | 5.0 | 5.0 | 5.0 | 4.6 | 4.8 | 5.0 | 5.0 | 4.91 |

P1 (zero-shot) lost points mainly on **completeness** (T1, T3) and **evidence
grounding** (shared with P2), consistent with the failure modes below. Full
per-test-case results are in [`results_table.csv`](./results_table.csv).

## 8. Failure Modes Found
Three failure modes were identified during evaluation:

1. **Overgeneralization of regulatory consequences** — outputs sometimes described
   possible FDA actions (seizure, injunction, etc.) without clearly flagging that
   these are case-specific, not automatic outcomes.
2. **Insufficient source-uncertainty handling** — outputs occasionally stated
   regulatory details confidently where FDA's source didn't specify a figure or
   timeline (most visible in P1's T3 response about fine amounts).
3. **Incomplete differentiation between product risk and compliance risk** —
   responses could more clearly separate "CGMP documentation deficiency" from
   "confirmed product quality failure," to avoid implying every compliance issue
   means the drug itself is unsafe.

## 9. Prompt Improvements Made
Two P1 (zero-shot) outputs with observed weaknesses (T1 and T3) were revised to
directly address failure modes #2 and #3 above — adding explicit structure
requirements and an uncertainty rule. Both revised prompts scored a perfect 5/5
across all criteria after re-testing. Full revised prompts and results are in
[`prompt_library.md`](./prompt_library.md#revision-testing-part-7--prompt-revision-and-re-score).

## 10. Final Best Prompt
**Prompt 5 (final improved)** is the best-performing prompt, with a perfect 5.00
overall average. It outperformed P1 and P2 because it added role/audience framing,
tone guidance ("avoid alarmist language"), and an explicit uncertainty rule
("state 'not specified in source' rather than estimating"). This combination
directly reduced the overgeneralization and uncertainty-handling failure modes
observed in P1 and P2, while preserving P2's structural clarity.

## 11. AI Tool Usage Disclosure
See [`ai_usage_disclosure.md`](./ai_usage_disclosure.md) for full details.
Summary: Claude assisted with prompt/evaluation design, the Python script, and
failure-mode analysis; ChatGPT generated the 15 model outputs; all scoring and
final analysis was done by the author.

## 12. Reflection: What I Learned
Working through this evaluation showed how much a small amount of added structure
changes output reliability, even when a model is already performing well. The
zero-shot prompt (P1) was accurate but inconsistent in how completely it separated
compliance findings from safety implications — a distinction that matters a lot in
a regulated field like pharmaceuticals, where overstating risk can be just as
harmful as understating it. Adding an explicit uncertainty rule ("not specified in
source") turned out to be the single most effective guardrail: it directly
prevented the model from filling gaps in the FDA source with plausible-sounding
but unsupported details. I also learned that revising a prompt is often more
effective than any post-hoc fact-checking — once P1's two weak outputs were
re-prompted with clearer structural and evidentiary requirements, both reached
perfect scores. Going forward, I'd apply this same "role + structure + uncertainty
rule" pattern as a default template any time I'm using an LLM to summarize
regulatory or scientific source material.

---

## Repository Structure
```
mai600-module4-prompt-library/
├── README.md
├── prompt_library.md          # 3 prompts + revision testing (Part 7)
├── evaluation_set.csv         # 5 test cases
├── results_table.csv          # Scored outputs (3 prompts x 5 tests = 15 rows)
├── ai_usage_disclosure.md
└── notebooks/
    └── prompt_evaluation.ipynb   # Full executed evaluation notebook
```
