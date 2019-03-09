
# Quantify Yourself part 2: Open up a can of Statistics

In my previous post we started with downloading and installing the Anaconda python distribution and loaded our health data csv into a Python interpretter. I know that a lot of that installation work and perhaps figuring out pathing may have been frustrating. But like anything new we do, those intial challenges and frustrations are what build experience. 
Now that we have our data loaded it's time to start extracting insights. 

Open up the script from the previous part in your prefered text editor. 
When we finished last our script should look like this:

```python
import pandas as pd
import numpy as np

import os

df = pd.read_csv("health_data.csv")
print("information about your dataset")
print(df.info())
print("first 5 rows of the data")
print(df.head(5))

```

In this script the methods **.info()** and **.head()** just gave us some very bare bones but necessary information about our dataset. Next we'll go over some more sophisticated 
things we can do with our dataset such as descriptive statistics, correlation tables, and regression analysis. We'll also go over how to export these statistics to both a text file and an excel file. 

## Descriptive Stats
Pandas has a number of built in methods that make drawing up stats extremely easy. In this first part of this tutorial series we read out information about our dataset that I would almost always check upon loading a new dataset. From both .info() and .head() I'd be checking to make sure that each column has the expected number of non-null rows and that python had interpretted numbers as numbers and words as strings. That's what the ```print(df.info())``` line is for. And for good measure I always like to actually read out a sub-section of the dataset so I can see what python is actually seeing, and that's the purpose of this line: ```print(df.head(5))```. These steps are there just to make sure our data exists as we expect it to exist, but don't necessarily help us extract insights from our data. 

add the following lines to the bottom of your script:

```python
descriptive_stats = df.describe()
correlation_table = df.corr()

print("Descriptive Statistics")
print(descriptive_stats)

print('Correlation Table')
print(correlation_table)
```

In the above we are calling two new pandas.DataFrame methods and storing the results into variables that are again printed out into the console

When you run this script from the terminal, you should now have output that will look something like this:

```python
Descriptive Statistics
       Active Calories (kcal)  Dietary Calories (cal)  Distance (mi)  Steps (count)  Weight (lb)
count                   865.0            8.650000e+02     865.000000     865.000000   865.000000
mean                      0.0            3.808977e+04       3.217581    8109.345665     6.620182
std                       0.0            2.423989e+05       2.280001    5513.771349    36.219423
min                       0.0            0.000000e+00       0.000000       0.000000     0.000000
25%                       0.0            0.000000e+00       1.587784    4212.000000     0.000000
50%                       0.0            0.000000e+00       2.528945    6545.494470     0.000000
75%                       0.0            0.000000e+00       4.355165   10733.765974     0.000000
max                       0.0            3.237495e+06      14.516899   33195.107360   208.600000
                        Active Calories (kcal)  Dietary Calories (cal)     ...       Steps (count)  Weight (lb)
Active Calories (kcal)                     NaN                     NaN     ...                 NaN          NaN
Dietary Calories (cal)                     NaN                1.000000     ...           -0.059322     0.331208
Distance (mi)                              NaN               -0.056050     ...            0.988516    -0.076882
Steps (count)                              NaN               -0.059322     ...            1.000000    -0.093157
Weight (lb)                                NaN                0.331208     ...           -0.093157     1.000000

[5 rows x 5 columns]



```
So first let's go over what the .describe method outputed. 

The columns list out the columns we have in our dataset while the rows store stats for the associated column. 
* **count** simply the number of non-null values in a given column, my dataset is complete so each column has the same number of non-null rows: 510

* **mean** is just another word for the average, on average I have walked 3.73 miles in a given day.

* **std** short hand for standard deviation. In laymans terms a standard deviation is the range from which a value can be considered to be atypical in a probablistic context. The standard deviation for the distance I walked over this given timeframe is 2.5 miles, so a day in which I walked a signifcantly greater distance than normal would be 6.2 miles (2.5 + 3.7) while a day in which I walked significantly less would be 1.2 miles (3.7 - 2.5). For the most part the standard deviation is useful for giving context to your datapoints. 

* **25% , 50%, and 75%** these are all just shorthand for the 25th, 50th, and 75th percentiles. Percentiles points are representations of point within the data that a proportion of the values are bellow that given point. So for example 25% of the values in the distance column are 1.7 miles or less. The 50th percentile is the point where half of our data is above this point and half is bellow. This is also the definition of the *median*. Our 75th percentile is 5.23 miles so for three quarters of this given time frame I walked less that 5.23 miles but walked that amount or more for one quarter of this given timeframe. 

* **max** this is just shorthand for the maximum.  

You can see the documenation for .describe() [here](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.describe.html)

After our stats output we have our correlations table. Correlation tables are IMO an essential part of data exploration. I almost always use one as a guide for whatever next steps I take in performing data analysis and when properly visualized they can be great analytical products in their own right.

A correlation table is typically presented as a matrix with identical labels on both the columns and rows. The numbers that make up the matrix are the correlation values between the corresponding row and column labels. Correlation essentially is the degree in which two phenomena appear to act in concert. If a correlation is a positive number then we can say that when **A** increases **B** also typically increases. With my dataset Step Counts and Distance in miles are almost perfectly correlated, which makes sense. We can also see that my weight is correlated positively with Dietary Calories, though to a weaker degree. 

A negative correlation means that as **A** goes up **B** goes down and vice versa. With my dataset, weight is negatively correlated with steps and miles traveled. And it appears that my step count is slightly more negatively correlated with my weight than miles walked, I guess short strides are better than long strides in a very granular sense.

But looking at my table I noticed that I have only NaNs for my Active Calories column. NaN is python shorthand for not a number and the **.corr** method will put in NaNs where correlations cannot be made. The most plausible reason for this is that I didn't actually log any Active Calories, but QT Access had outputted it none the less since my smartphone was trying to track it. In the end my correlation table is simply cluttered by this column. Also looking at my 


## Cleaning up the slop
While correlation tables are useful, they have limitations in a scientific sense. But from glancing at my data it's clear that my raw data can be cleaned up a bit to get some more meaningful insights into what I have going on here. I'm not sure what your health data looks like, perhaps you're extremely fastidious and have kept a dutiful log of ever calorie you consumed. If not this next session will go over just a few data wrangling techniques that can make our correlation tables a bit more accurate.
