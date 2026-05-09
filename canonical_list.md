# Canonical list

## Papers and technical references

- **Attention Is All You Need** - https://arxiv.org/pdf/1706.03762  
  Foundation for attention mechanics and token-level decoding intuition used in Day 1 analysis.

- **StreamingLLM (attention sinks)** - https://arxiv.org/pdf/2309.17453  
  Useful for separating structural attention mass from semantic instruction-following.

- **LoRA: Low-Rank Adaptation** - https://arxiv.org/abs/2106.09685  
  Core reference for understanding why only low-rank adapter paths update during SFT.

- **Permutation P-values should never be zero** - https://pubmed.ncbi.nlm.nih.gov/21044043/  
  Critical reporting guardrail for finite resampling workflows (`p = 0.0` correction).

- **NAACL industry function-calling format study** - https://aclanthology.org/2025.naacl-industry.9.pdf  
  Strong evidence that prompt/tool format choices materially affect function-calling behavior.

## Runtime and implementation docs

- **DeepSeek V3.2 encoding/parser (open-weight)** - https://huggingface.co/deepseek-ai/DeepSeek-V3.2/blob/main/encoding/encoding_dsv32.py  
  Essential for understanding what is actually emitted vs what API objects normalize to.

- **vLLM tool-calling docs** - https://docs.vllm.ai/en/stable/features/tool_calling/  
  Practical map of constrained vs auto behavior and parser implications.

- **OpenAI function-calling guide** - https://developers.openai.com/api/docs/guides/function-calling  
  Clear reference for strict schema-constrained decoding expectations.

- **Transformers cache explanation** - https://huggingface.co/docs/transformers/cache_explanation  
  Good implementation-level grounding for KV cache behavior.

- **Statsmodels two-proportion power function** - https://www.statsmodels.org/dev/generated/statsmodels.stats.proportion.power_proportions_2indep.html  
  Reproducible basis for MDE and benchmark-size planning.

## Project patterns worth reusing

- **Policy-memory vs trajectory-memory separation**  
  Keep system/rubric rules immutable; summarize only evolving state.

- **Grouped-holdout for augmentation-heavy datasets**  
  Split by source family to avoid sibling leakage and false generalization confidence.

- **Module-level gradient norm instrumentation for LoRA**  
  Use `q/k/v/o` vs `gate/up/down` pressure as mechanism evidence alongside metrics.

- **Power-aware benchmark planning before reporting outcomes**  
  Decide smallest meaningful effect first, then size benchmark accordingly.
