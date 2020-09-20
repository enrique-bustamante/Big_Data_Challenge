# Big_Data_Challenge

The data is available via the Google Colaboratory notebook named
[BigDataChallenge](https://colab.research.google.com/drive/1JdPW5cmjWKVwqJTM11Gh0FOxn6_u8M8K?usp=sharing). The schema used to create the SQL tables in PG Admin are
provided in this repository. I have also included the summary on this document.


## **Analysis Summary**

### Purpose
The dataset that was analysed was the watches dataset. The purpose of this experiment is to learn how to work with big data while using Amazon Web Services (AWS) and Spark via pyspark. We want to look specifically at the vine reviews and if a bias exists in this particular group of reviews.

### Techniques
#### Extract
First we had to extract the data as part of our Extract Transform Load (ETL) process. We decided on using Google Colaboratory as a notebook to conduct all of our operations. Colaboratory can be used in a variety of ways. Since we will be working with a data set from AWS, we also have to set up an environment utilizing Spark. This involves downloading Java, then Spark.

#### Transform
To transform the data, we decided on using python to interact with Spark by using Pyspark. There are four tables that we are going to build from the AWS watch reviews data: customers, products, review ids, and vines.

The data set needed to be cleaned up a bit. We started off with 960872 rows when we used count on the dataframe. This row count dropped to 960679 when we used dropna on the set. There were no duplicates so the number of rows remained at 960679 when we used drop_duplicates. The last clean up I decided to use was to use only the reviews that were verified purchases. Since Pyspark dataframes are different than panda dataframes, we also imported col from the pyspark.sql package. Using col along with filter, we reduced the number of rows to 831247.

All of the tables with the exception of customers were easily created by using select to choose the appropriate columns. To get the customer count, groupby was utilized along with count, to create the table with the customer ids and the count of reviews. We also had to sort the counts in a descending fashion and rename the count column to customer_count, for it to work with postgreSQL.

#### Load
The last part of the ETL process is the load phase. To connect with our PostgreSQL server, we utilized Java Database Connectivity (JDBC). To verify the data had ported into the tables properly, we ran "select * from <table> limit 100 in SQL Query in PG Admin.

#### Vine Analysis
We calculated the mean star rating for the set by first transforming the data into a Pandas dataframe, using .topandas(), and mean. We also created a subset by adding a filter to the vine table for all the reviews that had Vine. Unfortunately, this dataset only had one review that also had a yes for Vine. This makes the set too small to calculate statistical significance between the sample and population. We will cover how we planned to approach the data in Considerations

### Considerations
Had we more vine reviews available, we would've utilized a chi-squared test. The star ratings are technically ordinal data which is categorical. We would like to have a sample size of at least 30 reviews. Our null hypothesis would be that there is no difference between the average star rating between the vine reviews and all of the reviews combined. If we yielded a p-value lower than 0.05, we would reject the null and say that there indeed is a bias associated with the vine reviews. Otherwise, there is no significant difference.
