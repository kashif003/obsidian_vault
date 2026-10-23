
---

PAPER:


 The paper compares five head-importance scoring methods on **ViT-Large** (`google/vit-large-patch16-384`, 384 heads) on **ImageNet-1K**, pruning 10%–60% of heads with **no fine-tuning** afterward.

- [ ] need to see if we need to add other transformers or other vits

Methods so far used:


| Method         | Scoring idea                                                                                                                        |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| **Magnitude**  | Non-gradient baseline: ℓ2 norm of each head's Q, K, V weights                                                                       |
| **GMAR**       | ℓ1 norm of absolute gradients of the predicted class logit w.r.t. the attention matrix (positive and negative gradients both count) |
| **GMAR++**     | ReLU on gradients (drops negatives), then ℓ2 norm (emphasizes a few large values)                                                   |
| **LeGrad**     | ReLU on gradients, then plain sum                                                                                                   |
| **Chefer CAM** | Gradient weighted by LRP-style relevance (attention × gradient, normalized), then ReLU and sum                                      |

**Pipeline details**

- Gradient scores are accumulated over 1,000 ImageNet validation images.
- The attention-rollout stage of each original method is omitted, since the goal is ranking heads, not saliency maps.
- The lowest-scoring k% of heads are removed **globally** using `torch-pruning`.
- Its DependencyGraph keeps Q, K, V and the output projection consistent.
- Each pruned head removes 64 contiguous channels.
- Heads are pruned in reverse index order, and `num_attention_heads` is decremented manually.
- Evaluation: Top-1 on the full 50,000-image validation set, NVIDIA A100 40GB, deterministic single run per configuration.

Results:

- Parameters and FLOPs fall **linearly** with the pruning ratio.
- At 60% head pruning: **246.4M** params (vs. 304.1M) and **141.5G** FLOPs (vs. 174.8G), a reduction of only about **19%**.
- The reduction is small because the FFN blocks are left untouched.


| Method     | 10%       | 20%       | 30%       | 40%       | 50%       | 60%       |
| ---------- | --------- | --------- | --------- | --------- | --------- | --------- |
| LeGrad     | 82.02     | 77.97     | 69.21     | 51.24     | 27.70     | 6.66      |
| Chefer CAM | 82.48     | 78.28     | 70.16     | 51.64     | 24.33     | 6.65      |
| Magnitude  | 82.39     | 71.94     | 54.90     | 38.70     | 11.24     | 6.62      |
| GMAR       | **83.98** | 81.92     | **77.18** | 67.01     | **53.80** | **40.45** |
| GMAR++     | 83.85     | **81.95** | 76.57     | **67.76** | 53.28     | 38.33     |

- GMAR is best at 10%, 30%, 50%, 60%; GMAR++ is best at 20% and 40%.
- Magnitude is competitive at 10% but collapses at higher ratios (6.62% at 60%).
- LeGrad and Chefer CAM behave similarly and fall behind at higher ratios.


Gradient-based scores (GMAR, GMAR++) are substantially better proxies for functional head importance than weight norm, and they also beat the ReLU-gradient methods LeGrad and Chefer CAM.


1. **Coarse granularity:** whole heads are removed; finer-grained pruning of individual attention dimensions is not compared.
2. **Global, isolated scoring:** cross-layer cascading effects are ignored.
3. **Future work:** post-pruning fine-tuning, layer-aware pruning, finer-grained pruning, and evaluation on Swin and DeiT.


- Reference 17 still contains the note "Verify exact page range/author order before submission."
- The paper calls GMAR "sign-aware," but its formula uses an absolute value, so it treats gradient sign symmetrically. GMAR++, LeGrad and Chefer CAM are the ones that discard negative gradients. The Discussion's contrast of "sign-aware (GMAR, GMAR++)" vs. "ReLU-filtered (LeGrad, Chefer CAM)" is therefore inconsistent, since GMAR++ is also ReLU-filtered. The real differentiator may be the aggregation (ℓ1/ℓ2 vs. plain sum), which is worth clarifying.


