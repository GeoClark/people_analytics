# people_analytics
The following document provides a detailed technical analysis of employee retention at an unknown company.  The notebook applies a combination of logistic regression and random forest classification to predict employee attrition from a Kaggle dataset.

## Introduction
Employee salary and benefits are among the highest expenses for any business. A productive and well-trained workforce can provide excellent returns to any company.  Low productivity and employee attrition provide significant headwinds against a company's competitiveness.  The company with the best combination of people, products, and process wins.  
The notebook uses a combination of modeling and visualizations to identify important predictors for predicting employee retention and attrition.
The notebook used a publically available dataset from Kaggle with 35 features and 1470 rows, each representing an employee.  Each employee was labeled with their attrition status ("Yes" or "No").  A mix of visualizations was used to assess the relative importance of each feature on employee retention.
The modeling workflow is straightforward for an imbalanced classification problem.  Data were oversampled to balance the two classes and scaled for modeling.
After creating dummy variables for categorical features the 35 features swell to 50 features.  Recursive feature selection subsets the optimum model with k features from 2 to 50.  Models with 10-15 features balance accuracy and interpretability. A combination of Logistic Regression (LR) and Random Forest Classification (RFC) is implemented with success. LR  provides superior interpretability and RFC provides better accuracy.  The log odds coefficients of the LR model are converted to base odds to understand the relative impact of an increasing feature on an employee's attrition odds.

