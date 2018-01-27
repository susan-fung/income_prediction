# Income Prediction

The goal of this project is to develop a classifier to predict whether an individual has an annual income of more than fifty thousand dollars.

## Data
The data set I used is the [Adult data set]( http://archive.ics.uci.edu/ml/datasets/Adult).
The data set is an extraction from the 1994 US census database. There are 14 features and around 49,000 instances in the data set

## Tools
I used the following tools for this project:
-	Jupyter notebook with Python 3 kernel
-	Pandas package for data wrangling
-	Matplotlib for data visualization
-	Scikit-learn package for machine learning

## Data Wrangling
1. Handle missing data. Since only three features (`workclass`, `occupation`, and `native-country`) have missing values, and less than 6% of the data each is missing, I decided to delete the rows with the missing values. The resulting data set has around 45,000 instances
2. Handle categorical data. Since scikit-learn does not handle categorical data and 8 out of 14 features in the data set is categorical, I had to first encode the categorical data. I applied one-hot-encoding to the data set. One-hot-encoding means each level in the categorical feature is converted to a new feature, with a binary representation of whether the level is present (1) or absent (0).

## EDA
Looking at the scatter plot matrix for the continuous variables, there does not seem to be any correlation between them. Moreover, none of them follows a normal distribution.

## Model Selection
I split my data into 3 sets: training(64%), validation(16%) and test (20%).
The criteria to evaluate my model is error rate since false negatives and false positives are not important in this context.
The models I chose to sweep through are:
1.	k-nearest neighbors
2.	Decision tree
3.	Random forest
4.	Log regression
5.	Naive Bayes

Using the validation set, random forest achieved the lowest validation error rate of 16.0%
However, the model is overfitting with 104 features. I performed feature selection next.

## Feature Selection
I selected features based on importance weights (`SelectFromModel` class from scikit-learn)
It reduced the number of features to 17. The validation error was reduced to 13.6%. Overfitting has improved significantly as well

## Hyperparameters Tuning
The three Random Forest hyperparameters I tuned were:
-	`n_estimator`
-	`max_feature`
-	`max_depth`

However, after the hyperparameters tuning, the validation error was increased slightly to 13.9%.

## Final Result
The final model:
-	Random Forest: `n_estimators` = 11, `max_features` = 7, `max_depth` = 11
-	17 features: `age`, `fnlwgt`, `educational-num`, `capital-gain`, `capital-loss`, `hours-per-week`, `workclass_Private`, `education_Bachelors`, `education_HS-grad`, `marital-status_Married-civ-spouse`, `marital-status_Never-married`, `occupation_Exec-managerial`, `occupation_Prof-specialty`, `relationship_Husband`, `relationship_Wife`, `gender_Female`, `gender_Male`

The test error was 14.0%
