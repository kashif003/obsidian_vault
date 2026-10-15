
#lrp

1.<span style="color: var(--color-yellow)"> LRP based compression methods.</span>
	+ negative/ zero relevance is removed completely.
2.<span style="color: var(--color-yellow)"> Does pruning and mixed precision quantization.</span>
	+ take median of positive scores and if a certain score is higher than it then do higher precision quantization other size do lower precision quantization.

<span style="color: var(--color-red)">Q. WHY LRP?</span>
	Gradient based LRP avoids the weakness of DeepLIft.
	<span style="color: var(--color-red)">Q. What is deeplift?</span>
		- another paper, which needs the reference for pruning the parameters.

<span style="color: var(--color-yellow)">ALGORITHM:</span>
1. compute LRP per filter.
	+ what does this filter mean? 
		+ it means weights, *(as they have done experiment on MLP)*
2. sort score.
3. prune filters with negative scores.
4. compute median for remaining scores.
5. if filter score is higher than threshold then quantize to higher precision otherwise quantize to lower precision.

<span style="color: var(--color-yellow)">Experimental setup:</span>
+ Model: a **toy 3-layer dense MLP** (1000 hidden neurons/layer), not a CNN or any real vision backbone.
+ Dataset: a **synthetic 4-class toy dataset called "multi"**, generated programmatically — not CIFAR, not ImageNet, not any standard CV benchmark.


# Results


1. Model size: 8MB → 0.9MB (pruning + mixed-precision quant)
2. Accuracy: 94.1% (original) → 90.5% best compressed variant

# Summary


<span style="color: var(--color-green)">They used LRP to compute an importance score for each weight (called "filters" in the paper, though the model is a dense MLP). Weights with negative or zero LRP score are pruned. The remaining (positive-score) weights are compared against the median score of their own layer, and quantize to higher bit-width if above the median, or lower bit-width if below. </span>


