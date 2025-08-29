# Survival Analysis of Veterans with Lung Cancer 

## Introduction 

Lung cancer is a leading cause of cancer-related death. Understanding factors that influence survival is crucial for guiding treatment. Survival analysis allows us to estimate survival probabilities and assess how patient characteristics affect outcomes. This analysis uses data from the Veterans' Administration Lung Cancer Study to examine what factors are most strongly associated with survival.

## Goal 

The primary goal of this analysis is to determine whether survival outcomes of lung cancer patients can be predicted based on patient characteristics. Specifically, we aim to identify which variables are significant predictors of survival, quantify their impact, and assess whether the assumptions underlying parametric models are reasonable when compared to non-parametric estimates.

## Data Collection 

The data used in this analysis comes from the Veterans' Administration Lung Cancer Study, which is included in the R survival package. This dataset originates from a randomized trial comparing two treatment regimens for lung cancer and is widely used for survival analysis examples. It contains information on 137 patients, including treatment type (trt), tumor cell type (celltype), survival time (time), censoring status (status), Karnofsky performance score (karno), time from diagnosis to randomization (diagtime), age (age), and prior therapy (prior).

Read more about the data [here](https://rdrr.io/cran/survival/man/veteran.html). 

## Parametric Analysis 

Parametric survival analysis involves fitting data to a specific, pre-defined probability distribution, such as Weibull, exponential, or log-normal. I'm choosing to use the Weibull distribution because of the flexibility it provides: it can model both increasing and decreasing hazard rates over time. First, I will fit separate models for each covariate to assess their individual association with survival time. This helps identify which variables are most relevant for inclusion in the multivariable parametric survival model. 

Based on the univariate Weibull models, the only covariates significantly associated with survival (p < 0.05) are cell type and Karnofsky performance score (karno). This suggests that these variables have a meaningful impact on survival time and should be considered in the multivariable parametric survival model.

![image](https://github.com/catherinealeal/VeteranSurvivalAnalysis/blob/main/images/param_model.png)

Positive coefficients indicate longer survival while negative coefficients indicate shorter survival.

- Coefficient for celltypesmallcell = 3.48 (p < 0.05): Veterans with small cell tumors have shorter survival compared to veterans with squamous cell tumors. 
- Coefficient for celltypeadeno = -0.71 (p < 0.05): Veterans with adeno cell tumors have much shorter survival compared to veterans with squamous cell tumors.  
- Coefficient for celltypelarge = -0.32 (p > 0.05): Survival of veterans with large cell tumors does not differ significantly from survival of veterans with squamous cell tumors.
- Coefficient for karno =  0.03 (p < 0.05): Higher Karnofsky performance score is associated with longer survival. 
- Intercept coefficient = 3.48: Baseline log survival for veterans with squamous cell tumors and a Karnofsky performance score of 0. 

The model was fit with a shape parameter K = 0.938. Since this value is not significantly different from 1 (p > 0.05), there is not enough evidence to conclude that the hazard is non-constant. A Weibull model with constant hazard is equivalent to an exponential model. 

Chi-squared of the likelihood ratio test = 63.15 (p < 0.05): The Weibull model with co-variates celltype and karno provides significantly better fit than a Weibull model with no co-variates. 

## Non-Parametric Analysis 

While parametric models assume a specific distribution for survival times, non-parametric models infer a survival distribution based on the data. The non-parametric model I'm going to use is the Kaplan-Meier estimator. By plotting Kaplan–Meier curves, I can visually compare survival patterns across groups (tumor cell types) and use log-rank tests to formally assess whether differences are statistically significant. This complements the parametric Weibull model by showing whether the distributional assumptions made there are reasonable.

![image](https://github.com/catherinealeal/VeteranSurvivalAnalysis/blob/main/images/KM.png)

This plot shows the overall survival probability for all veterans in the dataset. The stepwise curve represents the proportion of patients surviving beyond each time point. For example, the curve indicates that only about 10% of veterans are expected to survive one year after treatment.

The median survival time indicates the time at which the survival probability is 50%. In this case, the median survival time is 80 days, indicating that only half the veterans are expected to survive for 80 days after treatment. 

Now, I want to look at group effects, specifically for cell type, since this was identified in the parametric analysis to be significantly associated with survival. 

![image](https://github.com/catherinealeal/VeteranSurvivalAnalysis/blob/main/images/KM_CellType.png)

It is clear from this plot that survival of veterans depends on tumor cell type. Veterans with squamous and large cell tumors tend to have higher survival probabilities, while those with adeno and small cell tumors experience much shorter survival times.

Indeed, median survival times vary substantially across tumor cell types. Veterans with large cell tumors had the longest median survival at 156 days, followed by squamous cell tumors at 118 days. In contrast, veterans with adenocarcinoma and small cell tumors had much shorter median survival times of only 51 days. This highlights that cell type is a strong predictor of survival, with squamous and large cell types associated with better outcomes compared to adeno and small cell types.

The log-rank test shows that survival differs significantly by tumor cell type (p < 0.05), with squamous and large cell tumors showing better survival, and small and adeno cells associated with worse survival.

## Evaluating Parametric Assumption 

I can evaluate the exponential assumption by overlaying the fitted exponential survival curve on the Kaplan–Meier estimate. The Kaplan–Meier curve reflects observed survival, while the exponential model assumes a constant hazard. Close alignment between the two curves would support the exponential fit.

![image](https://github.com/catherinealeal/VeteranSurvivalAnalysis/blob/main/images/AssumptionEval.png)

The fitted exponential survival curve closely aligns with the Kaplan–Meier estimate, indicating that the assumption of a constant hazard in a exponential model is reasonable for this dataset.

## Conclusion

Overall, this analysis demonstrated that tumor cell type and Karnofsky performance score are the most significant predictors of survival among veterans with lung cancer. Patients with squamous and large cell tumors generally exhibit longer survival times, while those with adeno and small cell tumors experience shorter survival. This means that knowing the tumor type can help doctors give a more informed prognosis. Higher performance scores are also associated with improved survival. The Karnofsky performance score measures a patient’s overall functional ability. Veterans with higher scores, meaning they are more physically capable, are likely to survive longer. This highlights the importance of overall health and fitness in treatment planning.

View the full analysis with R code [here]([https://github.com/catherinealeal/VeteranSurvivalAnalysis](https://github.com/catherinealeal/VeteranSurvivalAnalysis/blob/main/SurvivalAnalysis.Rmd)). 
