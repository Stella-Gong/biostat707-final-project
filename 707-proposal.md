# How Early Can We Predict In-Hospital Mortality?

**Comparing 6-, 12-, 24-, and 48-Hour ICU Observation Windows**

## Team Members

Siyu Gong

## 1. Clinical Question

Early prediction of in-hospital mortality can help clinicians identify high-risk ICU patients earlier. However, a shorter observation window provides less information, so the prediction may be less accurate. A longer observation window provides more information, but clinicians need to wait longer before getting the prediction. This project will study the trade-off between prediction time and model performance.

Our main question is: **How much predictive performance is lost when we use the first 6, 12, or 24 hours of ICU data instead of the first 48 hours?** We will evaluate both discrimination and calibration. We will also compare regularized logistic regression and tree-based models to see whether observation time affects their performance differently.

This project builds on the checkpoint analysis and focuses on comparing different prediction times. We will use the same evaluation framework for all four time windows and use the 48-hour results as the reference.


## 2. Dataset, Cohort, and Outcome

I will use the [PhysioNet/CinC Challenge 2012 dataset](https://physionet.org/content/challenge-2012/1.0.0/), released under the Open Data Commons Attribution License v1.0 (ODC-BY). The primary analysis will use set-a, containing 4,000 ICU stay records. We will not train or tune models on set-b or set-c.

The cohort consists of patients with ICU stays of at least 48 hours. The outcome is **in-hospital mortality**, obtained from the accompanying outcome file. The same eligible records will be used across all four observation windows. Patients will not be excluded simply because early measurements are missing.

## 3. Analysis and Validation Design

I will first summarize patient characteristics, mortality rate, and missing data. I will compare the proportion of missing data and the number of measurements across the four time windows to see how much information is available at each prediction time.

For each window, I will only use measurements collected up to the prediction time to create features. I will use the same feature definitions for all four windows. These features may include basic patient characteristics available at that time and the mean, minimum, maximum, last observed value, and number of measurements for clinical variables.

I will compare **Elastic Net logistic regression** and **gradient-boosted trees**. I will fill missing values in continuous predictors with the median and add indicators to show whether values are missing. I will also standardize predictors for logistic regression. All preprocessing steps that learn from data will use only the training folds.

I will use stratified nested cross-validation on set-a, with five outer folds to evaluate performance and three inner folds to select hyperparameters. I will use the same fold assignments for all time windows and model types. I will tune each time window separately but follow the same search procedure. This will help me compare the models fairly and avoid data leakage.

I will mainly evaluate performance using **AUROC, calibration plots, and Brier score**. I will also examine precision-recall performance as an additional measure. I will show how performance changes with observation time and compare each earlier window with the 48-hour window.

I will use SAPS-I and SOFA as clinical benchmarks. Following the project description, I will treat these scores as using information from the first 24 hours.

## 4. Expected Findings and Interpretation

I hypothesize that longer observation windows will generally improve predictive performance, but with diminishing gains. In particular, 24-hour predictions may approach the performance of 48-hour predictions. These are hypotheses rather than assumed results.

We will interpret performance differences alongside changes in missingness and measurement availability. Selected error examples will help illustrate when additional observations improve predictions. We will also distinguish predictive associations from causal effects, since measurement frequency and treatment processes can influence the available information. Also quantify the predictive cost of earlier assessment.

## AI Assistance Disclosure

Codex was used to help organize this proposal's important points and create the project repository. 

## Reference

Silva I, Moody G, Scott DJ, Celi LA, Mark RG. Predicting in-hospital mortality of ICU patients: The PhysioNet/Computing in Cardiology Challenge 2012. *Computing in Cardiology*. 2012;39:245-248. [Article](https://pmc.ncbi.nlm.nih.gov/articles/PMC3965265/).
