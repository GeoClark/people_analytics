# people_analytics
The followig document provides a detailed techical analysis of employee retention at an unknown company.  The notebook applies a combination of logistic regression and random forest classification to predict employee attrition from a Kaggle dataset.

## Introduction
Employee salary and benefits are among the ihghest expenses for any business. A productive and well-trained workflorce can provide excellent returns to any company.  Low productivity and employee attrition provide significant headwinds against a comapany's competetiveness.  The company with the best combination of people, products, and process wins.  
The notebook uses a combination of modelling and visualizations to identify important predictors for predicting employee retention and attrition.
The notebook used a publically available dataset from Kaggle with 35 features and 1470 rows, each representing an employee.  Each employee was labeled with their attrition status ("Yes" or "No").  A mix of visualizations were used to asses the relative importance of each feature on empoyee retention.
The modelling workflow is straightforward for an imbalanced classificiation problem.  Data were oversampled to balance the two classes and scaled for modelling.
After creating dummy variables for caegorical features the 35 features swell to 50 features.  Recursive feature selection subsets the optimum model with k features from 2 to 50.  Models with 10-15 features balance accuracy and interpretability. A combination of Logistic Regression (LR) and Random Forest Classiciation (RFC) are implemented with good success. LR  provides superior interpretability and RFC provides better accuracy.  The log odds coefficients of the LR model are converted to base odds to understand the realtive impact of an increasing feature on an employee's attrtion odds.

## Exploratory Data Analysis (EDA)
 ### Data
 The original dataset was pulled from Kaggle. In general the dataset required minimal processing.  Data were complete and non-duplicated, minimal imputation was required. The majority of processing was for getting the correct column types and creating dummy variables.  The orgiinal dataset was ~1500 rows and 35 features. After processing the dataset there were a total of 49 features.  Data were rescaled and analyzed out outliers (**Figure 1**).  Values with a z score >3 were considered outliers. The z score is the number of standard deviations from the mean. Although there are ~10-20 rows that meet our threshold for outliers, they were left in as the value may be extreme but are considered possible.
 
 ![image](https://user-images.githubusercontent.com/30851535/185760725-e75b863d-2764-4e9a-ac1d-23ef1406c296.png)
_**Figure 1:** A boxplot showing the range in z scores for all values in each variable. A value greater than 3 is considered to be an outlier._
 
 
 ![image](https://user-images.githubusercontent.com/30851535/185760948-c6ccb211-6e47-4974-b2b0-3bc83c45b7ef.png)
_**Figure X:** A boxplot comparing the ages of attriting employees._


 ![image](https://user-images.githubusercontent.com/30851535/185760995-6ee08440-f27b-4e6e-83ce-2cda2cc7af4a.png)
_**Figure X:** A barplot showing the frequency of attrting employees in the dataset._

 
 ![image](https://user-images.githubusercontent.com/30851535/185761072-8a839acd-dc14-431e-9c64-2ff82ae4d79d.png)
_**Figure X:** A radar plot comparing the mean values of relevant columns for both attrition groups._

 
  scaling
  rebalancing
 ### Observations
 pertinent box plots
## Modelling
### Logistic Regression
### Random Forest
### Discussion
## Recommendations and Conclusions
