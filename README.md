# Project-4: Predicting Credit Customer Turnover

* <a href='#overview'>Overview</a></br>
  - <a href='#how-to-run-any-of-the-models'>How to Run</a><br/>
* <a href='#decision-tree-classifier'>Decision Tree Classifier</a><br/>
* <a href='#k-nearest-neighbors'>K Nearest Neighbors</a><br/>
* <a href='#random-forest-model'>Random Forest Model</a><br/>
* <a href='#final-conclusion'>Conclusion and Recommendations</a><br/>


## Overview

<img src="https://github.com/lgnovo/Project-4/blob/main/Images/readme.png?raw=true">

###  How do you predict customer turnover?
Our group aims to develop a model that evaluates the likelihood of credit card customer turnover by analyzing data from a comprehensive consumer credit card portfolio-this includes demographic, financial, and (banking) behavioral information contained in 
 <a href="https://www.kaggle.com/datasets/thedevastator/predicting-credit-card-customer-attrition-with-m">this dataset from Kaggle.</a> 
We will identify variables that best predict customer attrition by focusing on variables most strongly associated with attrition flags as potential attrition predictors. The goal is to build and leverage the most relevant and effective models to craft recommendations for presentation to our hypothetical banking client (ie. increase/decrease credit limit, promotional offers, etc.) to improve client retention.

## Github Introduction

<img src="https://github.com/lgnovo/Project-4/assets/153560368/a3872152-676d-446e-b6b2-d7faed3d3c94" width="500" height="200"><br/>
* **Analysis components** - code used to perform analyses within the project, with analyst initials.
    - **Resource:** contains the csv used for this project from <a href="https://www.kaggle.com/datasets/thedevastator/predicting-credit-card-customer-attrition-with-m"> Kaggle.</a> 
* **Images** - stores image files used for Readme

### [Presentation](https://docs.google.com/presentation/d/1iTG4Il5VhoeqTq4OCIaFIKuo9iCqtARKabFU-kqVb3Y/edit#slide=id.p)
An introduction to our models and their aptitude to make predictions, and any hypothetical business practice recommendations.

________________________________________________________________
## How to Run any of the models
1. Import findspark and initialize. Import packages. Create a SparkSession
2. Read in the AWS S3 bucket into a DataFrame.
```sql
        from pyspark import SparkFiles
        url = "https://groupfourproject.s3.ca-central-1.amazonaws.com/bank_churners.csv"
        spark.sparkContext.addFile(url)
        df = spark.read.csv(SparkFiles.get("bank_churners.csv"), sep=",", header=True, ignoreLeadingWhiteSpace=True)
        df.show()
```
3. Clean data set and drop columns.
________________________________________________________________
# Decision Tree Classifier

After following <a href='#how-to-run'>How to Run,</a><br/>

**4. Demographic Data to Analyze using Spark SQL** <br/>
<img src="https://github.com/lgnovo/Project-4/blob/main/Images/fig%203_attrited%20dist.png?raw=true" width="600" height="320"> <br/>
   **_Key Finding:_** Existing Customers (85,000) data far outweighs Attrited Customer (1,627).

**5. Maniplating Attrition_Flag** to 0= Attrited Customer and 1= Existing Customer

**6. Setting up Unbalanced Train/Test Modeling** There are more existing customers than attrited customers. Unbalanced data is referring to the original cleaned dataset.

**7. Run DecisionTreeClassifier**
   
**8. Training/Test 1st Model- Unbalanced**

* **_Training Metrics:_**
  - Precision: 0.8034 (80.34%)
  - F1 Score: 0.9353 (93.53%)
  - Recall: 0.7917 (79.17%)
  - Accuracy: 0.9354 (93.54%)
  - **_Implications:_** The high F1 score and accuracy indicate that the model is performing very well overall. The precision is slightly higher than the recall, suggesting that the model is somewhat more conservative in its positive predictions, prioritizing avoiding false positives over missing some true positives. The recall is also reasonably high, indicating that the model is still capturing the majority of the actual positive instances.

* **_Test Metrics:_**
  - Precision: 0.7842 (78.42%)
  - F1 Score: 0.9315 (93.15%)
  - Recall: 0.7914 (79.14%)
  - Accuracy: 0.9314 (93.14%)
  - **_Implications:_** The model demonstrates strong performance on both training and test data, with only slight decreases in precision and accuracy on the test data, which is typical and indicates good generalization. The high F1 score and consistent recall values suggest that the model is robust in identifying positive instances while maintaining a balance between precision and recall across both datasets. The slightly lower precision on the test data suggests a few more false positives compared to the training data, but this is not significantly detrimental.

