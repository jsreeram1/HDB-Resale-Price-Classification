# HDB Flat Resale Price Category Prediction

## Project Description
The objective of this project is to predict the HDB Flat price which categorised as Above Median or Below Median. This prediction would help to identify a flat resale price is Above Median or Below Median resale price and based on this result the buyer or seller of the resale flat can decide whether to buy/sell the flat or not. To achieve this three Machine Learning Models on classification will be developed - Logistic Regression, K-Nearest Neighbour and Decision Tree,  from which one best model will be selected to achieve the object.

---

## Prerequisites and Installations:

### Preresuites:

- *Anaconda or Miniconda - powerful distribution of Python.*
- *Git - Version control system that allows to track changes in code.*
- *Visual Studio Code or VS Code - A lightweight but versatile code editor packed with features that make coding more efficient.*

### Installations:

Follow the following steps to setup the project locally
1. **Clone the repository**

```bash
   git clone https://https://github.com/jsreeram1/HDB-Resale-Price-Classification.git
   cd C:\Users\Ram\Desktop\AIAP Foundation\Classification
```
2. **Install dependencies**

```bash
  pip install -r requirements.txt
```
3. **Set up configuration**:

    Ensure the config.yaml file is in the root directory with the correct path to your dataset (e.g. resale_transactions_categorised.csv).
    Update the target_column, val_size, and other parameters as needed.

4. **Run the project**:

```bash
  python main.py
```

OR

You can follow the steps mentioned below in detail    

Open Anaconda Prompt and create a conda environment - hdbenv for this project, then activate it. Install the following packages one by one, which could be found in requirements.txt

1. numpy
2. matplotlib
3. pandas
4. pip
5. PyYAML
6. scikit-learn
7. seaborn

Open VS Code, select Terminal from View menu, in the below horizontal panel use dropdown menu just near the + sign and select Miniconda, then activate the hdbenv environment. Then, use the Extensions icon from the left panel and select and install both python and jupyter notebook. Use 'Show and Run Commands (Control + Shift + P)' to select the 'Python Create Environment' to create the environment hdbenv and open the jupyter notebook and select the kernel - hdbenv(python 3.11.17) from the list. Now, you can start to use the Jupyter Notebook to do EDA, Data Cleaning and Preprocessing, etc.

## Overview of Key Findings of EDA:  

Import the Pandas. With the help of pandas load CSV data from durectiry where it is downloaded. Then start to explore the data. The followings are the findings of EDA.
 
  1. There are 88688 rows and 14 Columns (including id column)

  2. The data types are float64(1), int64(4), str(9) - value in the bracket is the no. of columns

  3. The following columns are having either empty cells or 'None' value
    
     - 'town_name' - has 783 empty cells
     - 'flatm_name' - has 514 empty cells

  4. Some of the cells of column 'lease_commence_date' has values negative values.

  5. The 'floor_area_sqm' column has a minimum value of 31 sqm.

  6. There are 4223 rows are duplicated and these need to be dropped.   

  7. The columns 'flat_type' is having values 4 ROOM and FOUR ROOM. The value FOUR ROOM  needs to be converted into 4 ROOM, so the value will be standardised.

  8. The columns 'storey_range' is having values like 07 TO 09. These values are in the string format. To convert it into numeric value. Remove the string TO, then average the value 07 and 09 to 8 which will be in the numerical format.

  9. The values of the price_category columns are either Above Median or Below Median and no other values and are in string format. So it should be converedted into binary values 1 or 0.   

## Feature Handling Description:

  1. The missing values of 'twon_name' and 'flatm_name' are filled with the value cooresponds to the 'town_id' and 'flatm_id' which are already available in other rows with the values of 'town_name' and 'flat_name'.  

  2. The typo error in the vlaues of the column 'lease_commence_date' will be removed by using abs() method.  

  3. The duplicated rows will be dripped using drop_duplicates method.

  4. The values of the 'storey_range' columns will be converted into numeric format and the value will be moved to a new column 'storey_midpoint'.

  5. The values of the 'price_category' column will be converted into binary values, ie. Above Median = 1 and Below Median = 0, using map method.

  6. The followings irrelevant columns for the classification model 'id', 'town_id' and 'flatm_id' will be dropped drop method. 

  7. The column 'month' has both year and month, but it is in the string format. It is better to change into datetime format to a new column 'year_month' and then fiburcate it into two columns namely 'year' and 'month' using to_datetime function. Two more columns are added. 'year' - with the value of year, 'year_month' - with the value of 'month' before the above changes. The column 'month' is retained with the value of month. 'year_month'column is not required and so it will be dropped.

  8. After analysing the unique values of 'remaining_lease' column, the valuses should be converted into numerical values. Eg. 67, 81 years 10 months, 86 years will be convereted into months in numerical format and the value will be moved to a new column 'remaining_lease_month'. 'remaining_lease' column will be dropped. 

  9. 'Storey_range', 'block' and 'street_name' column won't be needed due to its irrelavance, so those column will be dropped.

## Instruction for Executing the Pipeline:

