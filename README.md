# Tanzania water pumps predictor
Use [Tanzanian Water Well Data](https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/page/23/) to create a classification model predicting the function of water pumps throughout Tanzania

## Findings
- Most broken pumps in big cities  located on the coast with high population density.
- Elevation and population are the main predictors of model.
- There are 14850 records in the test data, we predict 64% functional, 34% non functional, 2% functional needs repair.


## The Process

### Data Cleaning
1. There are 59400 records in the train data and 14850 records in the test data.
2. Predict the condition of water wells. There are three status: functional, non functional, and functional needs repair.
3. The features we used:
    - amount_tsh - Total static head (amount water available to waterpoint)
    - gps_height - Altitude of the well
    - installer - Organization that installed the well. Set NaN as 'others'
    - wpt_name -  Name of the waterpoint if there is one
    - num_private
    - basin - Geographic water basin
    - region - Geographic location
    - population - Population around the well
    - scheme_management - Who operates the waterpoint. Set NaN as 'others'
    - permit - If the waterpoint is permitted. Set NaN as 'others'.
    - extraction_type - The kind of extraction the waterpoint uses
    - management_group - How the waterpoint is managed
    - payment - What the water costs
    - quality_group - The quality of the water
    - source_class - The source of the water
    - waterpoint_type_group - The kind of waterpoint
    - date_recorded - The date the row was entered
4. Set ‘NaN’ to ‘others’
5. Adjust unreasonable values: gps_height, construction_year
6. Consider performance and avoid overfitting. Only use top 50 values in each features, we set others as 'others'
7. Parses categorical variables by one-hot encoding


### Modeling
1. Dummy Classifier
2. Decision Tree Classifier
3. Bagging Classifier
4. Random Forest Classifier
5. XGB Classifier
6. GridSearchCV: search better parameter values for each model

### Evaluation
1. Get recall score, f1 score, precision score, accuracy, and hamming loss. 
2. Calculate mean cross validation.

Compare results of models, we choose bagging classifier because it has a better cross-validation score and wasn’t computationally expensive.


### Deployment
Compare the evaluation of models
Overlay a population heat map with the locations of the pumps


## Setup Instructions

To download the necessary data, please run the following command:

```bash
# installs necessary requirements and downloads necessary data
sh setup.sh
```

## `tanz-water` conda environment

This project relies on you using the [`environment.yml`](environment.yml) file to recreate the `tanz-water` conda environment. To do so, please run the following commands:

```bash
# create the tanz-water conda environment
# note: this make take anywhere from 10-20 minutes
conda env create -f environment.yml

# activate the tanz-water conda environment
conda activate tanz-water

# make tanz-water available to you as a kernel in jupyter
python -m ipykernel install --user --name tanz-water --display-name "Python (tanz-water)"
```
