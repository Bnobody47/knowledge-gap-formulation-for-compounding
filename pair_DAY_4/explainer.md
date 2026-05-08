# Why your non-significant benchmark result may be a power problem, not a model problem

**Partner question (Kemeriya Major):**  
With 216 binary pass/fail tasks and baseline pass rate around 74%, what is the minimum detectable effect (MDE) at 80% power, how many tasks are needed to detect +3/+5/+8 point gains, and how should a finite-bootstrap `p = 0.0` be reported correctly?

## Short answer

Your Week 11 Delta A result is more consistent with an underpowered benchmark for small gains than with a clean "no effect" conclusion. Under a standard two-proportion planning approximation (alpha=0.05, power=0.80, baseline ~0.74), 216 tasks gives an MDE of about **+10.9 points**. So +3 to +5 point improvements are unlikely to be detected at this sample size. Also, with 2,000 bootstrap replicates, `p = 0.0` is invalid; the minimum possible empirical p-value is `1/(B+1)=1/2001≈0.00050`.

## The load-bearing mechanism

A p-value is a statement about evidence under a hypothesis **given your sample size and noise level**. If power is low for your effect range, "not significant" is often "cannot tell yet," not "no improvement exists."

For binary-task benchmarks, planning power for a two-proportion comparison links:

- baseline pass rate,
- desired detectable lift,
- alpha and target power,
- required task count.

If your benchmark is too small for the lift you care about, CIs stay wide and null rejection becomes unlikely even when true gains exist.

## What your Week 11 numbers imply

From `submission_report.md`:

- `Delta A = -2.34 pts`
- `95% CI [-11.09, +6.20]`
- `p = 0.71`

This CI width (17.29 points) is too broad for decisive interpretation on small practical improvements. The interval still contains plausible positive lifts (+3, +5), so the defensible interpretation is **inconclusive for small effects**, not definitive no-effect.

## MDE at current size (216 tasks)

Using baseline ~74%, alpha=0.05 (two-sided), power=0.80:

- **MDE ≈ +10.9 points**

That means this benchmark is mainly sensitive to large effects.

Approximate detection power at n=216:

- true +3 point lift -> ~11%
- true +5 point lift -> ~23%
- true +8 point lift -> ~52%

So if true gain is modest (+3 to +5), non-significance is expected.

## Task count targets for Tenacious-Bench v0.2

For the same baseline and test settings:

- detect +3 points -> about **3,226 tasks**
- detect +5 points -> about **1,128 tasks**
- detect +8 points -> about **420 tasks**

Design implication:

- if +5 is the minimum practically meaningful lift, v0.2 should target ~1.1k+ tasks;
- if +3 matters, v0.2 needs a multi-thousand-task scale.

## Correcting the `p = 0.0` bootstrap line

For finite Monte Carlo/bootstrap resampling, use:

`p = (r + 1) / (B + 1)`

where:

- `B` = number of resamples
- `r` = number at least as extreme as observed

With `B=2000`, `r=0`:

- `p = 1/2001 ≈ 0.00050`

Correct reporting options:

- `bootstrap p ≈ 0.0005`
- `bootstrap p <= 1/2001`
- `bootstrap p < 0.001`

Not valid: `p = 0.0`.

## Practical report rewrite

"Under a standard two-proportion planning approximation (baseline ~74%), a 216-task benchmark has an 80%-power MDE of about 10.9 points. Therefore Delta A = -2.34 pts (95% CI [-11.09, +6.20], p = 0.71) is inconclusive for small practical gains, not definitive evidence of no effect. To detect +3/+5/+8 point improvements at 80% power, v0.2 requires about 3,226 / 1,128 / 420 tasks respectively. For Delta B with 2,000 bootstrap samples, report p ≈ 0.0005 (or p < 0.001), not p = 0.0."

## Scope note

These are planning approximations from a two-proportion framework. Since benchmark comparisons are item-paired, paired analyses (e.g., McNemar-style) can be more efficient when discordant counts are favorable. But this approximation already answers the decision question: 216 tasks is too small for +3 to +5 detection.

## Takeaway

Evaluation tells you score change; statistics tells you whether your benchmark could detect the change you care about. Day 4’s key outcome is moving from "significant/non-significant" reporting to **power-aware benchmark design**.
