

---
+ **Original paper (Yeom et al., 2021):** used LRP with heuristically-chosen hyperparameters to score and prune CNN filters.
+ **This paper (Hatefi et al., 2024):** kept the same core idea (score with LRP → prune lowest-relevance units) but 
	+ (a) explicitly optimized LRP's hyperparameters for the pruning objective on CNNs, and 
	+ (b) extended the same optimized-hyperparameter approach to transformers by swapping in AttnLRP as the attribution method, since vanilla LRP doesn't handle attention layers well.



GAPS:
	1. campare with different XAI **optimized** methods.
	2. no check for class wize cascading degredation.
	3. no mention of the hyperparameter optimization cost.



- [ ]  Need to ask gpt about the cache storing instead of params to reduce the flops or shortcut for the prediction based on any pattern.

- [ ] I have cloned the repo on server need to run the code.