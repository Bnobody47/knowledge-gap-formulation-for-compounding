# Question (final, post–morning call)

**Topic / subtopic:** Training and post-training mechanics — preference tuning, reward shaping, and evaluator behavior

**Sharpened question:**
In my Week 11 `Sales Agent Evaluation Bench`, I use preference-style training and evaluator-based scoring to reduce `bench_overcommitment` and improve cautious, evidence-aware responses. What I still cannot defend is the mechanism-level effect of post-training objective choice (DPO vs ORPO/SimPO-style objectives) on this behavior: which parts of the model’s token-level decision policy are actually shifted by each objective, and why can one objective improve instruction-following while also increasing style artifacts or evaluator gaming in held-out data?

**Grounded in my work:**  
This gap is grounded in Week 11 artifacts where I selected Path B and implemented preference-oriented training/evaluation (`methodology.md`, `training/train_path_b_orpo.py`, `training/train_path_b_preference.py`, `scoring_evaluator.py`). Closing it would let me justify objective choice more rigorously, redesign my ablations, and better separate true behavior improvement from score-optimizing artifacts.

---
Properties check: diagnostic · grounded · generalizable · resolvable in one explainer
