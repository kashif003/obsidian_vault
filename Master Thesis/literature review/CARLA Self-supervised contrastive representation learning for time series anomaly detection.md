

---

paper: [link](https://arxiv.org/abs/2308.09296)
Published on: 97 aug 2024
AI tool chat link: [link](https://claude.ai/chat/375cfe66-f5d7-4205-91fb-8905b1be8d7d)


---

Brief summary:

+ Self supervised anomaly detection method which works without labels.

The suggested methods has two stages:

1. Pretext stage:
	+ anomaly injection: inject the anomaly in the window and have labelled window with anomaly. types(global, contextual, seasonal, trend, shapelet) (these types have been described in [[Deep learning for  time series anomaly detection A Survey 1#2.4 Anomalies in time series dataset.| Note]])
	+ for multivariate data only few dimensions were corrupted

	+ building triplet: 
		+ anchor: normal window.
		+ positive: random earlier window
		+ negative: anchor with injected anomaly
	
	+ Triplet loss: resnet 18 with MLP last layer and 128 dims in latent space, model is trained to push the anomaly window far away form the normal anchor at the same time bring the normal close to anchor.
	+ finding neighbours: all windows are embedded and the intuition is that a normal windows nearest neighbours are other normal windows and farther neighbour is odd window.

2. Self-supervised classification:
	+ the backbone of resnet is used for classifying 10 classes.
		+ 10 classes are the clusters that model discovers.
	+ similarity and consistency loss. one brings   the positive close to the anchor and another pushes negative far away
	+ entropy loss is also used so that  the model does'nt dump all the samples into one class 

Inference

After training, while doing the inference the class with the highest number of datapoints is labelled as normal and other classes with less datapoints are labelled as anomaly-like

<span style="color: var(--color-red)">Q. what if normal class and other second class is very close and then some normal end up in another class,  so we will get false positives again?</span>


---


Section 1:

contrastive learning increases the  distance between the anchor and negative sample(anomaly) at the same time decreases distance between the anchor and normal. 

the distance between the normal and anomaly data point in TS data is not enough because of which we get FP'S. 

They first trains a ResNet with a triplet loss, where the positive is a temporally nearby window and the negative is the same window with a synthetic anomaly injected. 
This makes normal windows cluster together and separates them from anomaly-like windows. It then finds each window's nearest and furthest neighbours in that embedding space and uses them as pseudo-labels to train a second, self-supervised classification stage that sorts windows into clusters, with the majority cluster treated as normal. The anomaly score is the probability of not belonging to that majority cluster.



---

Related work:

ML methods that have been used:
ARIMA model, clustering models, decision trees, density based Algos


Deep learning methods:
OmniAnomaly (unlabelled anomalies), AE, VAE, transformers
LSTM-VAE,  DITAN,  THOC (excel in ,minimising forecasting errors while keeping the temporal dependencies)


---
**IMPORTANT** (novalety of paper)
TNC and T-Loss were built to learn scalable representations using time-based negative sampling, triplet loss and signal smoothness, while RoSAS applies triplet loss to outlier detection on non-time-series data.
contrastive learning in TSAD it is used by TS2Vec (hierarchical, multi-level representations) and DCdetector (dual attention with a pure contrastive loss). 
The gap, as the paper frames it, is that these time series methods learn representations at particular semantic levels and rely on invariance assumptions (augmented windows are positives, distant windows are negatives) borrowed from image work, which can wrongly turn anomalies into positives or normal windows into negatives, so normal and anomalous scores blend together), while the triplet-loss outlier work isn't designed for time series.
CARLA addresses this by dropping those assumptions: it uses temporally close windows as positives, injects synthetic anomalies (global, contextual, seasonal, trend, shapelet) as negatives so the model learns what deviations look like, and adds a second self-supervised classification stage over nearest/furthest neighbours to further separate real anomalies from normal windows.

---

Methods used:

Anomaly injection:

+ they have injected point anomalies and subsequence anomalies
	+ point anomalies: global and contextual (fig 3: b, c)
	+ subsequence anomalies: selected a particular window and introduced anomalies in that window which include seasonal , shapelet and trend (fig 3: d,e,f)


Pretext stage:
#contrastivelearning #tripletloss #resnet
1. Contrastive representation learning:
	+ use resnet model for learning a discriminative representation learning, the channels for resnet model represent different TS dimension

	+ triplets(a,p,n) are created a is the anchor, p is is positive where are smaples in temporal close proximity of the anchor and n is negatives these are the anchor samples but have been augmented with anomaly.

so the model is trained to increase the distance between the anchor and negative pair and at the same time distance is reduced between the anchor and positive pair.



2. Nearest and furthest neighbour:
	+ so here we get the index for the the samples which are near and which are futher
	+ stage 1 embeds the anchors and injected negatives, Part Two records each one's 5 nearest and 5 furthest neighbours, and stage 2 treats near as "should share a class" and far as "should not".

Self supervised classification stage:

Stage 2 copies the stage 1 ResNet and swaps the head for a softmax over 10 classes. It trains it so that each window and its 5 nearest neighbours get the same class (consistency), each window and its 5 furthest neighbours get different classes (inconsistency), and an entropy term stops all windows collapsing into one class.



Inference


At inference time, the classification is done and if the models has the majority samples in one class then it is marked as normal class and if the model  has datapoints in other classes then those classes are labelled as  anomaly.

---

Datasets used:
#swat #wadi #nasa 
NASA dataset: anomaly about the spacecraft monitoring systems.
SERVER MACHINE dataset: 28 servers over 10 days.
SWaT: 51 sesnors, 11 days, 41 deliberately generated anomaly over 4 days.
WATER distirbution testbed (WADI): dataset from scaled down urban water distribution systems.


other datasets are Yahoo, key performance indicator


Benchmarking methods:

in the section 4.2

evaluation setup:

in section 4.3


---
Section 4.5.1

they have shown the difference in effect of classification stage and pretext stages

---



Paper outlook:

- Check the Github repo.
	- maybe replicate the results provided
- check the another paper.


