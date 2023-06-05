---
title: Review of causal inference in user growth 
author: Jianmin
date: 2/8/2021
---

# 因果推断概述
# 核心技术工具箱
## 核心技术
- Instrumental Variables(IV)
- Regression Discontinuity Design(RDD)
- Fixed Effect Model(FEM)
- Synthetic Control(SC)
- Diff-in-Diff(DID)

## Potential Outcome framework
- Single observational experiments
   - Propensity score mathcing:  
      - [Causal Inference: What If, 2020](https://www.hsph.harvard.edu/miguel-hernan/causal-inference-book/)
   - Stratification/IPTW: 
      - [Causal Inference: What If, 2020](https://www.hsph.harvard.edu/miguel-hernan/causal-inference-book/)
      - [Stratification and weighting via the propensity score in estimation of causal treatment effects: A comparative study, 2004](https://www4.stat.ncsu.edu/~davidian/statinmed.pdf)
   - Doubly-robust estimation: 
      - [Double Robustness in Estimation of Causal Treatment Effects, 2007](https://www4.stat.ncsu.edu/~davidian/double.pdf) 推导简明清晰
- Multiple observations in time series
   - Diff-in-Diff: 
      - [How Much Should We Trust Differences-in-Differences Estimates? 2002](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=305064)
      - [Difference-in-Difference Estimation, 2021](https://www.publichealth.columbia.edu/research/population-health-methods/difference-difference-estimation)
   - Synthetic Control: 
      - [Synthetic Control Methods for Comparative Case Studies, 2010](https://economics.mit.edu/files/11859)
   - Fixed effect model: 
      - [On the Use of Linear Fixed Effects Regression Models for Causal Inference, 2013](https://imai.fas.harvard.edu/talk/files/JH12.pdf)
      - [Two-way fixed effects estimators with heterogeneous treatment effects, 2018](https://arxiv.org/abs/1803.08807)
      - [Double/Debiased Machine Learning for Treatment and Structural Parameters, 2016](https://economics.mit.edu/files/12538)
      - [DoubleML](https://docs.doubleml.org/stable/index.html)
   - Structural Time Series model:
      - [Structural Time Series(GAM), 2020](https://structural-time-series.fastforwardlabs.com/)
      - [Causal Impact](https://google.github.io/CausalImpact/CausalImpact.html)
      - [Inferring causal impact using Bayesian structural time-series models, 2015](https://research.google/pubs/pub41854/)
      - [State Space Model and Kalman Filter for Time-Series Prediction, 2019](https://towardsdatascience.com/state-space-model-and-kalman-filter-for-time-series-prediction-basic-structural-dynamic-linear-2421d7b49fa6)
- Multiple cause inference
   - [The Blessings of Multiple Causes, 2019](https://arxiv.org/abs/1805.06826)
   - [On Multi-Cause Causal Inference with Unobserved Confounding: Counterexamples, Impossibility, and Alternatives, 2019](https://arxiv.org/abs/1902.10286)
- Instrumental Variables
   - [Mostly Harmless Econometrics - Chapter 4](https://www.mostlyharmlesseconometrics.com/)
   - [Deep IV: A Flexible Approach for Counterfactual Prediction](http://proceedings.mlr.press/v70/hartford17a.html)
- Uplift model
   - Review:
      - [Causal Inference and Uplift Modeling : A review of the literature, 2016](https://proceedings.mlr.press/v67/gutierrez17a/gutierrez17a.pdf)
   - Meta Learners: 
      - [Uber's causalml package, 2019](https://github.com/uber/causalml)
      - [Meta-learners for Estimating Heterogeneous Treatment E ects using Machine Learning(x-learner), 2019](https://arxiv.org/pdf/1706.03461.pdf)
   - Transform outcome: 
      - [Outcome Transformations for Uplift Modeling and Treatment Effect Estimation, 2020](https://johaupt.github.io/causal%20inference/uplift%20modeling/outcome%20transformation/Lai_Kane_Transformed_outcome.html)
   - Causal tree: 
      - [Recursive Partitioning for Heterogeneous Causal Effects, 2015](https://arxiv.org/abs/1504.01132)
      - [Machine Learning Methods for Estimating Heterogeneous Causal Effects, 2015](https://arxiv.org/pdf/1810.13237)
- Advanced A/B test
   - CUPED: [Improving the Sensitivity of Online Controlled Experiments by Utilizing Pre-Experiment Data](https://exp-platform.com/Documents/2013-02-CUPED-ImprovingSensitivityOfControlledExperiments.pdf)
   - ATE/ITE/CATE: 
   - LATE: 
   - HTE: 
- Theory
   - [Fundamental Theorems for Econometrics, 2020](https://bookdown.org/ts_robinson1994/10_fundamental_theorems_for_econometrics/)
## Causal Graph
- Introduction
   - [Causal inference in statistics - An overview, 2009](https://ftp.cs.ucla.edu/pub/stat_ser/r350.pdf)
- Advanced
   - [The Causal-Neural Connection: Expressiveness, Learnability, and Inference, 2021](https://arxiv.org/abs/2107.00793)
   - [Relating Graph Neural Networks to Structural Causal Models, 2021](https://arxiv.org/abs/2109.04173)
## Poineers
- Econometrics
   - [Victor Chernozhukov, MIT](http://www.mit.edu/~vchern/)
   - [Susan Athey, Stanford](https://athey.people.stanford.edu/)
   - [Guido W. Imbens, Stanford](https://www.gsb.stanford.edu/faculty-research/faculty/guido-w-imbens)
   - [Alberto Abadie, MIT](https://economics.mit.edu/faculty/abadie)

# 工业界应用
- User Growth
   - [Preventing churn like a bandit, Gerben Oostra, BigData Republic, 2020](https://medium.com/bigdatarepublic/preventing-churn-like-a-bandit-49b7c51b4929)
- Product engagements
   - [Using Causal Inference to Improve the Uber User Experience, Uber Data, 2019](https://eng.uber.com/causal-inference-at-uber/)
   - [Mediation Modeling at Uber: Understanding Why Product Changes Work (and Don’t Work)](https://eng.uber.com/mediation-modeling/)
   - [Causal inference from observational data: Estimating the effect of contributions on visitation frequency at LinkedIn, 2019](https://arxiv.org/abs/1903.07755)
   - [Machine Learning for Decision Making, Coursera, 2018](https://medium.com/teconomics-blog/machine-learning-for-decision-making-e776f9f8917e)
   - [How to Use Machine Learning to Accelerate AB Testing, 2018](https://medium.com/teconomics-blog/using-ml-to-resolve-experiments-faster-bd8053ff602e)
   - [Machine Learning Meets Instrumental Variables, Coursera, 2018](https://medium.com/teconomics-blog/machine-learning-meets-instrumental-variables-c8eecf5cec95)

- Others
   - [Computational Causal Inference at Netflix, Netflix Tech Blog, 2020](https://netflixtechblog.com/computational-causal-inference-at-netflix-293591691c62)
   - [Causal inference: Understanding the fundamentals, Data Science at Microsoft, 2020](https://medium.com/data-science-at-microsoft/causal-inference-part-1-of-3-understanding-the-fundamentals-816f4723e54a)
