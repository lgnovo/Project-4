# Project-4
Credit Crunchers
<a href='#overview'>Overview</a></br>
<a href='#background-information'>Background Information</a></br>
<a href='#presentation'>Presentation</a><br/>
<a href='#how-to-run'>How to Run</a><br/>




## Overview
<strong><i>Overview</i></strong>: 
<br/><br/>
<img src="https://github.com/lgnovo/Project-4/blob/leanne/Image.png?raw=true">
<br/><br/>
<strong><i>Team Members</i></strong>: Leanne Novo, Beyonka Powell, Brian Quintero, Chuchu Wang 



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
