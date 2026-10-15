

---

paper: [link](https://dl.acm.org/doi/10.1145/3691338)  -
Published on: oct 2024  
AI tool chat link: [link]([[https://claude.ai/chat/7063c7d4-6f41-4d8b-9ee2-a89d211f5f0d](https://claude.ai/chat/64f2b605-b64f-4e7f-b8db-829599747750)](https://claude.ai/chat/6227226c-f26a-4bbe-9e32-78842426554c))


----

Brief summary:


This paper is a survey that gives a broad overview of time series anomaly detection using deep learning. It explains the basics of time series data and the types of anomalies, then organises the existing models into categories (**forecasting-based, reconstruction-based, representation-based, and hybrid**) and by architecture **(RNN, CNN, AE/VAE, GAN, GNN, Transformer, and so on)**. It does not go deep into the technical details of each model. Instead, it gives a short description of each one: what it does, the problem it tries to solve, and how it differs from earlier work.

---

2.3 Time series Decomposition


Time series (TS) can be decomposed into 4 componests:

+ Secular trend: long term trend in the series.
+ Seasonal trend: seasonal pattern which occurs at fixed frequency.
+ cyclical fluctuations: extended deviation form the underlying series defined by secular trend and seasonal variaitons.
+ irregular variations: random irregular events


### 2.4 Anomalies in time series dataset.
#typesofanomalies #gloabl #contextual #seasonal #trend #shapelet
In time series anomaly can be classified into three:

1. Temporal anomaly: unusual over time.
	+ these are judged either by nearby values or against whole series
	+ can occur in MTS as well and can effect single or multiple dimentions

types of temporal anomalies:

**a. Global:** spike with extreme value which is compared with whole series.
					$|x_t - \hat{x}_t| > \text{threshold}$
x= output of model, $\hat{x}$ = desired output of model.

**b. Contextual:** value that is fine globally but is wrong locally.*it has same formulla as above only the threshold changes*
							$\text{threshold} \approx \lambda \times \text{var}(X_{t-w:t})$
$\lambda$ controlling cofficient for threshold, var = vaiance of context,$X_{t-w:t}$ context of the data points

**c. Seasonal:** the shape or trend of series look normal but repeating pattern is off.
					$\text{diss}(S, \hat{S}) > \text{threshold}$
diss = fucntion measuring dissimilarity in seasonality, S = actual seasonality, $\hat{S}$ = expected seasonality

**d. Trend:** an event causes lasting shift in the mean or slope while cycle and seasonality stay intact.
					$\text{diss}(T, \hat{T}) > \text{threshold}$
**e. Shapelet:** A subsequence whose shape or cycle differ form the usual pattern in the rest of the series.
				$\text{diss}(C, \hat{C}) > \text{threshold}$
			
![[2026-10-03 11_15_00-Greenshot.png]]



as we can see we need to compare the normal data with the anomulous data to define the anomaly:

So how do we compare?

Dynamic time warping: #DTW #dynamictimewarping 
Dynamic time warping (DTW) is a method for measuring how different two time series (or subsequences) are. It matches each point in one series to one or more points in the other, allowing the alignment to stretch or compress along the time axis, and it finds the alignment with the smallest total mismatch. That minimum mismatch is the DTW distance. If the distance between an observed subsequence and the expected (normal) one exceeds a threshold, the subsequence is flagged as anomalous.


**Anomalies in multivariate time series**
#multivariate 

A multivariate time series (MTS) consists of more than one variable (dimension) recorded over time. These variables are usually related to each other, and the relationship can be linear or non-linear. An anomaly can break this relationship.

An **intermetric anomaly** is a break in the relationship between dimensions. Each series may look completely normal when examined on its own, but when it is examined together with another series, the problem becomes visible. For example, power consumption and CPU usage normally rise and fall together; if that link disappears while each metric still looks ordinary individually, the event is an intermetric anomaly.

$$
\max_{\forall j, k \in \mathcal{D}, j \neq k} 
\text{diss}_{\text{corr}} \left( \text{Corr}(X_{j}, X_{k}), \text{Corr}(X_{j, t+\delta_{tj}:t+w+\delta_{tj}}, X_{k, t+\delta_{tk}:t+w+\delta_{tk}}) \right) > \text{threshold} \quad
$$

the equation above is the difference between normal relationship and current relationship.

above equation in one sentence: *"For every pair of metrics, compare how they normally move together with how they are moving together in this window; if the biggest difference is too large, flag it."*



3.1.1 Temporal/Spatial: 
when we have the UTS input model learns temporal(accross time) informations. but for MTS model can learn both temporal and spatial dependencies.


3.1.2 learning schemes: unsupervised, supervised, semi-supervised, self supervised.

semi-supervised: when we know which data is normal.

self-supervised: model generates its own labels.


3.1.3: inputs: windows are common because it is possible to compare subsequences rather than just a point, window holds preceding data so we have contextual informations as well. we can use two windows sliding or shiffled window

suggestions: imputing missing values -> downdsampling -> normalizing data -> then making windows 


### 3.1.7 Incremental learning
#incrementallearning

- A machine learning paradigm in which the model's knowledge grows whenever new observations arrive, instead of being trained once and frozen.
- It is useful when training data becomes available gradually, and the goal is to adapt to new data while keeping what was learned before.

### Anomaly scores and thresholding

- **Anomaly score:** usually defined from the model's loss function.
    - Reconstruction-based models use the reconstruction error (or reconstruction probability).
    - Forecasting-based models use the prediction error.
- **DevNet** ([link](https://arxiv.org/pdf/1911.08623)): an end-to-end model that uses a few labelled anomalies to optimise anomaly scores directly, instead of first improving representations.
- **Thresholding methods** (turn scores into labels):
    - **NDT (Nonparametric Dynamic Thresholding):** sets the threshold dynamically.
    - **POT (Peaks-Over-Threshold):** based on extreme value theory.

### 3.2 Forecasting-based models

- They usually use a sliding window to predict the next value. A large gap between the prediction and the actual value (the prediction error) marks an anomaly.
- #forecastingmodels

**RNN, LSTM, GRU**

- LSTM-based models are more effective than plain RNN-based models, especially when it is unclear whether normal behaviour involves long-term dependencies.
- Limitations: LSTMs and GRUs struggle to learn complex seasonal patterns in multi-seasonal series, and recurrent models in general struggle with very long sequences because of their fixed window size.

**CNN models**
#cnn
- **TCN (Temporal Convolutional Network):** a 1D fully convolutional network, so the output has the same length as the input.
- **Causal convolution:** the output at time t uses only past data, with no future leakage.

### 3.3 Reconstruction-based models

- They can detect anomalies more accurately than forecasting-based models because they see the current window of data. The trade-off is a slight delay in detection, so they are preferred when accuracy matters and some delay is acceptable.
#reconstruction
**VAE (3.3.2)**

- Anomalies are flagged by low reconstruction probability or high reconstruction error.



----

Section 5: Evaluation metric for time series anomaly detection.

**SOTA datasets are in table 3**

**Metrics to use for evaluating TS models:**

check table 4. of paper, it also provides the guidlines.


---

Section 6: Interpretability metirc
#metric

**HitRate@P%**

- Tells us, in an MTS, how well the model identified which dimensions are responsible for an anomaly.
- Formula (Eq. 25): true causes found in the top P% of the model's ranked dimensions ÷ total true causes.
- Rewards finding **all** of the true causes.

**Interpretation Score (IPS)**

- Tells us whether the model pointed to the right dimensions after detecting an anomaly.
- Formula (Eq. 26): the fraction of true causes found in the model's top k dimensions, **averaged over all N anomaly segments**.
- Same idea as HitRate, but it gives one overall score across many segments.

**RC-top-k (Relevant Causes top-k)**

- Measures the fraction of anomaly events for which **at least one** true cause appears in the model's top k dimensions.
- Formula (Eq. 27): events with at least one true cause in the top k ÷ total events.
- Rewards finding **at least one** cause, so it is the most lenient of the three.

**RDCG@P% (Reconstructed Discounted Cumulative Gain), if you want to include it**

- Adapted from NDCG, a ranking metric from information retrieval.
- Ranks dimensions by reconstruction error and gives more credit when relevant dimensions appear higher in the ranking (Eq. 28).
- Unlike the three above, it is sensitive to the **order** of the ranking.



---

Section 7: Application areas of Deep anomaly detection in time Series

In this section many applications are mentioned but we are interested with industrial control systems.

it is mentioned that a study used lstm based AE to detect anomalies in multivariate systems where anomalies occur in production equipment components.




Important links and papers:

1. Github: https://github.com/zamanzadeh/DACAD

2. Domain Adaptation Contrastive Learning for Anomaly Detection in Multivariate Time Series: [link](https://ieeexplore.ieee.org/document/11003402)



Outlook:

1. paper proposes different architectures, we need to check which one fits our purpose or which one we can experiment with.
2. The section 3 consists of different models and architecture, if needed study this section again for detailed overview
3. Need to look into the models and hybrid models particularly.
4. it provides the **metrics**(including interpretability metrics) that should we be used while detecting the  anomalies and also provides the **datasets**.


- [ ] look into the github repo that they provided.


