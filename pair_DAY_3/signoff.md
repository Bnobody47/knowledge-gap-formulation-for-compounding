# Asker sign-off

**Status:** closed

**Paragraph:**
I now understand that SFT cross-entropy in my LoRA run measured next-token prediction quality, not “style learning” directly. The explainer made clear how gradients flow through the frozen base model while only LoRA `A/B` are updated, and why a high-augmentation dataset can create repeated gradient directions that improve metrics without proving broad style generalization. I also now have concrete diagnostics to defend claims: grouped holdout by original-email family and gradient norm comparisons across target modules. This closes the gap between “loss converged” and “the adapter learned what I intended.”

(Replace with Yakob Dereje's exact final sign-off wording if required.)
