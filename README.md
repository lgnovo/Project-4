# Project-4
# Team Credit Crunchers
<a href='#overview'>Overview</a></br>
<a href='#background-information'>Background Information</a></br>
<a href='#presentation'>Presentation</a><br/>
<a href='#how-to-run'>How to Run</a><br/>




## Overview
<strong><i>Overview</i></strong>: 
<br/><br/>
<img src="https://github.com/mshawn12/video-game-sales-analysis/blob/main/images/video_game_header.png?raw=true">
<br/><br/>
<strong><i>Team Members</i></strong>: Leanne Novo, Beyonka Powell, Brian Quintero, Chuchu Wang 



## Background Information
The team will leverage a Global Video Game Sales & Ratings dataset from <a href="https://www.kaggle.com/datasets/thedevastator/global-video-game-sales-ratings">Kaggle</a> in order to test various hypotheses about video game genres as well as develop a video game recommendation engine.

## Presentation
- View Analysis presentation <a href="https://github.com/mshawn12/video-game-sales-analysis/blob/mydashboard/resources/group1_video_game_analysis.pdf">here</a>


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