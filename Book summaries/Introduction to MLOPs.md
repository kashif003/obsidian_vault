
---


# 0.Buzz words

Deployment and infrastructure
+ Kubernetes
+ linux cgroups
+ flask
model portability formates
+ PMML
+ PFA
+ ONNX
+ POJO
Testing strategies
+ <span style="color: var(--color-yellow)">shadow testing:</span> testing the app without the user knowing about it , when user sends a request the old app processes it and the copy of the request is sent to new version as well just to test it. 
+ <span style="color: var(--color-yellow)">A/B testing: </span>certain users get the new feature and others dont, based on the  users response on new feature decision of integration is made

Others
+ <span style="color: var(--color-yellow)">CI/CD:</span> continuous integration and continuous delivery
+ <span style="color: var(--color-yellow)">Jenkins:</span> open source automation server primarily used by software development teams to implement CI part of  CI/CD. tests the new code performs the tests
+ spinnaker: this is open sourse multi cloud deliver system which does the CD in CI/CD 
+ <span style="color: var(--color-yellow)">Apache Spark:</span> open source system designed to process and analyze the data

# 1. Why Now and challenges



<span style="color: var(--color-red)">Q. why its hard to scale ML?</span>

 + Many dependencies
 + communication gaps.
 + Data scientists are not software engineers.



# 2. People of MLops


<span style="color: var(--color-yellow)">Data scientists:
</span>
+ building models.
+ initial problem framing and ongoing monitoring.


<span style="color: var(--color-yellow)">Data Engineers:</span>

+ source the right data
+ fix the pipeline issues affecting model in produciton.

<span style="color: var(--color-yellow)">Software Engineers:
</span>
+ Integrate models into applications.
+ perorm CI/CD.


<span style="color: var(--color-yellow)">Devops:</span>
+ build and test for security, performance, availability
+ manage CI/CD pipeline.


<span style="color: var(--color-green)">NOTE:</span> Their are other roles as well but we are not discussing about those role currently.


# 3. Key MLOPs Features


<span style="color: var(--color-blue)">1. Development</span>
	1. Establishing business objectives
	2. Data sources and EDA (Exploratory data analysis)
	3. feature engineering and selection
	4. Training and evaluation
	5. Reproducibility
	6. Responsible AI
<span style="color: var(--color-blue)">2. Deployment</span>
	+ Two main deployments
		1. model as a server
			1. REST API, REAL TIME
		2. embedded model
			1. packaged into Applications
	+ containerization
		+ Docker/ kubernetes
		+ blue-green deployment
		+ elastic scaling
<span style="color: var(--color-blue)">3. Monitoring</span>
	+ Devops
		+ traditional performance  monitoring
	+ Data scientists
		+ monitor model degradation over time
	+ Business
<span style="color: var(--color-blue)">4. Iteration</span>
	+ model retraining over time
	+ etc
<span style="color: var(--color-blue)">5. Governance</span>

# 4. Developing ML models


This chapter deals with what kind of models  we can develop and what kind of data do we need to develop these  models


# 5. preparing for production


this chapter deals with variety of things which are detailed below:

Runtime environments:
+ production environments: JVM, kubernetsm, TF etc
+ Adaptation spectrun
+ Tooling considerations: sklearn to PMML or ONNX
+ performance consideration
+ Data access

Model Risk Evaluation:

Quality Assurance for ML:
+ QA
+ key testing
+ sub-population
+ model stability

Reproducibility and Auditability
+ Model should be reproduced again.
+ all the history of changes that model went through should be accessible.

Machine Learning Security:
+ Adversarial attacks
+ Poisoning attacks
+ other vulnerabilities 


# 6. Deploying to produciton


CI/CD pipelines
+ Recommendation: start with simple models and built by adding more things to it

Building ML Artifacts:

Deployment Strategies:
+ integration
+ delivery
+ deployment
+ release

# 7. Monitoring and feedback loop


Two levels of monitoring:
1. Resource Level 
2. Performance Level

How often models should be retrained?

+ depends on
	1. Domain
	2. Cost
	3. Ground truth lag
	4. online learning

Understanding Model Degradation
1. Ground truth evaluation:
2. Input drift detection
	1. Feature drift
	2. Concept drift

detection technique:
+ Kolmogorov simirnov for continuous features
+ Chi squared for categorical features


The Feedback Loop:
1. Logging
2. Model evaluation / model store
3. online evaluation;
	1. champion challenger
	2. A/B testing




# 8. Model Governance


This chapter deals with the legal rules and regulation which and why we need to publish them 



# 9. MLOps in Practice — Consumer Credit Risk Management


**Tools/technologies mentioned:** FICO score (proprietary scoring system, not really a "tool" to adopt), XGBoost, Shapley values, PMML, JAR files, Docker, and statistical metrics (Kolmogorov-Smirnov, Wasserstein distance, population/characteristic stability index, AUC, Gini).


# 10: MLOps in Practice — Marketing Recommendation Engines


# 11: MLOps in Practice — Consumption Forecast (Power Grid)

