# Report: Predict Bike Sharing Demand with AutoGluon Solution
#### Mark Melling

## Initial Training
### What did you realize when you tried to submit your predictions? What changes were needed to the output of the predictor to submit your results?
Kaggle only accepts predictions >= 0, so it was necessary to make sure that all negative preditions were set to 0.

### What was the top ranked model that performed?
The top ranked model, with a score of -118.456660, was RandomForestMSE_BAG_L1.

## Exploratory data analysis and feature creation
### What did the exploratory analysis find and how did you add additional features?
My first step was to just consider what factors are likely to have an impact on bike usage. 
The most obvious being the time of day, day of week, and possibly the month. 
Subsequently I conducted some analysis using shapely values (https://towardsdatascience.com/tagged/shapley-values?p=8c4a2bc11c2a), this can be seen in the appendix of the bike sharing notebook.

The analysis supported the intuition that hour is an important feature, in fact probably the most important feature.

![shap_summary.png](https://raw.githubusercontent.com/markmelling/bike_sharing/main/img/shap_summary.png)


### How much better did your model preform after adding additional features and why do you think that is?
The model performed significantly better. The best model was a WeightedEnsemble_L3 model with a score of -32.3

#### The leaderboard
 |pos|model|score_val|pred_time_val|fit_time|
 |---|---|---|---|---| 
 |0|      WeightedEnsemble_L3|  -32.323917|       6.760135|  512.133244|   
 |1|      WeightedEnsemble_L2|  -32.410403|       4.290395|  170.613109|   
 |2|     ExtraTreesMSE_BAG_L2|  -32.723657|       5.997226|  344.214564|   
 |3|          CatBoost_BAG_L2|  -32.766262|       5.561719|  379.297584|   
 |4|   NeuralNetFastAI_BAG_L2|  -32.924143|       5.977263|  464.524949|   
 |5|          LightGBM_BAG_L2|  -32.949774|       5.630867|  348.544685|   
 |6|   RandomForestMSE_BAG_L2|  -33.347990|       5.990088|  348.845684|   
 |7|        LightGBMXT_BAG_L2|  -33.429228|       5.696853|  349.956519|   
 |8|          LightGBM_BAG_L1|  -33.835882|       0.889617|   34.317848|   
 |9|        LightGBMXT_BAG_L1|  -34.417811|       2.732017|   94.352649|   
 |10|          XGBoost_BAG_L1|  -34.812459|       0.212669|   38.598792|   
 |11|    ExtraTreesMSE_BAG_L1|  -38.510896|       0.462628|    1.565891|   
 |12|  RandomForestMSE_BAG_L1|  -38.833999|       0.455389|    2.785168|   
 |13|  NeuralNetFastAI_BAG_L1|  -44.033253|       0.424783|  166.175911|   
 |14|    LightGBMLarge_BAG_L1|  -86.971789|       0.156467|    3.789872|   
 |15|   KNeighborsDist_BAG_L1| -120.276825|       0.065916|    0.020597|   
 |16|   KNeighborsUnif_BAG_L1| -122.071270|       0.069732|    0.025976|   

## Hyper parameter tuning
### How much better did your model preform after trying different hyper parameters?
I experimented with using different hyperparemeters for autogluon itself and specific models, but largely was not able to improve on autogluons choice of parameters.
Where I did discover some variation in performance was different choice of both eval_metric and training time. See table below.

### If you were given more time with this dataset, where do you think you would spend more time?
My feeling is that outside of eval_metric and training time, it is probably most effective to let autogluon choose the best hyperparamters. 

Spending more time on featuring engineering looks like the best place to spend more time. For instance the SHAP analysis suggests that features such as 'holiday' and 'season' could be dropped.

### A table with the models run, the hyperparameters modified, and the kaggle score.

|model|eval_metric|presets|training_time||score|
|---|---|---|---|---|
|initial|root_mean_squared_error|best_quality|600|1.37974|
|add_features|root_mean_squared_error|best_quality|600|0.45910|
|hpo1|mean_absolute_error|best_quality|600|0.46934|
|hpo2|root_mean_squared_error|best_quality|600|0.51804|
|hpo3|r2|best_quality|600|0.51596|
|hpo4|mean_absolute_error|best_quality|3600|0.45688|


### A line plot showing the top model score for the three (or more) training runs during the project.

![model_train_score.png](https://raw.githubusercontent.com/markmelling/bike_sharing/main/img/model_train_score.png)

### A line plot showing the top kaggle score for the three (or more) prediction submissions during the project.

![model_test_score.png](https://raw.githubusercontent.com/markmelling/bike_sharing/main/img/model_test_score.png)

## Summary
Spending time to understand the data and determine which new features can be created and possibly those that could be dropped is an effective way of significantly improving the models.

Largely speaking Autogluon does a pretty good job of model and hyperparmeter selection.

