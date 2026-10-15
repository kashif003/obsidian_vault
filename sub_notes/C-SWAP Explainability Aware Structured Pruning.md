

link: https://github.com/ThalesGroup/C-SWAP

---




- [I] Uses **causal inference** to classify neurons into three categories:
	1. <span style="color: var(--color-red)">critical</span> - removing it will hurt performance.
	2. <span style="color: var(--color-yellow)">neutral</span> - removing it has no significant effect.
	3. <span style="color: var(--color-green)">Detrimental</span> - removing it helps performance 

<span style="color: var(--color-red)">Q. what is causal inference?</span>
	cutting down the output of certain neuron  and observing the change in model prediction, and comparing this change without cutting the neuron output.

<span style="color: var(--color-red)">Q. What do they prune?</span>
	They perform structured pruning.
	<span style="color: var(--color-blue)">vit</span>- MLP neurons and heads.
	<span style="color: var(--color-blue)">CNN</span> - filters/channels
	so, they block the output of neuron in MLP, attention head in transformer block and channel in CNN.


ALGORITHM:
1. Prune layer by layer.
2. For each neuron compute causal effect on output.
3. if the neuron is not critical it is removed.
4. if more pruning is required then critical neurons are removed.

ALGO SUMMARY:
<span style="color: var(--color-green)">Starting from the output layer and moving backward toward the input layer, and within each layer going neuron by neuron, they run causal inference on each neuron given the network **as it currently stands** (i.e., reflecting any prior removals). If a neuron is not critical, it's pruned immediately, and the process continues to the next neuron on this now-smaller network. If a neuron is critical, it's kept and set aside rather than pruned. This continues neuron by neuron, layer by layer, until the whole network has been processed — after which, if more pruning is still required, the previously set-aside critical neurons are pruned too, ranked by causal effect (least critical first).</span>


HOW SCORING IS DONE?
	They use two things together:
	1. **causal effect score (ξ)** 
	2. **statistical significance test**
	combined via a **voting rule across classes**.
![[Pasted image 20260725155123.png]]

	A raw ξₙ value alone isn't enough — they need to know if that change is _statistically meaningful_ or just noise. So, for each class k separately, they run a **paired t-test** comparing the distribution of σₙ*(x) vs σ(x) on that class's samples, at significance level **α = 0.05**. This gives a binary predicate π_n^(k): true if the neuron's effect is statistically significant for class k, false otherwise.


EVALUATION:
They introduce new metric called SAUCE(sparsity AUC estimator), i.e area under accuracy vs pruning % curve.


# Results

CLAIM:
1. C-SWAP consistently beats baselines (random pruning, OMP, various XAI attribution methods like LRP/DeepLIFT/Integrated Gradients/Conductance, and an adapted mechanistic-interpretability pruner "AMP") on SAUCE.
2. It can remove up to ~50% of ResNet-18 parameters with no accuracy loss, without fine-tuning.
3. It also extends successfully to semantic segmentation (DDRNet/Cityscapes), maintaining performance up to ~40% pruning.

XAI METHODS USED:
	1. Integrated gradients
	2. ILRP
	3. internal influence
	4. Conductance
	5. AMP
1. 

# LIMITATIONS


Computing per-neuron causal relevance is expensive for very wide layers (e.g., ~35 hours for ViT on a single RTX 3090)



# Summary

<span style="color: var(--color-green)">They perform causal inference progressively, going through the model layer by layer from output to input, and within each layer, neuron by neuron. For each neuron, they cut its outgoing connections and compute two things: (1) a global causal effect — the average relative change in correct-class prediction probability across all sample classes combined — and (2) a separate statistical significance test (paired t-test) run independently for each class, checking whether the neuron's effect on that specific class is real or just noise. These per-class results are then combined via a voting rule: if the neuron is significant for at least one class and the global causal effect is negative (removing it hurts performance), it's marked critical and preserved; if significant for at least one class with a positive effect (removing it helps), it's detrimental and pruned; if not significant for any class, it's neutral and pruned. Non-critical neurons are pruned immediately as the process moves through the network, while critical neurons are only revisited for pruning at the very end if further compression is still required.</span>





# Revision of Paper summary


Here are the three steps in simple, short sentences:

**Step 1 — Compute causal effect:** For each neuron, temporarily cut its output and measure how much the model's confidence in the correct class changes, averaged across all sample classes. This gives a single score, ξₙ.

**Step 2 — Test for statistical significance:** For each class separately, check whether the change caused by cutting the neuron is a real effect or just random noise, using a statistical test (paired t-test) with a 5% significance threshold.

**Step 3 — Vote and classify:** If the neuron's effect is significant for at least one class, classify it as **critical** (kept) when ξₙ < 0, or **detrimental** (pruned) when ξₙ > 0. If it's not significant for any class, it's **neutral** (pruned).


- [/] SAUCE metric can be important here.
