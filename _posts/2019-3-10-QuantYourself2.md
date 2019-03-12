---
layout: post
title: Starter Data Science Project The Quantified Self, part 2
permalink: /Quantyourself2/
published: true 
---

## Where we left off

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
things we can do with our dataset such as descriptive statistics and correlation tables. We'll also go over how to export these statistics to an excel file. 

## Descriptive Stats
Pandas has a number of built in method[^1]s that make drawing up stats extremely easy. In this first part of this tutorial series we read out information about our dataset that I would almost always check upon loading a new dataset. From both .info() and .head() I'd be checking to make sure that each column has the expected number of non-null rows and that python had interpretted numbers as numbers and words as strings. That's what the ```print(df.info())``` line is for. And for good measure I always like to actually read out a sub-section of the dataset so I can see what python is actually seeing, and that's the purpose of this line: ```print(df.head(5))```. These steps are there just to make sure our data exists as we expect it to exist, but don't necessarily help us extract insights from our data. 

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
Correlation Table
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

But looking at my table I noticed that I have only NaNs for my Active Calories column. NaN is python shorthand for not a number and the **.corr** method will put in NaNs where correlations cannot be made. The most plausible reason for this is that I didn't actually log any Active Calories, but QT Access had outputted it none the less since my smartphone was trying to track it. In the end my correlation table is simply cluttered by this column. Next I'll show you how to remove this column prior to exporting the table to excel. 

## Slicing Dataframes
Pandas comes with many built in features for manipulating data. This blog series isn't a exhaustive overview of pandas but 
I do want you to get a taste for what you can do with pandas. One routine task we can use pandas for is to **slice** a dataframe. Slicing involves selecting subsections of our dataframe. Slicing can happen either along the columns or rows in a dataframe. 
In our case we want to clear out both the column and row labeled 'Active Calories (kcal)'  

There are two ways of doing this that I'll show you for now. The first method involves using a label based approach. Pandas is aware that the columns and rows that makes up a given dataframe have names. In the output on your terminal the column and row labels appear on the top and left hand sides of the tables. 

### label based slicing ###

Pandas dataframes have a method called **.loc** that allows us to pick out what we want from our dataframe in a very straightforward manner. 
```python
df.loc[row_label, column_label]
```
What this would look like using our stats table as an example we would do something like this 

```python
descriptive_stats.loc['mean', 'Dietary Calories (cal)']
# output: 38089.76926462494
```
to get more that one column you would just have to supply the column_label parameter with a list of labels

```python
descriptive_stats.loc['mean', ['Dietary Calories (cal)', 'Steps (count)']]
```


The first parameter governs which rows pandas selects, while the second governs the columns returned. Note that just using
```python
df.loc[row_selector]
```
will simply grab a subset of rows and all the columns, but if you wanted to grab all the rows and just a subset of columns 
you would need to do something like this: 

```python
df.loc[:, 'Dietary Calories (cal)']
```
The : is a wild card character in pandas meaning that is shorthand for everything between, when used by itself it basically tells pandas to not bound the wild card which selects everything. 

Using the : between two labels tells pandas to return everything between two labels. So say we wanted to just select the 
dietary calories, distance, and steps columns  and just the row labeled 'mean' we could do something like this: 

```python
df.loc['mean', 'Dietary Calories':'Steps (count)']
```
One other use of the : wildcard is to bound it on one end and leave the other open

```python
df.loc[:, 'Active Calories (kcal)':]
```
This would select every column after the active calories column while 

```python
df.loc[:, :'Active Calories (kcal)']
```
would select everything up to the active calories column, but not that column itself

### integer based slicing ###
The other way we could do the same operations is to use numbers or more specifically integers to select pieces of our data
to do that we use **.iloc** instead of .loc 

Python is zero-indexed meaning that numbers start at 0, so to grab the first row of a dataframe one would use this line of code: 
```python
df.iloc[0]
```
Selecting the first 10 rows and second to fourth columns would look like this:

```python
df.iloc[:9, 1:3]
```

Taking what we just went over we can remove the offending row and column from our correlation table with this addition to our 
code:
```python

cleaned_corr_table = correlation_table.loc['Dietary Calories (cal)':, 'Dietary Calories (cal)':'Weight (lb)']
print('Cleaned Correlation table')
print(cleaned_corr_table)
```
With this addition we should get something like this as our output: 

```
Correlation Table
                        Active Calories (kcal)  Dietary Calories (cal)  Distance (mi)  Steps (count)  Weight (lb)
Active Calories (kcal)                     NaN                     NaN            NaN            NaN          NaN
Dietary Calories (cal)                     NaN                1.000000      -0.056050      -0.059322     0.331208
Distance (mi)                              NaN               -0.056050       1.000000       0.988516    -0.076882
Steps (count)                              NaN               -0.059322       0.988516       1.000000    -0.093157
Weight (lb)                                NaN                0.331208      -0.076882      -0.093157     1.000000
Cleaned Correlation table
                        Dietary Calories (cal)  Distance (mi)  Steps (count)  Weight (lb)
Dietary Calories (cal)                1.000000      -0.056050      -0.059322     0.331208
Distance (mi)                        -0.056050       1.000000       0.988516    -0.076882
Steps (count)                        -0.059322       0.988516       1.000000    -0.093157
Weight (lb)                           0.331208      -0.076882      -0.093157     1.000000
```

### Exporting to Csv ###
Finally lets go over how to export our correlation and stats tables to a csv file. 
With pandas this is about as straightforward as reading in our initial dataset. Every pandas dataframe contains a host of 
methods for exporting to different file formats: json, html, txt, excel, csv are some of the most handy but there are many more. 

For right now we'll just export to a csv file. Comma Seperated Values (CSV) can be easily read by excel and other programming languages so by default I typically export data in this format. 

At the very end of your python script add these lines: 

```python 
descriptive_stats.to_csv('descriptive stats.csv')
cleaned_corr_table.to_csv('Correlation table.csv')
```
The first parameters are the file names as they will appear in your local directory. 

## Up next: Jupyter notebooks 
While correlation tables are useful, they have limitations in a scientific sense. But from glancing at my data it's clear that my raw data can be cleaned up a bit beyond just slicing the dataframe up. In order to get some more meaningful insights into what I have going on here, I'm going to have to dig deeper intot he data. I'm not sure what your health data looks like, perhaps you're extremely fastidious and have kept a dutiful log of ever calorie you consumed. If not this next session will go over just a few data wrangling techniques that can make our correlation tables a bit more accurate as well as introduce some more methods and tools to draw insight out of the data. To do this we will leave running our python code from the terminal behind for a bit and start using a fantastic tool for anaylzing data with python: jupyter notebooks. 

[^1]: Methods are attributes of a class, a class is a blueprint to make an object. If you made a car, the mechanism that starts the car would be a method of the car object.  
