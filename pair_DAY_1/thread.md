# Public thread (Day 1)

**Post 1/6**
Why do models in long evaluator/agent loops start ignoring initial system rules even when those tokens are still in context? Because “in context” is a storage guarantee, not an influence guarantee.

**Post 2/6**
At decode step *t*, the model forms a fresh query and scores all prior tokens. A rule token only influences output if it still wins attention mass against newer tokens. Long loops add many recent, high-similarity competitors (dialogue, tool output, prior model text).

**Post 3/6**
So drift is often not prompt disappearance. It’s routing drift: semantic control shifts from front-loaded rubric tokens to recent trajectory tokens. This gets worse in multi-turn settings where each turn changes local objective/query geometry.

**Post 4/6**
Attention sinks add a nuance: early prefix tokens can keep attracting attention mass, but that can be normalization-anchor behavior, not semantic rule-following. “High attention near start” does NOT always mean “high instruction fidelity.”

**Post 5/6**
KV cache reuse = speed optimization + frozen old states (available, not reinterpreted). Prefix caching = cheap replay of immutable rubric across calls. Practical fix: reconstruct each step as canonical policy + compact state + current item, and re-anchor critical rules near decision points.

**Post 6/6**
For my Week 11 evaluator, this changed my approach from “increase context” to “preserve control path”: stateless judging, policy/trajectory separation, rule-recall pass, and deterministic post-checks for overcommitment. Full write-up: [add blog URL]