## Exploratory Data Analysis (EDA)
 ### Data
 The original dataset was pulled from Kaggle. In general, the dataset required minimal processing.  Data were complete and non-duplicated, minimal imputation was required. The majority of processing was for getting the correct column types and creating dummy variables.  The original dataset was ~1500 rows and 35 features. After processing the dataset there were a total of 49 features. Just over 200 of the employees in the dataset had left the company representing ~15% of all rows (**Figure 1**).
 
 ### Exploration
 ![image](https://user-images.githubusercontent.com/30851535/185760995-6ee08440-f27b-4e6e-83ce-2cda2cc7af4a.png)
_**Figure 1:** A barplot showing the frequency of attriting employees in the dataset._ Figure 1 shows the relative frequency of attrition.

 Data were rescaled and analyzed for outliers (**Figure 2**).  Values with a z score >3 were considered outliers. The z score is the number of standard deviations from the mean. Although there are ~10-20 rows that meet our threshold for outliers, they were left in as the value may be extreme but is considered to be extreme but feasible.
 
 ![image](https://user-images.githubusercontent.com/30851535/185760725-e75b863d-2764-4e9a-ac1d-23ef1406c296.png)
_**Figure 2:** A boxplot showing the range in z scores for all values in each variable. A value greater than 3 is considered to be an outlier._

 ### Observations
 A detailed exploration of the dataset was conducted. In that evaluation, the relationship between attrition and age was identified as being a major predictor of attrition.  Other properties such as compensation, stock options, years of experience, years with a given supervisor, and overtime all correlate strongly with age and department  **Figures 3-6** demonstrate the key relationships between attrition and other features in the dataset.  **Figure 3** illustrates that younger employees attrite more often than older employees.  **Figure 4** illustrates that attriting employees work overtime more than retained employees.  **Figure 5** shows that a larger proportion of attriting employees have lower stock options.  This relationship correlates with age as well; a smaller proportion of younger employees have stock options. 
  ![image](https://user-images.githubusercontent.com/30851535/185760948-c6ccb211-6e47-4974-b2b0-3bc83c45b7ef.png)
  
_**Figure 3:** A boxplot comparing the ages of attriting employees._

 ![image](https://user-images.githubusercontent.com/30851535/185761539-2c275be9-f1ce-48b6-85a1-50f5b4cf11ae.png)
 
_**Figure 4:** A barplot showing the proportion of attriting employees by overtime._

![image](https://user-images.githubusercontent.com/30851535/185761577-308972b4-aaf6-4fb0-86c0-7d0cd6d9f0b5.png)

_**Figure 5:** A barplot showing the proportion of attriting employees by the level of stock options._

![image](https://user-images.githubusercontent.com/30851535/185761072-8a839acd-dc14-431e-9c64-2ff82ae4d79d.png)

_**Figure 6:** A radar plot comparing the mean values of relevant columns for both attrition groups._

 **Figure 6** brings a lot of these insights together.  The radar plots compare the means of the standardized values of all continuous columns in the data set. The values were standardized so the mean of each column is 0. The "No attrition" group accounts for ~80% of the total, therefore most columns are centered around 0. There are some key differences between the two groups that match our observations in the previous plots.
Monthly rate, stock options, monthly income, job satisfaction, job level, Environment satisfaction, age, and total working years all significantly lower the no-attrition group.  Distance from home and Number of companies worked is higher than the no-attrition group.  Most of the people who leave are younger, unhappy with work, live further from home, and/or have left other companies before.

## Modelling
After extensive cleaning and digging into the data it is time to build a model. Model building has dual purposed; 1. it is an extension of the EDA that allows an analysis to quantify the importance of certain variables. 2. The model can be used for predictive purposes; in this case a classifiaction model will take relevant features and predict whether an employee will be retained.  For this project the following workflow was used:
1. Data were oversampled and balnaced using the SMOTE package
2. The model was iterated to solve for the ideal subset of features
3. The workflow was used for Logistic Regression and a Random Forest Classifier (RF).  The LR model was converted from log odds to base odds for increased interpretability.
4. The performance of the RF was quantified.  The model was tuned using a bayesian optimizer, the perofrmance of the tuned model was then evaluated.

The attrition dataset has nearly 1500 rows but the dataset is "unbalanced"; attriting employees represent only 15% of the total rows.  Applying SMOTE oversamples the vectors between actual "Yes Attrition" values.  The effect of the oversampling is clear in **Figure 7**.  

 ![image](https://user-images.githubusercontent.com/30851535/185761742-71336b3c-d1df-4957-886c-d07b010879ef.png)
 
_**Figure 7:** A barplot showing the proportion of attriting employees after oversampling with SMOTE._

### Logistic Regression

 Logistic regression classification is an extension of linear methods used for binomial classification. In this case predicting "Yes" or "No" attrition.
 The model was applied on 2-29 parameters to assess the ideal number of features **Figure 8**.

![image](https://user-images.githubusercontent.com/30851535/185763275-a9ff0a46-ae93-4f42-b8d5-19df88e07fd8.png)

_**Figure 8:** A boxplot comparing the error of a logistic regression model for the best model with n_features. Models are untuned._

**Figure 8** shows the accuracy for the best subset of k-features.  There's a quick diminishing return after 8 variables with the ideal count around 12 features.  The iideal number of features could vary depending on the needs of the business partner.  If interpretatibility is the main concer then 10-12 variables is a good optimum.  If accuracy is the armaount concern the user could introduce additional variables.  Random Forest or possibly SVM should be used in plaec of logistic regression if accuracy is the primary metric of concern.  The coefficients of logistic regression are expressed as log odds, coefficients are more interpreatble when converted to base odds (**Figure 9 **)

![image](https://user-images.githubusercontent.com/30851535/185772714-2835fcd7-49ac-46ea-97f1-863ec7fbe4b9.png)

_**Figure 9:** An output of the best 10 feature model with base odds calculated in the final column._

**Figure 9** provides an example of the principal advantage of logistic regression over other machine learning algorithms, interpreatibility.  The odds ratio can be interpreted as follows: for all else being equal a single unit increase in the feature of interest results in a change of x to the base odds.  A value <1 decreases the odds of a positive classification and a value >1 increases the odds.  For example, overtime increases the base odds of attrition by 91% and each stock option level decreases the base odds of attrition by 32%.  The top 3 features that decrease the odds of attrition are stock option level, job involvement, and age.The top 3 features that increase the odds of attrition are overtime (Yes or No), number of companies worked, and monthly rate.  Changes to company policy can be recommended from this model.  For example a reexamination of overtime policies and  eligibility for stock options.  A random forest model was built and tuned to increase the accuracy of the predictions.

### Random Forest Classifier


![image](https://user-images.githubusercontent.com/30851535/185763934-2d11d002-6fb7-442d-be43-73e57b66357f.png)

_**Figure X:** A boxplot comparing the error of a Random Forest Classification model for the best model with n_features. Models are untuned._

### Discussion
## Recommendations and Conclusions















