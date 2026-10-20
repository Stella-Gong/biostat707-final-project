# How Early Can We Predict In-Hospital Mortality?

**Comparing 6-, 12-, 24-, and 48-Hour ICU Observation Windows**

## Team Members and Roles

[Insert each member's name and specific responsibilities. Assign data preparation and exploratory analysis, feature engineering, modeling and validation, and evaluation and writing across the team.]

## 1. Clinical Question

Earlier prediction of in-hospital mortality could help clinicians identify high-risk ICU patients sooner. However, shorter observation windows provide fewer measurements and potentially less reliable estimates of mortality risk. This project will examine the trade-off between prediction timing and predictive performance.

Our primary question is: **How much predictive performance is lost when using the first 6, 12, or 24 hours of ICU data instead of the first 48 hours?** We will assess both discrimination and calibration and examine whether the relationship between observation time and performance differs between regularized logistic regression and a tree-based model.

This project extends the checkpoint analysis by making prediction timing the central comparison. The 48-hour analysis will serve as a reference within a consistent evaluation framework.

## 2. Dataset, Cohort, and Outcome

We will use the [PhysioNet/CinC Challenge 2012 dataset](https://physionet.org/content/challenge-2012/1.0.0/), released under the Open Data Commons Attribution License v1.0 (ODC-BY). The primary analysis will use set-a, containing 4,000 ICU stay records. We will not train or tune models on set-b or set-c; any use of these sets will follow course instructions.

The cohort consists of patients with ICU stays of at least 48 hours. The outcome is **in-hospital mortality**, obtained from the accompanying outcome file. The same eligible records will be used across all four observation windows. Patients will not be excluded simply because early measurements are missing.

Before proposal submission, we will confirm that the data can be downloaded and loaded and that timestamps, mortality labels, key predictors, and benchmark scores are available. Because the dataset includes stays of at least 48 hours, our conclusions will not directly generalize to all newly admitted ICU patients.

## 3. Analysis and Validation Design

We will first summarize patient characteristics, mortality prevalence, and missingness. To understand how information availability changes over time, we will compare missing-data proportions and measurement counts across the four windows.

For each window, we will restrict the original records to measurements available by the prediction time before constructing features. We will apply the same feature definitions across windows, including basic patient characteristics available at prediction and summaries of clinical measurements: mean, minimum, maximum, last observed value, and measurement count. Record identifiers will not be predictors.

We will compare **Elastic Net logistic regression** and **gradient-boosted trees**, connecting the project to the course material on regularization and tree ensembles. Continuous predictors will receive median imputation with missingness indicators. Logistic regression predictors will also be standardized. All learned preprocessing steps will be fitted exclusively within the relevant training folds.

We will use stratified nested cross-validation on set-a, with five outer folds for evaluation and three inner folds for hyperparameter selection. The same record assignments will be used across observation windows and model classes. Each window will be tuned separately using the same search procedure. This design applies the course principles of honest model selection and leakage prevention.

The main evaluation measures will be **AUROC, calibration plots, and Brier score**, with precision-recall performance as a supplementary assessment. We will plot performance against observation time and report differences between each early window and the 48-hour reference.

SAPS-I and SOFA will serve as clinical benchmarks. Following the project description, these will be treated as 24-hour-information benchmarks, with their information advantage over earlier models explicitly acknowledged. For probability-based evaluation, score-to-risk mappings will be fitted using training folds only.

## 4. Expected Findings and Interpretation

We hypothesize that longer observation windows will generally improve predictive performance, but with diminishing gains. In particular, 24-hour predictions may approach the performance of 48-hour predictions. These are hypotheses rather than assumed results.

We will interpret performance differences alongside changes in missingness and measurement availability. Selected error examples will help illustrate when additional observations improve predictions. We will also distinguish predictive associations from causal effects, since measurement frequency and treatment processes can influence the available information.

The project will quantify the predictive cost of earlier assessment. It will not establish an optimal clinical prediction time without specifying an intervention and the consequences of waiting. Other limitations include the restricted cohort and the absence of external validation.

## AI Assistance Disclosure

OpenAI ChatGPT/Codex was used to help organize and draft this proposal and create the project repository. Its benefit was helping structure the analysis plan; a limitation is that its suggestions require independent verification against course requirements and the dataset. No empirical results were generated for this proposal.

## Reference

Silva I, Moody G, Scott DJ, Celi LA, Mark RG. Predicting in-hospital mortality of ICU patients: The PhysioNet/Computing in Cardiology Challenge 2012. *Computing in Cardiology*. 2012;39:245-248. [Article](https://pmc.ncbi.nlm.nih.gov/articles/PMC3965265/).
