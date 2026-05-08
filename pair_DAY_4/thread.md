# Public thread (Day 4)

**Blog (full post):** https://dev.to/bnobody47/why-your-non-significant-benchmark-result-might-be-a-power-problem-not-a-model-problem-5god

**Post 1/6**  
A non-significant result (`p = 0.71`) does not automatically mean "no model improvement." It can also mean your benchmark is underpowered.

**Live:** https://x.com/a_beamlak/status/2052817001769259362

**Post 2/6**  
For Tenacious-Bench (216 binary tasks, baseline ~74%, alpha=0.05, power=0.80), MDE is about **+10.9 points**. That is too large if you care about +3 to +5 practical gains.

**Live:** https://x.com/a_beamlak/status/2052817109483225336

**Post 3/6**  
At n=216, approximate detection chance is low for small gains: +3 pts ~11%, +5 pts ~23%, +8 pts ~52%. Missing significance is expected in that range.

**Live:** https://x.com/a_beamlak/status/2052817212969210035

**Post 4/6**  
Task targets for v0.2 (80% power):  
+3 pts -> ~3,226 tasks  
+5 pts -> ~1,128 tasks  
+8 pts -> ~420 tasks

**Live:** https://x.com/a_beamlak/status/2052817299107729801

**Post 5/6**  
With 2,000 bootstrap samples, `p = 0.0` is invalid. Minimum attainable empirical p is `1/(B+1)=1/2001≈0.0005`.

**Live:** https://x.com/a_beamlak/status/2052817396755321274

**Post 6/6**  
Best reporting: `p ≈ 0.0005` (or `p < 0.001`). Main lesson: evaluation is score change; statistics is whether your benchmark could detect the change you care about. Full write-up: https://dev.to/bnobody47/why-your-non-significant-benchmark-result-might-be-a-power-problem-not-a-model-problem-5god

**Live:** https://x.com/a_beamlak/status/2052817657414525258