**9. Traning/Testing 2nd Model- Balanced**

* **_Training Metrics_**
  - Precision: 0.9310 (93.10%)
  - F1 Score: 0.9205 (92.05%)
  - Recall: 0.9041 (90.41%)
  - Accuracy: 0.9206 (92.06%)
  - **_Implications:_** The high precision (93.10%) indicates that when the model predicts a positive instance, it is very likely to be correct, showing a strong ability to avoid false positives. The F1 score (92.05%) reflects a well-balanced model performance, effectively combining precision and recall. The high recall (90.41%) suggests that the model is adept at identifying the majority of true positives, with a relatively small number of false negatives. The accuracy (92.06%) indicates a high overall correctness in predictions, signifying that the model is reliable in making accurate predictions.

* **_Test Metrics:_**
  - Precision: 0.9102 (91.02%)
  - F1 Score: 0.8969 (89.69%)
  - Recall: 0.8837 (88.37%)
  - Accuracy: 0.8969 (89.69%)
  - **_Implications:_** The high precision (91.02%) indicates that when the model predicts a positive instance, it is very likely to be correct, showing a strong ability to avoid false positives even on the test data. The F1 score (89.69%) reflects a well-balanced model performance, effectively combining precision and recall, though slightly lower than the training F1 score. The high recall (88.37%) suggests that the model is adept at identifying the majority of true positives, with a relatively small number of false negatives on the test data. The accuracy (89.69%) indicates a high overall correctness in predictions, signifying that the model is reliable in making accurate predictions on new data.


**Comparison of Train Prediction:**

The precision for the second train prediction (93.10%) is significantly higher than the first train prediction (80.34%), indicating an improvement in avoiding false positives. The F1 score for the second train prediction (92.05%) is slightly lower than the first (93.53%), suggesting a minor drop in the balance between precision and recall.
The recall for the second train prediction (90.41%) is higher than the first (79.17%), showing a notable improvement in capturing true positives.
The accuracy for the second train prediction (92.06%) is slightly lower than the first (93.54%), indicating a small decrease in overall prediction correctness.

**Comparison of Test Prediction:**

The precision for the second test prediction (91.02%) is slightly lower than the second train prediction (93.10%), indicating a minor increase in false positives when applied to unseen data. The F1 score for the second test prediction (89.69%) is slightly lower than the second train prediction (92.05%), suggesting a minor drop in the balance between precision and recall. The recall for the second test prediction (88.37%) is slightly lower than the second train prediction (90.41%), showing a slight decrease in capturing true positives. The accuracy for the second test prediction (89.69%) is slightly lower than the second train prediction (92.06%), indicating a minor drop in overall prediction correctness on the test data.

### Conclusion
The 1st model demonstrates strong performance on both the training and test datasets. With training metrics showing a precision of 80.34%, an F1 score of 93.53%, recall of 79.17%, and accuracy of 93.54%, the model is effective at making accurate and balanced predictions. The test metrics, with a precision of 78.42%, an F1 score of 93.15%, recall of 79.14%, and accuracy of 93.14%, indicate that the model generalizes well to new data with only a minor decrease in performance. This robust performance across both datasets suggests that the model is reliable and well-suited for practical applications.

The 2nd model exhibits strong performance on both the training and test datasets. The training metrics show high precision (93.10%), F1 score (92.05%), recall (90.41%), and accuracy (92.06%), indicating the model's effectiveness in making accurate predictions and identifying positive instances. The test metrics, with precision at 91.02%, F1 score at 89.69%, recall at 88.37%, and accuracy at 89.69%, confirm that the model generalizes well to new data. Despite a slight drop in performance on the test data, the metrics remain robust, highlighting the model's reliability and balanced performance.
________________________________________________________________
# K Nearest Neighbors

**4. Demographic Data to Analyze using Spark SQL** <br/>
   **_Key Finding:_** Existing Customers (85,000) data far outweighs Attrited Customer (1,627) (from <a href='#decision-tree-classifier'>Decision Tree Classifier</a>).

**5. Maniplating Attrition_Flag** to 0= Attrited Customer and 1= Existing Customer

**6. Setting up Balanced and Unbalanced Train/Test Modeling** 

**7. Run K-Means Algorithm for balanced and unbalanced datasets**

**7. Run Pricipal Component Analysis (PCA) for balanced and unbalanced datasets**

The ideal K remains at 3. This will inform the K Nearest Neighbors analysis for K=3:

**7. Run K Nearest Neighbors for balanced and unbalanced datasets**

