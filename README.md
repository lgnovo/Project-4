# Project-4: Predicting Credit Customer Turnover

* <a href='#overview'>Overview</a></br>
* <a href='#background-information'>Background Information</a></br>
* <a href='#presentation'>Presentation</a><br/>
* <a href='#how-to-run'>How to Run</a><br/>




## Overview
<strong><i>Team Members</i></strong>: Credit Crunchers: Leanne Novo, Beyonka Powell, Brian Quintero, Chuchu Wang 
<img src="https://github.com/lgnovo/Project-4/blob/leanne/Image.png?raw=true">


##  How do you predict customer turnover?
Our group aims to develop a model that evaluates the likelihood of credit card customer turnover by analyzing data from a comprehensive consumer credit card portfolio-this includes demographic, financial, and (banking) behavioral information contained in the following dataset:
 <a href="https://www.kaggle.com/datasets/thedevastator/predicting-credit-card-customer-attrition-with-m">Kaggle</a> 
We will identify variables that best predict customer attrition by focusing on variables most strongly associated with attrition flags as potential attrition predictors. The goal is to build and leverage the most relevant and effective models to craft recommendations for presentation to our hypothetical banking client (ie. increase/decrease credit limit, promotional offers, etc.) to improve client retention


## Presentation
Our group presentation will encompass an introduction to our model and its relevance to our research question. The goal is to present our model’s aptitude to make predictions and any hypothetical business practice recommendations.
[View Presentation Here](https://docs.google.com/presentation/d/1iTG4Il5VhoeqTq4OCIaFIKuo9iCqtARKabFU-kqVb3Y/edit#slide=id.p)


## How to Run
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
  
4. Demographic and Behavioral Data to Analyze using Spark SQL

5. Maniplating Attrition_Flag to 0= Attrited Customer and 1= Existing Customer

6. Setting up Unbalanced Train/Test Modeling

7. Run DecisionTreeClassifier
   
8. Training/Test 1st Model- Unbalanced

Key Finding: Higher count of Existing Customer (85000) vs. Attrited Customer(1627) which is biased towards Existing Customer.

![Unbalanced](https://github.com/lgnovo/Project-4/assets/111611012/517864ac-e64d-455e-99a9-0026c51dfd9e)

# Training Metrics:
•	Precision: 0.8034 (80.34%)
•	F1 Score: 0.9353 (93.53%)
•	Recall: 0.7917 (79.17%)
•	Accuracy: 0.9354 (93.54%)

# Implications:

The high F1 score and accuracy indicate that the model is performing very well overall. The precision is slightly higher than the recall, suggesting that the model is somewhat more conservative in its positive predictions, prioritizing avoiding false positives over missing some true positives. The recall is also reasonably high, indicating that the model is still capturing the majority of the actual positive instances.

# Test Metrics:
•	Precision: 0.7842 (78.42%)
•	F1 Score: 0.9315 (93.15%)
•	Recall: 0.7914 (79.14%)
•	Accuracy: 0.9314 (93.14%)

# Implications:

The model demonstrates strong performance on both training and test data, with only slight decreases in precision and accuracy on the test data, which is typical and indicates good generalization.The high F1 score and consistent recall values suggest that the model is robust in identifying positive instances while maintaining a balance between precision and recall across both datasets.The slightly lower precision on the test data suggests a few more false positives compared to the training data, but this is not significantly detrimental.

9. Traning/Testing 2nd Model- Balanced Data

Balancing count of Existing Customer (1984) vs. Attrited Customer(1627) which is biased towards Existing Customer.

![balanced](https://github.com/lgnovo/Project-4/assets/111611012/db497f8b-8100-47d3-98a9-3c78744c6caa)

# Training Metrics:
•	Precision: 0.9310 (93.10%)
•	F1 Score: 0.9205 (92.05%)
•	Recall: 0.9041 (90.41%)
•	Accuracy: 0.9206 (92.06%)

# Implications:

The high precision (93.10%) indicates that when the model predicts a positive instance, it is very likely to be correct, showing a strong ability to avoid false positives.The F1 score (92.05%) reflects a well-balanced model performance, effectively combining precision and recall.
The high recall (90.41%) suggests that the model is adept at identifying the majority of true positives, with a relatively small number of false negatives. The accuracy (92.06%) indicates a high overall correctness in predictions, signifying that the model is reliable in making accurate predictions.

# Test Metrics:
•	Precision: 0.9102 (91.02%)
•	F1 Score: 0.8969 (89.69%)
•	Recall: 0.8837 (88.37%)
•	Accuracy: 0.8969 (89.69%)

# Implications:

The high precision (91.02%) indicates that when the model predicts a positive instance, it is very likely to be correct, showing a strong ability to avoid false positives even on the test data. The F1 score (89.69%) reflects a well-balanced model performance, effectively combining precision and recall, though slightly lower than the training F1 score. The high recall (88.37%) suggests that the model is adept at identifying the majority of true positives, with a relatively small number of false negatives on the test data. The accuracy (89.69%) indicates a high overall correctness in predictions, signifying that the model is reliable in making accurate predictions on new data.

# Comparison of Train Prediction:

The precision for the second train prediction (93.10%) is significantly higher than the first train prediction (80.34%), indicating an improvement in avoiding false positives. The F1 score for the second train prediction (92.05%) is slightly lower than the first (93.53%), suggesting a minor drop in the balance between precision and recall.
The recall for the second train prediction (90.41%) is higher than the first (79.17%), showing a notable improvement in capturing true positives.
The accuracy for the second train prediction (92.06%) is slightly lower than the first (93.54%), indicating a small decrease in overall prediction correctness.

# Comparison of Test Prediction:

The precision for the second test prediction (91.02%) is slightly lower than the second train prediction (93.10%), indicating a minor increase in false positives when applied to unseen data. The F1 score for the second test prediction (89.69%) is slightly lower than the second train prediction (92.05%), suggesting a minor drop in the balance between precision and recall. The recall for the second test prediction (88.37%) is slightly lower than the second train prediction (90.41%), showing a slight decrease in capturing true positives. The accuracy for the second test prediction (89.69%) is slightly lower than the second train prediction (92.06%), indicating a minor drop in overall prediction correctness on the test data.

# Conclusion
The 1st model demonstrates strong performance on both the training and test datasets. With training metrics showing a precision of 80.34%, an F1 score of 93.53%, recall of 79.17%, and accuracy of 93.54%, the model is effective at making accurate and balanced predictions. The test metrics, with a precision of 78.42%, an F1 score of 93.15%, recall of 79.14%, and accuracy of 93.14%, indicate that the model generalizes well to new data with only a minor decrease in performance. This robust performance across both datasets suggests that the model is reliable and well-suited for practical applications.

The 2nd model exhibits strong performance on both the training and test datasets. The training metrics show high precision (93.10%), F1 score (92.05%), recall (90.41%), and accuracy (92.06%), indicating the model's effectiveness in making accurate predictions and identifying positive instances. The test metrics, with precision at 91.02%, F1 score at 89.69%, recall at 88.37%, and accuracy at 89.69%, confirm that the model generalizes well to new data. Despite a slight drop in performance on the test data, the metrics remain robust, highlighting the model's reliability and balanced performance.
