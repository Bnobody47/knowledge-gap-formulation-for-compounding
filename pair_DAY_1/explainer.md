# Why front-loaded rules drift in long evaluator and agent loops

**Partner question (Birkity Yishak):** In multi-turn evaluation/agent loops, why do models gradually stop following initial system rules even when those tokens are still in the context window, and what role do attention sinks, KV cache reuse, and prefix caching play? What practical controls preserve instruction fidelity over long sessions?

The short answer is: **in context is not the same as in control**. A rule token can remain visible to the model but lose influence if the current decode step no longer routes enough attention through it.

At each decode step, the model forms a new query vector and scores all prior key vectors under causal masking. Influence comes from **current query-key compatibility**, not from historical importance. So even if the system prompt stays inside the window, it only matters when enough heads/layers still allocate competitive attention to it (or to later tokens that faithfully carry it forward).

In long loops, three things usually shift that routing away from front-loaded rules:

1. **Recency competition grows.** New user turns, tool outputs, and prior model responses provide fresh, high-similarity tokens that often beat early rubric tokens in attention mass.
2. **Self-conditioning accumulates.** Slightly off-policy model outputs become recent context, then influence the next step, creating drift compounding.
3. **Turn boundaries reset local objectives.** Each new prompt/task chunk changes the query geometry; system-rule attention often drops across turns rather than decaying smoothly token-by-token.

## The load-bearing mechanism

A useful mental model is not “the model forgot the prompt,” but:

- The prompt is stored.
- The decode computation increasingly chooses newer states as better predictors of the next token.
- Semantic authority drifts from static rules to dynamic trajectory.

This distinction explains a common engineering confusion: you inspect attention and still see mass near early tokens, yet behavior drifts. That can happen because of **attention sinks**: some first tokens receive stable “normalization anchor” attention that does not guarantee meaningful rule-following semantics.

## What attention sinks, KV cache, and prefix cache actually do

### Attention sinks

Attention sinks are early tokens (often near the prefix start) that attract persistent attention mass, partly because softmax must allocate probability somewhere. They can stabilize long-stream behavior, but sink attention can be structural rather than semantic. So “high attention on early prefix” does not automatically mean “high instruction fidelity.”

### KV cache reuse

KV caching reuses previously computed key/value states to avoid recomputation during autoregressive decoding. This is mainly a speed optimization. It keeps old tokens available, but as **frozen snapshots**. They are not re-encoded in light of later turns; each new query must rediscover them as relevant. That tends to favor recent, task-local states.

### Prefix caching

Prefix caching reuses a shared static prefix across separate requests. It does not directly fix drift inside one giant mutable chat. Its big value is operational: it makes it cheap to reconstruct each call with an immutable canonical rubric, instead of relying on one ever-growing conversation.

## Concrete worked example from my evaluator stack

In my Week 11 evaluator logic (`scoring_evaluator.py`), I currently score cautious behavior from lexical cues:

- downgrade cues: `phased`, `scope`, `discovery`, `handoff`
- overcommit cues: `immediately`, `this week`, `we can deploy`

The model passes `bench_commitment_safety` when weak-signal cases include downgrade cues and avoids hard-commit cues.

In a long agent loop, this can drift even when the original rubric is still present:

- Step 1: model produces cautious phrasing (on-policy)
- Step 2: tool output introduces urgency language
- Step 3: model echoes urgency tone and emits “this week” phrasing
- Step 4: that output itself becomes recent high-similarity context and is reused as style evidence

Result: rule text remains in-window, but next-token generation is increasingly conditioned by recent trajectory tokens. The scoring logic catches some of this at output level, but the mechanism behind drift is decode-time routing and self-conditioning.

## Practical mechanisms that preserve fidelity (beyond bigger context)

1. **Stateless/quasi-stateless reconstruction per step.** Build each evaluator call as: immutable rubric + compact task state + current item. Do not rely on one long mutable thread.
2. **Separate policy memory from episodic memory.** Keep rubric/tool rules immutable; summarize only observations/trajectory.
3. **Re-anchor critical rules near point-of-use.** Repeat compressed governing constraints before judgment/action steps.
4. **Insert explicit rule-recall pass.** First ask model to name applicable rubric items, then produce verdict.
5. **Constrain outputs structurally.** Use schema/checklist outputs so off-policy drift is easier to detect and reject.
6. **Use post-hoc validators.** Keep deterministic checks (like overcommitment guardrails) outside model reasoning to enforce hard boundaries.
7. **Measure drift explicitly.** Track per-turn instruction adherence and variance, not only final pass/fail.

## Beyond prompt engineering: system-level controls

Prompt quality helps, but evaluator reliability improves most when you change the system architecture. A concise control stack for this exact failure mode:

1. **Fresh evaluator calls (stateless).** Judge each case independently with canonical rubric + current evidence, so previous weak judgments cannot contaminate future ones.
2. **Immutable policy block + dynamic state block.** Keep scoring rules fixed; only conversation/task evidence is summarized or compacted.
3. **Retrieval-based rule injection.** Pull only the most relevant rubric clauses near the decision point (e.g., overcommitment + unsupported timeline rules for weak-signal cases).
4. **External validators and guardrails.** Add deterministic checks for unsafe phrasing (`immediately`, `guarantee`, `we can deploy this week` under weak evidence) and force revision on fail.
5. **Multi-pass evaluation.** Decompose into: signal strength -> commitment strength -> mismatch check -> final penalty/reward -> recommended safer behavior.
6. **Programmatic scoring for critical rules.** Let code enforce hard penalties (`weak signal` + `hard commitment` => overcommitment flag), while LLM handles extraction/explanation.
7. **Specialized narrow judges.** Use separate detectors (overcommitment, uncertainty handling, handoff quality) instead of one broad evaluator.
8. **Calibration with gold references.** Periodically test known cases; if drift appears, re-anchor/reset prompt state.
9. **Context hygiene and memory control.** Keep rules/current evidence; drop noisy history. For advanced stacks, pin policy tokens and evict low-value cache entries.
10. **Advanced levers (when self-hosting).** Attention steering, KV-cache policies, evaluator fine-tuning, and verifiable reward training can make rule adherence less dependent on fragile long-context attention.

## Scope discipline

This explainer focuses on decode-time influence drift in transformer attention and production controls for evaluator/agent loops. I did **not** cover full training-time interventions (e.g., long-horizon RL policy shaping) in depth; those are follow-on work once inference-time controls are in place.

## What changed in my understanding

Before: I treated instruction drift mostly as a context-length issue.

After research: I understand drift as a **control-path issue**. Tokens can be present but functionally inactive; attention sinks can mask this; KV cache preserves availability, not authority; prefix caching enables a better architecture (replay immutable rubric each step) rather than fixing a drifting session directly.

That reframes the engineering strategy from “add more window” to “control routing, re-anchor rules, and externalize hard guarantees.”