* **_Unbalanced:_**
```
Confusion Matrix:
[[ 268   64]
 [ 137 2063]]

Classification Report:
              precision    recall  f1-score   support

           0       0.66      0.81      0.73       332
           1       0.97      0.94      0.95      2200

    accuracy                           0.92      2532
   macro avg       0.82      0.87      0.84      2532
weighted avg       0.93      0.92      0.92      2532
```
* **_Balanced:_**
```
Confusion Matrix:
[[403   0]
 [  2 409]]

Classification Report:
              precision    recall  f1-score   support

           0       1.00      1.00      1.00       403
           1       1.00      1.00      1.00       411

    accuracy                           1.00       814
   macro avg       1.00      1.00      1.00       814
weighted avg       1.00      1.00      1.00       814
```
________________________________________________________________

# Random Forest Model

After following <a href='#how-to-run'>How to Run,</a><br/>

**4. Look at and identify the distribution on each variables. Here is an example of the distrubution on customer age.** <br/>
<img src="https://github.com/lgnovo/Project-4/blob/chuchu/images/example_customer_age_distribution.png?raw=true"><br/>

**5. Identify the Feature importances.** <br/>
We were able to identfy the top 10 importance features. Understanding which features most influence customer attrition can help align business strategies with customer needs and behaviors. For instance, if 'Months_on_book' is a top feature, it might indicate that loyalty programs or periodic check-ins with long-term customers could be beneficial. By understanding the key drivers of customer behavior, organizations can develop more targeted and proactive retention strategies.
Here is the top 10 Important Features: 
<br/><img src="https://github.com/lgnovo/Project-4/blob/chuchu/images/top_10_important_features.png?raw=true"><br/>

**_Key Findings_**:

* **Total Revolving Balance**: Total revolving balance is the total amount of money customers owe on revolving credit accounts, such as credit cards, at a given time.
  - Offering debt management programs or financial counseling to customers with high revolving balances could help reduce attrition by assisting them in managing their finances better.
  - For customers with low revolving balances, offering reward programs, cashbacks, or lower interest rates could increase engagement and reduce attrition.

* **Average Utilization Ratio**: The average utilization ratio is the percentage of a customer's available credit that they are using.
  - Monitoring utilization ratios and offering proactive credit line adjustments (increases or decreases) can help manage customer satisfaction and retention.
  - Educating customers on the benefits of maintaining a healthy utilization ratio and how it affects their credit scores might help reduce financial stress.

* **Focusing on Total Revolving Balance, Average Utilization Ratio, and Credit Limit** as the top features for predicting customer attrition provides a clear path for actionable strategies. By understanding the financial health and behavior of customers, institutions can implement targeted interventions to manage and reduce attrition effectively. This data-driven approach ensures that resources are allocated efficiently, customer needs are met proactively, and overall customer loyalty is enhanced.

**6. Run Random Forest Model:** <br/>
We decide to run the Model with top 10 important features to look at the accuracy score. Overall Accuracy: The model has an overall accuracy of 0.88, which means it correctly classified 88% of the instances in the test set.  However, The recall for class 1 is relatively low at 0.33. This means that the model is only identifying 33% of the actual attrited customers. Therefore, we decide to conduct some improvemnt, like using top 5 features or adjust the calss weigh of the data to try to improce recall. <br/>
```
Accuracy: 0.88
Classification Report:
              precision    recall  f1-score   support

           0       0.88      0.99      0.94      2543
           1       0.89      0.33      0.49       496

    accuracy                           0.88      3039
   macro avg       0.89      0.66      0.71      3039
weighted avg       0.89      0.88      0.86      3039

Confusion Matrix:
[[2523   20]
 [ 330  166]]
```
<br/>

**7. Improve Model:** <br/>
The final improvement model is to resample calss weight and still use top 10 features. We were able to get 97% accurancy and improved the class 1 (Attrited Customer) F1 score to 0.99. 
```
Accuracy: 0.97
Classification Report:
              precision    recall  f1-score   support

           0       0.99      0.96      0.97      2568
           1       0.96      0.99      0.97      2532

    accuracy                           0.97      5100
   macro avg       0.97      0.97      0.97      5100
weighted avg       0.97      0.97      0.97      5100

Confusion Matrix:
[[2453  115]
 [  25 2507]]
```
<br/>

### Conclusion: <br/>
We were able to imporve Random Forest model, after balancing the classes, performs exceptionally well in predicting customer attrition. The high accuracy, precision, recall, and F1-scores across both classes demonstrate the model's robustness and reliability.<br/>
________________________________________________________________
