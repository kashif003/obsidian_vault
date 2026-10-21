


---


paper_Link: [link](https://arxiv.org/pdf/2404.11269?)

AI_tool: [link]()

published date: sept 2025.


---

brief summary:

it addreses that gap in unsupervised domain adaptation:
existing methods assume the source and target domains share the same anomaly classes, so they fail when the target has different kinds of anomalies. The model is trained once, jointly, on labeled source data and unlabeled target data, then applied to the target

for more generalization they inject anomalies what are described in [[Deep learning for  time series anomaly detection A Survey]], these introduced anomalies are treated as negatives for the contrastive losses.

Source domain has labels so they perform supervised leanring, for target domain they do self supervised learning. same as they did in [[CARLA Self-supervised contrastive representation learning for time series anomaly detection]] 

they use a TCN(temporal convolutional network) as feature extractor and a descriminator  with gradient reversal technique which makes the features domain invariant.

They also use center based cross entropy (check below what it does)..



----

Things they do:

+ perform supervised and self supervised classication, supervised is done source domain where we have the info about the anomaly and normal and self supervised is done where we have no idea.
+ they do the same thing that is done in [[CARLA Self-supervised contrastive representation learning for time series anomaly detection]] where they injected anomalies and performed contrastive learning
+ they make use of center  based entropy and bring the normal sameples close to each other at the same time push the anomaly  samples away form each other.




anomaly injection:
in source domain anchor is a normal window and positive is another normal window and negative is anomaly window(which is labelled) or injected normal window(which they make by anomaly injection).

in target domain  it is same as [[CARLA Self-supervised contrastive representation learning for time series anomaly detection]]

<span style="color: var(--color-red)">
Q: if the anomaly injection is same what is the point then, wont they have same anomalies in target domain as well?</span>













---

Potential papers:
1. [TSAD by diffusion models](https://dl.acm.org/doi/full/10.1145/3783986) 
2. 
