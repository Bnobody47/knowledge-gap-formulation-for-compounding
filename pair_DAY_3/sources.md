# Sources

- **Canonical source 1:** Hu et al., *LoRA: Low-Rank Adaptation of Large Language Models*. https://arxiv.org/abs/2106.09685
- **Canonical source 2:** Goodfellow et al., *Deep Learning* (cross-entropy / softmax fundamentals, Ch. 6) and standard language-model training formulation. https://www.deeplearningbook.org/
- **Canonical source 3 (project-specific):** Week 11 Tenacious-Bench prompt/spec and training artifacts (SFT + LoRA path, datasheet limitation statement).
- **Canonical source 4 (project style anchor):** Tenacious Style Guide v2 (tone markers, good/bad examples, policy constraints).

- **Tool / pattern exercised:** Reviewed Day 3 research synthesis `Tenacious_Bench_Training_Mechanics_Answer.pdf` and mapped its mechanism claims to concrete diagnostics: grouped holdout split by original-family and LoRA gradient-norm logging by module group.

Optional follow-ons:
- Raffel et al., *Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer* (token-level objective framing). https://arxiv.org/abs/1910.10683
- PEFT docs for practical LoRA instrumentation. https://huggingface.co/docs/peft
