# Grounding commit

**Pointer:** `C:\Users\Bnobody_47\Documents\Sales Agent Evaluation Bench\scoring_evaluator.py` and `C:\Users\Bnobody_47\Documents\Sales Agent Evaluation Bench\methodology.md`

**Paragraph:**
After Day 1, I am grounding the explainer by updating evaluator-loop design notes and scoring rationale to treat instruction fidelity as a decode-time control-path issue, not a pure context-length issue. Concretely, I will add a stateless evaluation pattern (immutable rubric + compact state + current item), insert an explicit rule-recall pass before verdict generation, and document policy/trajectory separation so summaries do not rewrite controlling rules. This directly follows from the explainer’s mechanism-level conclusion: tokens can remain in context while losing influence, so fidelity must be preserved through architecture and loop controls, not only bigger windows.

(Replace with commit hash/PR link after committing.)
