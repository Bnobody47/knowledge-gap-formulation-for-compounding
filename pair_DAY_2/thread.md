# Public thread (Day 2)

**Blog (full post):** https://dev.to/bnobody47/why-deepseek-v32-tool-calls-can-drift-from-ordered-system-instructions-1gb

**Post 1/6**
DeepSeek V3.2 `tool_choice="auto"` is not just "model chooses tool." In open-weight stacks, tool calls are often emitted as text wrappers then parsed into structured objects.

**Live:** https://x.com/a_beamlak/status/2052094708109754798

**Post 2/6**
That means 3 layers decide reliability: token generation, prompt serialization, and parser recovery. If any layer is brittle, ordered tool workflows can drift.

**Live:** https://x.com/a_beamlak/status/2052094841232794057

**Post 3/6**
Key difference: parser-based auto mode vs decoder-constrained strict function-calling. In strict mode, invalid next tokens are masked during generation. In parser-first mode, structure can fail after generation.

**Live:** https://x.com/a_beamlak/status/2052094947138941244

**Post 4/6**
Why system instruction order breaks: free-decoding competition at tool boundary, long prompt distance from rule text to action point, reasoning/action boundary leakage, and truncation inside wrapper syntax.

**Live:** https://x.com/a_beamlak/status/2052095062947877023

**Post 5/6**
Best mitigations: compact tool schemas, encode order criteria in tool descriptions + system prompt, reserve token headroom, validate parsed args, and checkpoint long chains into smaller turns.

**Live:** https://x.com/a_beamlak/status/2052095204404908054

**Post 6/6**
Takeaway: for order-sensitive agents, treat `auto` as best-effort policy. If correctness matters, prefer constrained/named tool selection + strict validation. Full write-up: https://dev.to/bnobody47/why-deepseek-v32-tool-calls-can-drift-from-ordered-system-instructions-1gb

**Live:** https://x.com/a_beamlak/status/2052095407065350624
