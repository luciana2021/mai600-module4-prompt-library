# Prompt Library — CGMP Non-Compliance Task

**Source article:** [Facts About the Current Good Manufacturing Practice (CGMP)](https://www.fda.gov/drugs/pharmaceutical-quality-resources/facts-about-current-good-manufacturing-practice-cgmp) — U.S. FDA

**Task used across all prompts:** Explain what happens when a pharmaceutical company fails to comply with CGMP regulations.

**Scope note:** This evaluation uses 3 prompt variants (representing increasing levels
of structure) tested against 5 evaluation cases, run manually (no API) via ChatGPT.

| Prompt ID | Type | Purpose |
|---|---|---|
| P1 | Zero-shot | Basic task instruction without examples |
| P2 | Structured | Adds role, context, and output format |
| P5 | Final improved | Adds audience, tone, and evidence-uncertainty guardrails |

---

## Prompt 1 — Zero-shot

```
Explain what happens when a pharmaceutical company fails to comply with CGMP
regulations, in the context of the following scenario:

{scenario}
```

## Prompt 2 — Structured

```
You are a regulatory affairs advisor. Explain what happens when a pharmaceutical
company fails to comply with CGMP regulations, for the scenario below. Present the
answer as a numbered list of consequences.

Scenario: {scenario}
```

## Prompt 5 — Final improved prompt

```
You are a regulatory affairs advisor briefing a new quality manager with no CGMP
background. In a professional, reassuring but precise tone, explain what happens
when a pharmaceutical company fails to comply with CGMP regulations, for the
scenario below. Avoid alarmist language; clarify that non-compliance does not
automatically mean the drug is unsafe. If a specific figure, timeline, or citation
is not known from FDA's guidance, state 'not specified in source' rather than
estimating.

Scenario: {scenario}
```

**Why Prompt 5 is the best-performing prompt:** it scored a perfect 5.0 average
across all 7 criteria (vs. 4.97 for P2 and 4.91 for P1). It combines P2's role and
structure with explicit audience framing, tone guidance, and an uncertainty rule —
which most directly targets the failure modes observed (overgeneralized regulatory
consequences and insufficient uncertainty handling; see `README.md` §8).

---

## Revision Testing (Part 7 — Prompt Revision and Re-Score)

Based on the failure modes identified during evaluation, two P1 (zero-shot) outputs
were revised and re-tested:

### P1 | T1 — Revised

**Original weakness:** Did not clearly separate the CGMP documentation deficiency
from any confirmed product quality impact.

**Revised prompt:**
```
You are a regulatory affairs advisor briefing a new pharmaceutical quality manager.

Explain the consequences of a CGMP inspection failure involving inadequate
equipment calibration records.

Requirements:
- Use a professional and reassuring tone.
- Explain that a CGMP deficiency does not automatically mean the drug is unsafe.
- Clearly separate:
  1. Inspection finding
  2. Potential quality impact assessment
  3. Regulatory consequences
  4. Corrective and preventive actions (CAPA)
- Avoid assuming product failure unless supported by evidence.
```

**Result after revision:** All 7 criteria scored 5/5. Failure mode: none observed
after revision.

### P1 | T3 — Revised

**Original weakness:** Did not explicitly emphasize that a specific fine amount is
unavailable / not specified in the source.

**Revised prompt:**
```
You are a regulatory affairs advisor.

Explain whether FDA charges a specific fine amount for a first-time CGMP violation.

Requirements:
- State clearly if a fixed fine amount exists or does not exist.
- If FDA guidance does not provide a specific figure, write:
  'not specified in source'
- Do not estimate penalties.
- Explain what factors influence FDA enforcement decisions.
- Use a professional regulatory briefing tone.
```

**Result after revision:** All 7 criteria scored 5/5. Failure mode: none observed
after revision.
