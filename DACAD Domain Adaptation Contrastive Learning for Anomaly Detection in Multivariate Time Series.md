


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
