# Sources

- **Canonical source 1:** DeepSeek V3.2 open-weight encoding/parser (`encoding_dsv32.py`) and model card docs. https://huggingface.co/deepseek-ai/DeepSeek-V3.2/blob/main/encoding/encoding_dsv32.py
- **Canonical source 2:** OpenAI function-calling/structured outputs guidance (decoder-constrained strict mode). https://developers.openai.com/api/docs/guides/function-calling
- **Canonical source 3 (runtime behavior):** vLLM tool-calling documentation (auto vs constrained behavior matrix). https://docs.vllm.ai/en/stable/features/tool_calling/
- **Canonical source 4 (format sensitivity evidence):** NAACL industry paper on function-calling prompt format effects. https://aclanthology.org/2025.naacl-industry.9.pdf

- **Tool / pattern exercised:** Analyzed the Day 2 research report `DeepSeek V3.2 Tool Selection Tokens and System-Instruction Adherence.pdf` and mapped its token/protocol findings to practical parser/runtime failure modes and mitigation patterns.

Optional follow-ons:
- DeepSeek API chat completion docs (`tool_choice`, strict beta). https://api-docs.deepseek.com/api/create-chat-completion
- OpenAI structured outputs blog. https://openai.com/index/introducing-structured-outputs-in-the-api/
