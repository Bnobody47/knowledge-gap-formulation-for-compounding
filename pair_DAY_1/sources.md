# Sources

- **Canonical source 1:** Vaswani et al., *Attention Is All You Need* (transformer attention mechanics). https://arxiv.org/pdf/1706.03762
- **Canonical source 2:** Xiao et al., *StreamingLLM: Efficient Streaming Language Models with Attention Sinks*. https://arxiv.org/pdf/2309.17453
- **Canonical source 3 (authoritative docs):** Hugging Face Transformers caching docs (KV cache behavior and generation). https://huggingface.co/docs/transformers/cache_explanation
- **Canonical source 4 (serving docs):** vLLM prefix caching design. https://docs.vllm.ai/en/stable/design/prefix_caching/

- **Tool / pattern exercised:** Inspected Week 11 evaluator scoring logic and drift-sensitive cues in `C:\Users\Bnobody_47\Documents\Sales Agent Evaluation Bench\scoring_evaluator.py` and mapped them to decode-time drift risks discussed in the explainer.

Optional follow-ons:
- Liu et al., *Lost in the Middle: How Language Models Use Long Contexts*. https://arxiv.org/abs/2307.03172
- Anthropic prompt caching docs. https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching
