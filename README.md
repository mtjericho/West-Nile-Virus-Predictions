# West-Nile-Virus-Predictions
Predict the probability of West Nile Virus (WNV) occurring  
[based on this competition](https://www.kaggle.com/c/predict-west-nile-virus).
---

## Problem Statement

The West Nile Virus ('WNV') is a disease most commonly spread by certain species of mosquitoes. Cases of West Nile Virus occur during mosquito season, which starts in the summer and continues through fall. 

Most people infected with the virus will not get sick. Around 20% may experience flulike symptoms. However, the virus can cause serious illness. According to the US Centers for Disease Control ('CDC'), less than 1% of people bitten by an infected mosquito will become severely ill. 

The number of WNV cases in an area could rise in any given year. This risk is why the Chicago Department for Public Health ('CDPH') established a surveillance and control program in 2004, which includes annual spraying.  

As data scientists, with the provided datasets, we would like to build a predictive model that can determine whether or not a particular area will have WNV-carrying mosquitoes. With such a model, CDPH can target mosquito-spraying efforts towards areas that pose the greatest risk, mitigating the potential of another outbreak. We also evaluate effectiveness of the current annual spray program.

---

## Predictive Models


| Classifier          | Hyperparams                                                                                                                                   |   Train ROC AUC |   Val ROC AUC |   Test ROC AUC |
|:--------------------|:----------------------------------------------------------------------------------------------------------------------------------------------|----------------:|--------------:|---------------:|
| Logistic Regression | {'logreg__C': 1, 'sampling__k_neighbors': 5, 'sampling__sampling_strategy': 'auto'}                                                           |            0.78 |          0.74 |           0.7  |
| XG Boost            | {'sampling__k_neighbors': 5, 'sampling__sampling_strategy': 'minority', 'xgb__max_depth': 2, 'xgb__scale_pos_weight': 10}                     |            0.81 |          0.8  |           0.76 |
| SVM                 | {'sampling__k_neighbors': 3, 'sampling__sampling_strategy': 'auto', 'svc__C': 0.1, 'svc__degree': 5, 'svc__kernel': 'rbf'}                    |            0.8  |          0.78 |           0.71 |
| Random Forest       | {'rf__max_depth': 4, 'rf__max_features': 'auto', 'rf__n_estimators': 1000, 'sampling__k_neighbors': 2, 'sampling__sampling_strategy': 'auto'} |            0.79 |          0.75 |           0.71 |

The train and val scores are obtained from performance on the split between training and validation data from the train.csv data. The test scores are from predictions on the test.csv data after retraining the models with best params on the full train dataset, with the ROC AUC score returned by Kaggle.

Comparing the performance on the test data versus the performance on the training data, we do observe some overfitting, especially from the support vector machine and random forest models. The support vector model was also by far the slowest to train and to do predictions, likely due to the need to project the data into complicated dimensions to do fitting.
The logistic regression model did the worse of the four models on both the train and test data, likely due to its inability to deal with collinearity of predictors as well as the other models.
Overall, XGBoost performed the best, with an ROC AUC score for the test data of 0.76 (rounded to 2 decimals). XGBoost tends to do well on data with many samples and relatively fewer features, which is one explanation for its good performance.


---

## Cost and Effectiveness

**Cost**                                                                                                                                                                                             
The CDPH conducts mosquito spraying as a measure to kill adult mosquitos and reduce the occurrence of the West Nile Virus.
The spraying begins at dusk and continues through the night. The material being used to control the adult mosquitoes, Zenivex™, is applied at a rate of 1.5 fluid ounces (0.01171875 gallons) per acre. The spray will be applied by licensed mosquito abatement technicians. The cost of Zenivex E4 is about 80 USD dollars per gallon. Given that the total area of Chicago is 606.1 km2 (149 770.572 acres), it would cost $140,409 to cover the entire area. (info from [this source](https://www.chicago.gov/city/en/depts/cdph/provdrs/healthy_communities/news/2020/august/city-to-spray-insecticide-thursday-to-kill-mosquitoes.html)).


There are also biodiversity issues especially when dealing with natural protected areas. We have to prevent at least 19 non-serious case of Wnv to justify the cost. ($7,500 hospitalisation cost). 

**Effect**                                                                                              
          
Based on the exploratory data analysis conducted earlier, there was a lack of evidence to support the claim that mosquito spraying had any effect on reducing WnvPresent. Many of the traps’ locations did not match the spray locations. This made it difficult to verify the effectiveness after spraying. Furthermore, there are only year 2011 and 2013 spraying data with only 10 records of spraying. Out of these 10 sprays, 6 times sprays were conducted in Aug 2013 and early Sept 2013. However, it did not reveal any noticeable decrease in WNV occurrences in Sept 2013.

---

## Conclusion and Recommendations

Based on the high financial cost of spraying, detriment to the environment, and poor correlation of spraying to the presence of the virus, we recommend CDPH to cut down on spraying as the cost does not justify the benefits derived from it. This is evident from the much higher West Nile Virus presence seen in the year 2013 when spraying was conducted. CDPH should instead spend more effort in educating citizens in predicted areas to reduce mosquito breeding habitats by clearing areas with stale water and cleaning rain gutters, and to wear clothing that provides protection from mosquitos.

---
