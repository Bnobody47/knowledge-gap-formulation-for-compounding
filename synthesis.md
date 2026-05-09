# Week 12 synthesis

## What changed this week

Week 12 shifted my work from "ship features" to "defend mechanisms." Across Day 1 to Day 4, the biggest change was moving from output-level judgment ("it looks better") to mechanism-level judgment ("I can explain why this behavior appears and how to make it robust"). The practical pattern that emerged is consistent across topics:

1. identify a concrete uncertainty in a shipped artifact,
2. sharpen it into a diagnostic question,
3. produce a public explainer with evidence,
4. ground the learning back into portfolio edits.

That loop made my existing Week 10 and Week 11 systems more defensible and more production-ready.

## Gaps closed so far (Day 1-4)

### Day 1 - Inference-time mechanics

I closed a gap on why front-loaded system rules drift in long evaluator/agent loops even when the tokens remain in context. The key insight was that "in context" is a storage guarantee, not an influence guarantee. During decoding, recency, query-key compatibility, and self-conditioning can dominate earlier policy text. This changed my evaluator thinking from "increase context length" to "control the decision path": stateless calls, policy/state separation, and rule re-anchoring.

### Day 2 - Agent and tool-use internals

I closed a gap on tool invocation reliability in DeepSeek V3.2 auto mode. The core mechanism was not just model choice, but a three-layer system: token generation, prompt serialization, and parser recovery. That clarified why ordered instruction adherence can fail even when the model appears aligned. This changed my deployment posture: for order-sensitive workflows, auto parser-based tool use is best-effort unless strict validation and recovery controls are present.

### Day 3 - Training and post-training mechanics

I closed a gap on what LoRA SFT cross-entropy proves and what it does not prove in the Tenacious-Bench adapter setting. I can now explain token-level objective behavior, LoRA gradient flow through frozen base weights into trainable A/B matrices, and why high augmentation concentration can create metric improvements that still require generalization diagnostics. This gave me a clearer framework for separating style learning from surface-pattern memorization.

### Day 4 - Evaluation and statistics

I closed a gap on interpreting non-significant benchmark outcomes. I can now distinguish "no effect" from "underpowered benchmark," compute MDE and power-linked sample-size targets, and report finite-bootstrap p-values correctly. This changed my statistical reporting standard from significance-only to power-aware benchmark design.

## Most surprising thing learned

The most surprising week-level insight was how often "correct-looking" conclusions collapse when mechanism is examined:

- a model can keep policy tokens in context and still lose policy control,
- a tool call can look valid at API level while being fragile at parser boundary,
- a statistically significant score can still hide behavioral shortcuts,
- and a non-significant score can still be consistent with true practical gain if power is low.

Across all four days, the same meta-lesson held: reliability comes from controlling the process, not just checking final outputs.

## Canonical reading and tool contributions

This week produced a practical canon for my own workflow:

- transformer attention and decode-time behavior references,
- tool-calling protocol vs constrained decoding references,
- LoRA training mechanics and module-level diagnostic patterns,
- benchmark power analysis and finite-bootstrap reporting discipline.

I also treated implementation docs as first-class evidence (model cards, parser code, runtime docs, benchmark scripts), not only papers, because production failures often emerge at integration boundaries.

## Portfolio impact so far

The week already improved my Week 10/11 portfolio in three ways:

1. **Stronger mechanism attribution:** explanations now connect behavior to token-level and systems-level causes.
2. **More defensible evaluation claims:** significance statements now include uncertainty and power context.
3. **Clearer production controls:** guardrails are framed as architecture choices with measurable failure modes.

## Submission status note

This final submission reflects a completed 4-day run (Day 1-4), with full paired artifacts and public posts for those days.
