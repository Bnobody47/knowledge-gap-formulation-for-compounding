# Public thread (Day 3)

**Blog (full post):** https://dev.to/bnobody47/did-my-lora-learn-tenacious-style-or-just-memorize-augmented-patterns-3fi6

**Post 1/6**
In LoRA SFT, cross-entropy does not optimize “style” directly. It optimizes next-token probability at each position.

**Live:** https://x.com/a_beamlak/status/2052458185902927905

**Post 2/6**
So if your training set is mostly augmented variants of a small core, repeated token patterns can dominate gradient signals and drive fast loss convergence.

**Live:** https://x.com/a_beamlak/status/2052458342036000844

**Post 3/6**
LoRA update path is `W = W0 + B@A`: base weights are frozen; only A/B are trained. That means you learn a low-rank steering correction, not a full model rewrite.

**Live:** https://x.com/a_beamlak/status/2052458451599519746

**Post 4/6**
Interpreting modules matters: attention updates (`q/k/v/o`) often suggest context-routing changes; MLP updates (`gate/up/down`) often suggest phrase/tone shaping. You need measurements, not guesses.

**Live:** https://x.com/a_beamlak/status/2052458574173929753

**Post 5/6**
A real Delta A lift can still be ambiguous. To separate style learning vs memorization, use grouped holdout by original email family + LoRA gradient norm analysis by module.

**Live:** https://x.com/a_beamlak/status/2052458672047931523

**Post 6/6**
Best claim: “Improved next-token policy on measured distribution; validating generalization beyond augmentation families.” Full write-up: https://dev.to/bnobody47/did-my-lora-learn-tenacious-style-or-just-memorize-augmented-patterns-3fi6

**Live:** https://x.com/a_beamlak/status/2052458862339399840
