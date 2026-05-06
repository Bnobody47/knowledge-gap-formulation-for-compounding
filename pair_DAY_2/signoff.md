# Asker sign-off

**Status:** closed

**Paragraph:**
Melaku’s explainer closed my gap on the decision boundary between continued reasoning and tool invocation in multi-step loops. I now understand this boundary as a token-level probability competition (`reasoning-text tokens` vs `tool-call-format tokens`), and why tool hallucination is often a constraint problem rather than only a reasoning problem. The key practical insight was separating the roles of guardrails: schema constraints improve correctness of each tool call, while stop conditions control loop length and redundant actions; using both together gives the best stability-latency tradeoff. This gives me a clearer way to design and evaluate agent workflows in future projects.

(Replace with your exact final wording if your tutor requires verbatim call wording.)
