

---

paper: [link]([https://pmc.ncbi.nlm.nih.gov/articles/PMC13505426/pdf/main.pdf](https://www.mdpi.com/2076-3417/16/16/8005))
Published on: 11 aug 2026
AI tool chat link: [link](https://claude.ai/chat/eaa828d7-01ea-4d65-bd43-e5aef52eeeac)

----


Buzz words: 
+ canonical feature representation.
	+ instead of using raw columns the define 6 universal feature categories.
+ domain-adversarial learning
	+ by this they are making  that 6 features are universal for all the datasets.

---
what they did?

+ They trained a model using only normal data from the source dataset (no attack data involved at all), then applied that model, unchanged, to detect attacks in target datasets that do contain labeled attacks — using those labels solely to measure detection performance, not to help the model learn.


---
Tools used:

+ S7-1200 PLC



---

Canonical Feature Extraction and Engineering

+ they make 6 feeatures across all domains irrespective of raw column count.
+ They also use regex based sensor detection which can align certain feature with respective canonical feature.
+ table 3 defines the canonical features.
+ They align the source and target datasets by taking the **union** of their canonical features — so the final feature space includes every feature seen in either domain, not just the shared ones. For any given sample, if a feature from that union isn't available in its dataset, it's zero-padded. They also generate a binary mask marking which features were actually present versus zero-padded, so the model can tell real zeros apart from missing data.


Model selection and distributional alignment

+ "They tried different combinations of feature subsets and detectors, and for each candidate, computed the Wasserstein distance between the distribution of anomaly scores produced on the source domain versus on the target domain. A lower distance meant the detector's notion of 'normal vs. abnormal' behaved consistently across domains, rather than treating the target domain as systematically different just because it's a different dataset. Under this criterion, PCA using the {flow, valve, pump} feature subset had the lowest distance among all candidates tried, so it was selected as the final detector


---

Anomaly detection and domain adversarial framework

+ they trained pca model on normal dataset.
+ "PCA is trained only on normal source data. For any incoming sample (source or target), it computes a reconstruction error score. A fixed threshold — set as the 5th percentile of scores on held-out normal source data — determines the cutoff: exceed it, and the sample is flagged as anomalous. This same threshold, derived purely from source data, is then applied unchanged to every target domain, without any recalibration. A lower threshold means more samples (including normal ones) exceed it, raising the false positive rate; a higher threshold reduces false positives but risks missing real attacks."



---

Domain adversarial representation learning

They trained an autoencoder-style network with a feature extractor (encoder) and a reconstruction branch (decoder). The encoder's output was also fed into a domain classifier, whose job was to guess which domain the input came from. Training was set up so that the domain classifier, working from the encoder's output, would fail to guess correctly — this was achieved by flipping the sign of the gradients flowing back from the domain classifier into the encoder (via a gradient reversal layer), so the encoder was effectively pushed to sabotage the classifier's performance rather than help it. The encoder's overall training objective combined two things: minimizing reconstruction loss (rebuild inputs well) and maximizing domain-classification loss (make the classifier's job harder), weighted together. The domain classifier itself was trained normally, trying to minimize its own classification loss — it just kept losing that fight because the encoder was actively working against it.



---

linked papers:

+ check the section 2.2 of this paper, lot of linked papers.


---

outlook

1. domain adversarial representation learning was important to make sure the model doesnt remember the domain from which the data came from.




---

TODO

- [ ] read the results
- [ ] ask about NN auto encoder in detail.

