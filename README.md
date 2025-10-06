# Capstone-Final: Prediction of likelihood of recurrence in bladder cancer patients

_Link to corresponding Jupyter Notebook: 
https://github.com/pamelakhalaf/Capstone-Final/blob/main/Code/Capstone_BladderCancerRecurrence_Final.ipynb_

## Problem Statment 
Bladder cancer remains one of the most challenging cancers to treat with potential for disease recurrence. Upfront risk stratification becomes critical to help tailor treatment plan for patients and appropriate disease monitoring. The goal of this machine learning project is to identify patient and disease charateristics that are most predictive of bladder cancer reccurrence. 

## Model Outcomes 
Supervised classification based machine learning models will be built to predict binary outcome of disease recurrence or no recurrence. Best models will be analyzed to identify features of highest importance and their impact on disease outcome.  

## Data Acquisition 
A Kaggle published dataset of bladder cancer patients is used as basis for predictive modeling. The dataset is based on a publication of Wei et al (1989) and includes patient data and disease events as progression is tracked longitudinally. 

## Data Preprocessing/Preparation
The data is structured at the event level with every patient experiencing one or more events. As such we restructure the data to be at the patient level and to register the total number of recurrent events they have experienced. In order to further prepare the data for modeling, we performed the following data exploration/cleaning steps: 
1. Renamed features for added clarity
2. Created a new feature representing duration on therapy based on the start, stop time points
3. Converted object based features to numerical ones e.g. treatment choice was converted from placebo, thiotepa and pyridoxine to 0,1,2 indicating increasing scale of treatment intensity
4. Converted the number of recurrence feature to a binary class of 0 when no recurrence has taken place and 1 when 1+ recurrences have taken place
5. Eliminated the number of tumors and size of tumors feature at reccurence due to extensive % of missing values

No duplicates were identified and the final dataset was reduced to 118 patients. The limited size of the dataset while a challenge,  will be addressed in the modeling stage. 

## Modeling 
Given the limited size of the dataset and the risk of overfitting, we implement K-fold cross-validation in the modeling step to mitigate for it. The target variable was set as the binary feature of recurrence or no recurrence and is well balanced across the dataset. First, multiple classification models are defined and include: regularized logistic regression (lasso and ridge), SVM, KNN and decision trees. Hyperparameter tuning is performed for each of these models with GridSearchCV and using stratified k-fold. Data was also scaled for all models with the exception of decision trees which don't require it. 
Since the business problem in hand is a cancer recurrence problem, it is important that an evaluation metric is chosen such that false negatives are minimized to avoid misclassification of patients that could recurr as low risk. Hence, we anchor mostly to recall here and optimize for it in the hyperparameter tuning while continuing to assess training and testing accuracy, and precision. 

## Model Evaluation 
The training and testing accuracy are compared to ensure models are not overfitting. While recall is the evaluation metric of highest importance, we also consider precision and testing accuracy. Despite a perfect recall score, the SVM model didn't perform as well on accuracy and precision. L1 Logistic regression had a fairly high recall and reasonable accuracy and precision score with L2 Logistic regression performing slightly lower across metrics. 
KNN performed lower on recall and decision tree had good recall but lower precision and accuracy scores. Overall, the L1 Lasso logistic regression model perfomed best with a recall of 85%, and precision and accuracy of 79% respectively. The model is further assessed to evaluate the coefficients and understand features that are most predictive of recurrence. 

## Findings
The top patient and disease characteristics that are most predictive of recurrence are the interval duration on therapy, the patient status and the number of initial tumors upon diagnosis. The shorter the interval duration & the larger the number of initial tumors, the more likely cancer will recurr. Patient status is not as informative here given we don't know all conditions of patient status that were bucketed under "censored" but is an important feature to keep in mind if additional data could be gathered. These features that correlate with bladder cancer recurrence should be considered upfront when laying out patient's treatment plan and tailoring their care. We further assess how well our model perform for different patient subgroup split by treatment regimen and see better prediction accuracy for patients on placebo and pyridoxine. This is important to keep in mind espeially when performing risk stratification on patients receiving thiotepa as alternative treatment. 
