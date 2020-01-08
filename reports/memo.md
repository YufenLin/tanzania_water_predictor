# Business Problem
In Tanzania, 50% of the population does not have access to clean drinking water. Ground is generally cleaner than surface water which is often polluted by sewers and toxic waste from factories. Sometimes, people have to walk miles to the nearest water pump. With limited resources it is imperative to identify where best to allocate resources for pump repair. We will be successful if our end model is better than the dummy model

# Our Approach

## Clean Data
1. The features we used:
       * amount_tsh - Total static head (amount water available to waterpoint)
       * gps_height - Altitude of the well
       * installer - Organization that installed the well. Set NaN as ‘others’
       * wpt_name -  Name of the waterpoint if there is one
       * num_private
       * basin - Geographic water basin
       * region - Geographic location
       * population - Population around the well
       * scheme_management - Who operates the waterpoint. Set NaN as ‘others’
       * permit - If the waterpoint is permitted. Set NaN as ‘others’.
       * extraction_type - The kind of extraction the waterpoint uses
       * management_group - How the waterpoint is managed
       * payment - What the water costs
       * quality_group - The quality of the water
       * source_class - The source of the water
       * waterpoint_type_group - The kind of waterpoint
       * date_recorded - The date the row was entered
   2. There are 59400 records in train data and 14850 records in test data.
   3. Set gps_height = 0 where gps_height < 0
   4. Consider performance and avoid overfitting. If the unique value of one feature is more than 50 and the number of one unique value is less than 500(< 1% records), we set the value as ‘others’
   5. There are a lot of ‘0’ values in construction_year. It is an unreasonable value for year of construction, so it casts ‘0’ as ‘unknown’. Set other years into decade bins. Set construction_year as categorical features.
   6. Parses categorical variables by OneHotEncoder

## Build Models Using Training Data

Training Data Score
| Model |Mean Cross Validation Score | Recall | F1 Score | Precision | Accuracy | Hamming loss | Hyperparameter |
|------|------|------|------|------|------|------|------|
| Decision Tree |72.4% |78.6%|77.8%|78.0%|78.7%|21.3%|criterion='gini', max_depth= 139, min_samples_leaf= 9, min_samples_split= 9|
| Bagging Classifier |75.6%|85.5%|84.8%|86.0%|85.5%|14.5%|max_features=0.6, max_samples= 0.4, n_estimators= 200|
| Random Forest|75.6%|81.2%|79.7%|81.5%|81.2%|18.8%|criterion='entropy', max_depth= 40, min_samples_split= 5, min_samples_leaf = 2, n_estimators = 600|
| XGBoost |74.4%|91.0%|91.0%|91.0%|91.0%|8.9%|learning_rate = 0.01, max_depth = 30, n_estimators= 600|



## Best Model
As is shown with the above model metrics, our best model was the Bagging Classifier which achieved a mean cross validation score of 75.6%. Although this is similar to the Random Forest’s performance, the Bagging Classifier maintained higher scores in Recall, F1, Precision, and Accuracy. It also maintained a lower Hamming loss score. The Hyper parameters that gave us the best results were found through interactive grid searching. We feel confident in our model choice, and it is likely that we would only be able to increase performance through different cleaning and feature engineering techniques. We would have experimented more with various cleaning and feature engineering with more time. 

## Predict on Test Data

We used our bagging model to on the testing data. Our results were 64% functional pumps, 34 % non-functional, and 2% functional needs repair. From our decision tree feature importance, we found that elevation and population to be the main predictor of functionality.

## Conclusion

We found that the most broken wells are clustered near cities. We found this by overlaying a population heat map with the locations of the pumps

## Future Steps

Test categorical dependencies between each output. Drop columns that do not apply to the mechanisms of a pump to see which pump feature leads most to functionality
