# Question (final, post–morning call)

**Topic / subtopic:** Inference-time mechanics — decoding, uncertainty expression, and action selection under weak evidence

**Sharpened question:**
In my Week 11 `Sales Agent Evaluation Bench`, I penalize `bench_overcommitment`—cases where the model makes a hard delivery commitment when signals are weak—and I reward outputs that downgrade to a phased discovery / handoff. What I do **not** understand (and cannot currently defend) is the inference-time mechanism behind this behavior: how prompt conditioning, the model’s token probability distribution under uncertainty, and decoding choices (e.g., temperature / top‑p) interact to produce **cautious downgrade tokens** (phased/scope/discovery/handoff) versus **overconfident commitment tokens** (immediately/this week/we can deploy).

**Grounded in my work:**
This gap is grounded in Week 11’s `methodology.md` (the `bench_overcommitment` target) and the scoring logic in `scoring_evaluator.py`, where I currently infer “good uncertainty handling” from final text patterns without understanding the inference-time mechanism that makes those patterns appear. Closing this gap would let me revise/defend how I measure commitment safety and how I prompt/route generation in weak-signal scenarios.

---
Properties check: diagnostic · grounded · generalizable · resolvable in one explainer
