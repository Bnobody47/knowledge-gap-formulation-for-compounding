# Question (final, post–morning call)

**Topic / subtopic:** Evaluation and statistics — power, uncertainty, and significance reporting in binary-task benchmarks

**Sharpened question:**
In `Week-11/submission_report.md`, I reported `Delta A = -2.34 pts (95% CI [-11.09, +6.20], p = 0.71)` and `Delta B = +22.18 pts (95% CI [+14.43, +29.82], p = 0.0)` using 2,000 bootstrap samples over 216 binary pass/fail tasks. My gap is: with this benchmark size and baseline pass rate, what is the minimum detectable effect (MDE) at 80% power for a two-proportion comparison, and how many tasks should Tenacious-Bench v0.2 include to detect plausible true gains (+3, +5, +8 points) with 80% power? I also need the correct finite-bootstrap reporting for the “p = 0.0” line.

**Grounded in my work:**  
This is grounded in Week 11 `submission_report.md` where I interpreted `p = 0.71` as no clear gain without checking whether the benchmark was underpowered, and where I reported an impossible finite-bootstrap value (`p = 0.0` with B=2000). Closing this gap changes how I interpret Delta A, how I set task-count targets for v0.2, and how I report statistical evidence in future benchmark iterations.

---
Properties check: diagnostic · grounded · generalizable · resolvable in one explainer
