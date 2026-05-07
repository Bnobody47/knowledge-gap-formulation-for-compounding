# Did the LoRA adapter learn Tenacious style, or memorize augmented patterns?

**Partner question (Yakob Dereje):** In Week 11 Tenacious-Bench SFT, what does cross-entropy mean token-by-token, how do gradients flow through frozen base weights into LoRA `A` and `B`, which target modules likely move most (`q/k/v/o` vs `gate/up/down`), and how do we tell style generalization from surface-pattern memorization when 94.3% of data are augmentations of 128 originals?

## Short answer

Your Delta A improvement can be real and reproducible while still being ambiguous about *what* was learned. In LoRA-SFT, cross-entropy only optimizes next-token prediction on the training distribution. With heavy augmentation concentration, repeated gradient signals can reward recurring phrase templates and local token patterns. To defend true style learning, you need diagnostics beyond convergence: grouped holdout by source family plus module-level LoRA gradient analysis.

## 1) What cross-entropy is measuring token by token

In autoregressive SFT, the model predicts token `t_i` given prior context `t_<i`. At each step it produces logits over the vocabulary, applies softmax, and receives loss based on the probability assigned to the gold token.

- If `P(gold token)` is high -> low loss.
- If `P(gold token)` is low -> high loss.

So cross-entropy is not directly scoring "Tenacious voice" as an abstract concept. It scores local next-token accuracy. Style enters indirectly because your targets contain Tenacious-style outputs.

## 2) How gradients flow in LoRA with frozen base weights

For a target weight matrix:

`W = W0 + DeltaW`, where `DeltaW = B @ A`

- `W0` (base model) is frozen.
- `A` and `B` are trainable.

Backprop still traverses the whole network graph, but optimizer updates only `A/B`. That means LoRA learns a low-rank steering correction over the base model, not a full model rewrite. Mechanically, this is why LoRA can shift behavior with relatively few trainable parameters.

## 3) Interpreting the seven target modules

You targeted:

- **Attention side:** `q_proj`, `k_proj`, `v_proj`, `o_proj`
- **MLP side:** `gate_proj`, `up_proj`, `down_proj`

A practical interpretation framework:

- Larger attention-side update mass can indicate stronger context-to-decision routing (e.g., weak signal -> ask/qualify instead of assert).
- Larger MLP-side update mass can indicate stronger lexical/phrase transformation (useful for tone shaping, but also vulnerable to memorizing repeated phrase templates).

This is suggestive, not absolute. You need gradient norms and behavior tests to make defensible claims.

## 4) Why low diversity becomes a gradient-level limitation

Your datasheet says 94.3% of training pairs are augmented variants of 128 originals. This matters mechanically:

- Near-duplicate examples produce highly aligned gradients.
- Repeated aligned gradients push LoRA parameters in similar directions repeatedly.
- Loss falls quickly on recurring token patterns.

This can look like strong learning even when the adapter mostly captures surface regularities (openers, CTA phrasings, sentence shells) instead of general style policy.

So "loss converged" is necessary but not sufficient evidence of generalization.

## 5) How to distinguish style learning vs memorization

### Diagnostic A: Grouped holdout split by original family

All augmentations derived from one original email must stay in the same split. If siblings appear in both train and held-out, memorization can masquerade as generalization.

- Stable performance under grouped holdout -> stronger evidence of style generalization.
- Sharp drop under grouped holdout -> evidence that gains depended on augmentation-family overlap.

### Diagnostic B: LoRA gradient norm comparison by module

Log gradient norms during training and aggregate by module group (`q/k/v/o` vs `gate/up/down`).

Use this as mechanism evidence:

- Where is training pressure concentrated?
- Does module pressure align with expected behavior changes?
- Are gains dominated by phrase-shaping layers with weak robustness on grouped holdout?

### Minimal runnable check (hands-on)

Use a grouped split and gradient logging in the same training/eval run:

```python
# grouped split sketch (group_key = original_email_id)
from sklearn.model_selection import GroupShuffleSplit

gss = GroupShuffleSplit(n_splits=1, test_size=0.2, random_state=42)
train_idx, test_idx = next(gss.split(X=data, groups=original_email_id))

# gradient norm logging sketch
grad_sums = {"attn": 0.0, "mlp": 0.0}
for name, p in model.named_parameters():
    if "lora_" in name and p.grad is not None:
        g = p.grad.norm().item()
        if any(k in name for k in ["q_proj", "k_proj", "v_proj", "o_proj"]):
            grad_sums["attn"] += g
        if any(k in name for k in ["gate_proj", "up_proj", "down_proj"]):
            grad_sums["mlp"] += g
print(grad_sums)
```

Interpretation pattern:
- **Good signal:** grouped-holdout stays reasonably strong and cautious behavior remains evidence-conditional.
- **Memorization risk:** grouped-holdout drops hard while training/standard held-out stays high, especially with strong repetitive phrase reuse.

## 6) Interpreting your Delta A claim safely

Your +0.263 (p < 0.0001) is meaningful on the measured evaluation setup. The defensible claim is:

"The adapter improved next-token policy on this distribution; we are validating whether this reflects generalizable style learning or augmentation-family memorization."

That is stronger than claiming generalized style learning without diagnostics.

## Scope discipline

This explainer focuses on SFT + LoRA training mechanics and evaluation interpretation for the Tenacious-Bench setup. It does not attempt full post-training objective comparisons (e.g., DPO/ORPO) beyond this question's scope.

## Takeaway

In LoRA-SFT, cross-entropy rewards token prediction, not style abstractions directly. With low-diversity augmented data, repeated gradients can create real metric gains from surface-pattern reinforcement. To defend true style learning, pair statistical lift with grouped holdout and gradient/module diagnostics.
