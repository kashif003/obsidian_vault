
---


#lrp

- [i] Compare the l1 norm based pruning with SHAPE  based pruning.

METHODOLOGY:
	1. first prune based on L1 magnitude of the weights.
	2. Make SHAPE score for the parameters and prune based on that.
	3. globally prune the layer/ channels based on top bottom percentage.

# Results

No, particular observation. Different model favor different results.
# Summary

<span style="color: var(--color-green)">This paper introduces a layer-wise pruning method for object detection networks that scores each convolutional layer's importance using a SHAP-inspired metric—the product of a layer's output activations and the gradient of the loss with respect to those activations, captured via PyTorch forward/backward hooks—as an alternative to traditional L1-norm (weight magnitude) scoring. For both methods, layers are ranked from least to most important globally, and the bottom 5% are pruned by zeroing out their weights, with no fine-tuning applied afterward to isolate the pure effect of each pruning criterion.</span>


- [>] NOTHING SPECIAL IN THIS PAPER
