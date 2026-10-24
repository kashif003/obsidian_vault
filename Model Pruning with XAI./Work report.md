
---

## What's done
- **Setup:** ViT-Large (`google/vit-large-patch16-384`, 384 heads) on ImageNet-1K. Heads are pruned globally at 10-60%, with **no fine-tuning**.
- **Scoring methods compared:** Magnitude (weight-norm baseline), GMAR, GMAR++, LeGrad and Chefer CAM. All gradient methods use 1,000 val images, and rollout is omitted.
- **Pruning:** `torch-pruning` DependencyGraph. Each head removes 64 channels from Q/K/V/out_proj, in reverse index order, and `num_attention_heads` is decremented manually.
- **Evaluation:** Top-1 on the full 50k val set (baseline 85.81%), A100, one deterministic run per config.

## Key findings
- **GMAR is best** at 10/30/50/60%, and **GMAR++** is best at 20/40%.
- At 60% pruning, GMAR keeps **40.45%** and GMAR++ keeps 38.33%. Magnitude, LeGrad and Chefer CAM all fall to about 6.6%.
- Magnitude is fine at 10% (82.39%) but collapses quickly after that.
- LeGrad and Chefer CAM behave almost identically.
- Size drops linearly, but at 60% head pruning params and FLOPs fall only about **19%** (304.1M → 246.4M, 174.8G → 141.5G), because the FFN is untouched.

## Open issues
- **Claim inconsistency:** GMAR uses an absolute value, so it is not sign-aware. GMAR++, LeGrad and Chefer CAM are the ones that drop negative gradients. The real differentiator may be the aggregation (ℓ1/ℓ2 vs. plain sum), so the Discussion needs rewording, or an ablation to back it up.
- **Reference 17** still has a "verify page range/author order" note.
- **Scope:** only one model (ViT-Large) has been tested.

## Next steps
- [ ] Decide whether to add other models (DeiT, Swin, smaller ViTs) to show the findings generalize.
- [ ] Run an ablation separating sign handling from aggregation, to resolve the claim inconsistency.
- [ ] Add post-pruning fine-tuning to see how much accuracy recovers.
- [ ] Try layer-aware pruning to address cross-layer cascading effects.
- [ ] Try finer-grained pruning (individual attention dimensions) and compare.
- [ ] Fix reference 17.