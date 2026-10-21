


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
ans: the paper doesnt mention any of that but due to contrastive loss the model can detect any other anomaly other than these 5 which are injected but it is not guaranteed.


removing of domain bais:
the discriminator is train in GAN style which classifies if the datapoint is  form source or target domain but by using the reversal gradient technique on descriminator they change the features such that the domain specific bais is removed form the features.

IMPORTANT:
Initially the discriminator is good at telling the domains apart, and it keeps being trained throughout, but the gradient reversal layer flips the gradient only for the feature extractor, so the feature extractor learns to remove the domain bias. That is likely why the discriminator ends up unable to tell the datapoints apart, though this is good evidence rather than proof.



cross entropy classifier:
The CEC extends DeepSVDD by using source labels. An MLP on top of the TCN maps each window to a point, and a learnable center c (initialization not stated in the paper) is trained with a BCE-style loss on the labeled source domain, pulling normal windows toward c and pushing real anomalous ones away. At inference, c is frozen and a target window’s squared distance to c is its anomaly score, which is thresholded to flag anomalies.


overall loss fucntion has  therefore 4 parts:

1. supervised contrastive loss
2. self-supervised contrastive loss
3. Discriminator loss
4. classifier loss 

all these loss functions are combined then as a weighted sum.


---

experiments

lists the models and dataset that they have used,

---

start form baseline comparison



---

Potential papers:
1. [TSAD by diffusion models](https://dl.acm.org/doi/full/10.1145/3783986) 
2. **important**: LSTM based encoder decoder for TSAD [link](https://arxiv.org/pdf/1607.00148)
3. 


check the models they have used in the section V baselines.
