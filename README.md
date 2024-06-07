# Project-4: Predicting Credit Customer Turnover

* <a href='#overview'>Overview</a></br>
* <a href='#presentation'>Presentation</a><br/>
* <a href='#how-to-run'>How to Run</a><br/>




### Overview
<strong><i>Team Members</i></strong>: Credit Crunchers: Leanne Novo, Beyonka Powell, Brian Quintero, Chuchu Wang 
<img src="https://github.com/lgnovo/Project-4/blob/main/Images/readme.png?raw=true">


###  How do you predict customer turnover?
Our group aims to develop a model that evaluates the likelihood of credit card customer turnover by analyzing data from a comprehensive consumer credit card portfolio-this includes demographic, financial, and (banking) behavioral information contained in the following dataset:
 <a href="https://www.kaggle.com/datasets/thedevastator/predicting-credit-card-customer-attrition-with-m">Kaggle</a> 
We will identify variables that best predict customer attrition by focusing on variables most strongly associated with attrition flags as potential attrition predictors. The goal is to build and leverage the most relevant and effective models to craft recommendations for presentation to our hypothetical banking client (ie. increase/decrease credit limit, promotional offers, etc.) to improve client retention


### Presentation
Our group presentation will encompass an introduction to our model and its relevance to our research question. The goal is to present our model’s aptitude to make predictions and any hypothetical business practice recommendations.
[View Presentation Here](https://docs.google.com/presentation/d/1iTG4Il5VhoeqTq4OCIaFIKuo9iCqtARKabFU-kqVb3Y/edit#slide=id.p)

________________________________________________________________

## Random Forest Model

1. Import findspark and initialize. Import packages. Create a SparkSession.<br/>

2. Read in the AWS S3 bucket into a DataFrame.
```sql
        from pyspark import SparkFiles
        url = "https://groupfourproject.s3.ca-central-1.amazonaws.com/bank_churners.csv"
        spark.sparkContext.addFile(url)
        df = spark.read.csv(SparkFiles.get("bank_churners.csv"), sep=",", header=True, ignoreLeadingWhiteSpace=True)
        df.show()
```
<br/>

3. Clean data set and drop columns.<br/>

4. Look at and identify the distribution on each variables. Here is an example of the distrubution on customer age.<br/>
<img src="https://github.com/lgnovo/Project-4/blob/chuchu/images/example_customer_age_distribution.png?raw=true"><br/>

5. Identify the Feature importances. <br/>
We were able to identfy the top 10 importance features. Understanding which features most influence customer attrition can help align business strategies with customer needs and behaviors. For instance, if 'Months_on_book' is a top feature, it might indicate that loyalty programs or periodic check-ins with long-term customers could be beneficial. By understanding the key drivers of customer behavior, organizations can develop more targeted and proactive retention strategies.
Here is the top 10 Important Features: 
<br/><img src="https://github.com/lgnovo/Project-4/blob/chuchu/images/top_10_important_features.png?raw=true"><br/>

<strong><i>Key Findings</i></strong>:
<br/> 
        - Total Revolving Balance: Total revolving balance is the total amount of money customers owe on revolving credit accounts, such as credit cards, at a given time. 
                - Offering debt management programs or financial counseling to customers with high revolving balances could help reduce attrition by assisting them in managing their finances better. 
                - For customers with low revolving balances, offer reward programs, cashbacks, or lower interest rates, this will help with increase engagement and reduce attrition. 
<br/>  
        - Average Utilization Ratio: The average utilization ratio is the percentage of a customer's available credit that they are using. 
                -  Monitoring utilization ratios and offering proactive credit line adjustments (increases or decreases) can help manage customer satisfaction and retention.
                - Educating customers on the benefits of maintaining a healthy utilization ratio and how it affects their credit scores might help in reduce financial stress 
<br/> 
        - Focusing on Total_Revolving_Bal, Avg_Utilization_Ratio, and Credit_Limit as the top features for predicting customer attrition provides a clear path for actionable strategies. By understanding the financial health and behavior of customers, institutions can implement targeted interventions to manage and reduce attrition effectively. This data-driven approach ensures that resources are allocated efficiently, customer needs are met proactively, and overall customer loyalty is enhanced.<br/>
<br/> 

6. Run Random Forest Model:<br/>
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

7. Improve Model:<br/>
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

8. Conclusion: <br/>
We were able to imporve Random Forest model, after balancing the classes, performs exceptionally well in predicting customer attrition. The high accuracy, precision, recall, and F1-scores across both classes demonstrate the model's robustness and reliability.<br/>
