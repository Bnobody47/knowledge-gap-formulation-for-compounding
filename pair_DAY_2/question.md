# Question (final, post–morning call)

**Topic / subtopic:** Agent and tool-use internals — control flow, tool invocation boundary, and guardrail tradeoffs

**Sharpened question:**
In a tau2-bench-style multi-tool agent workflow, how does the agent actually transition from goal parsing to either continued reasoning or tool invocation, and which guardrails (schema constraints vs stop conditions) most effectively reduce tool hallucination and bad tool calls without adding unacceptable latency?

**Grounded in my work:**  
I want this to stay broadly reusable across my Week 10/11 systems and future agent projects, but still testable. So I am anchoring it to a tau2-bench-style workflow where I can measure: (1) tool hallucination/error rate, (2) loop/termination behavior, and (3) latency impact when guardrails are changed.

---
Properties check: diagnostic · grounded · generalizable · resolvable in one explainer