On execution of python script 'main.py', with the help of config-yaml file, it will set the basic parameters - to load data, splitting ratio of data, param_grid, numerical features, nominal_features etc., and it will help to execute the pipeline, which will do data preparation, data cleaning and preprocessing, data splitting, modelling, hyperparameter tuning, metrics settings, training the model with the help of train data, validation with the help of validation set, selection of best model and testing the best model on test data.

## Logical Steps/Flow of Pipeline:

The main.py script compiles and orchestrates the entire pipeline by:

- Loading and validating configuration and data

- Initializing data preparation and model training

- Running the training and evaluation process

- Train and evaluate tuned models with hyperparameter tuning

- Combine all models and their metrics into dictionaries

- Finding the best model based on Accuracy score

- Identifying and evaluating the best model on the test set

- Predicting scores for new HDB Resale Flat with a saved model

## Explanation of Model Choices

### Implemented Models

1. \*\*Logistic Regression\*\*: Logistic regression is a classification algorithm. It's often the starting point for binary classification problems, providing a straightforward approach to predicting probabilities of class membership. 

2. \*\*K-Nearest Neighbors\*\*: K-Nearest Neighbors is an intuitive algorithm that classifies observations based on the majority class of their nearest neighbours in the feature space. It's a non-parametric method that can capture complex decision boundaries. 

3. \*\*Decision Tree\*\*: Decision Trees, another non-parametric algorithm, make predictions by following a series of decisions based on the features. They're highly interpretable and can handle both numerical and categorical data effectively.

### Hyperparameter Tuning

- Grid search/Randomized search is performed for Logistic Regression, K-Nearest Neibours and Decision Tree models because each has its own hyperparameter. For Logistic Regression - C (inverse of regularization strength) and penalty (norm used in the penalization, e.g., 'l2'). For K-Nearest Neighbours - n_neighbors (number of neighbors to use) and weights (uniform or distance). For Decision Trees - max_depth (maximum depth of the tree) and min_samples_split (minimum number of samples required to split an internal node).   

- `fit_intercept` is not tuned and is left at `True` because the intercept is the model's starting prediction. `False` would force this starting prediction to zero.

- The final model is selected using cross-validation Accuracy because it shows both typical performance and how stable it is across folds.

## Evaluation Models

### Eavluation Metrics

- **Accuracy**: It measures the proportion of correct predictions among the total number of cases examined. Accuracy is a good measure when your classes are balanced.

- **Precision**: The tool Confusion matrix can be used to calculate Precision, Recall and F1 Score. Precision is the ratio of correctly predicted positive observations to the total predicted positives. It answers the question: "What proportion of predicted positives are actually correct?"
"

- **Recall**: Recall (or Sensitivity) is the ratio of correctly predicted positive observations to all observations in actual class. It answers the question: "What proportion of actual positives was correctly identified?"

- **F1 Score**: The F1 Score is the harmonic mean of Precision and Recall. It is useful when you need a balance between Precision and Recall and there is an uneven class distribution.

### Model Comparison and Results
The data is split into 80% training (67,572), 10% validation (8,447) and 10% test (8,446). All models are compared on the validation set, and the selected model is then evaluated once on the test set.

### Evaluation Results on Validation set

| Model | Accuracy | Precision | Recall | F1 Score | ROC AUC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Logistic Regression | 0.89178 | 0.88979 | 0.89276 | 0.89127 | 0.96000 |
| K-Nearest Neighbours | 0.87627 | 0.88268 | 0.86606 | 0.87429 | 0.94108 |
| Decision Trees | 0.90126 | 0.89948 | 0.90205 | 0.90076 | 0.90279 |

## Consideration for Deployment

### Model Comparison based on GridSearch CV

| Model | Accuracy | Precision | Recall | F1 Score | ROC AUC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Logistic Regression | 0.89261 | 0.89015 | 0.89418 | 0.89217 | 0.89262 |
| K-Nearest Neighbours  | 0.87888 | 0.88610 | 0.86773 | 0.87682 | 0.87881 |
| Decision Trees  | 0.89238 | 0.90832 | 0.87131 | 0.88943 | 0.89224 |
 
### Model Comparison based on RandomisedSearch CV

| Model | Accuracy | Precision | Recall | F1 Score | ROC AUC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Logistic Regression | 0.89261 | 0.89015 | 0.89418 | 0.89217 | 0.89262 |
| K-Nearest Neighbours  | 0.87888 | 0.88610 | 0.86773 | 0.87682 | 0.87881 |
| Decision Trees  | 0.87781 | 0.91027 | 0.83651 | 0.87183 | 0.87755 |

While comparing all the model, the Decision Tree Classifier model's Precision value is greater than that of other two models. So the Decision Tree Classifer model is selected as the best model.

### Application of best model on Test Data

Best Decision Tree Classifier Model, Final Test Metrics:

| Metrics | Value |
|---|---|
| Final Test Accuracy: | 0.8832721676334794 |
| Final Test Precision: | 0.9203771608171818 |
| Final Test Recall: | 0.8374642516682554 |
| Final Test F1: | 0.876965310706264 |
| Final Test ROC AUC: | 0.8829758331971012 |


