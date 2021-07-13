---
layout: post
title: Starter Data Science Project The Quantified Self 
tags: beginner python excel tutorial
---

## Getting started with Python
For this series I'm going to approach introducing the tools and techniques of Data Science, in a manner I've so far not seen online yet.
Most introductions to Data Science revolve around datasets that are already well known and with the foreknowledge of what can and cannot be extracted from it. A certain 20Th century tragedy and various flower specimens come to mind. While there's not exactly wrong with learning data science through these kinds of examples, I think it removes the element of data science I personally love the most: discovery!

I personally first learned Python in order to explore complex sets of data I encountered during my Peace Corps tour back in 2014. Being able to transform what I was learning through blogs, online courses, and books into actionable insights. Helped create a virtuous cycle of seeing a technique and immediately applying it to a personal project that grew in scope on a almost daily basis.

It's my aim to try to give some guidance on how to recreate that same experience with this blog series. While I'd encourage anyone interested in getting into Data Science to start with data that they are familiar with and care enough about to what to analyze. But for many it's hard to think of something. What I'll be laying out is a data source that is

- definitely unique
- relevant and actionable

We'll be using health data collected from our own iPhones or wearables as a guide towards learning python for data analysis all the while
steadily building up our own understanding of ourselves and our habits. Hopefully you'll end up with a enough python knowledge to be dangerous, and have an experience that will serve as a framework when working with data in the future.

you can download the code samples [this github repo](https://github.com/coreyclip/Quant_yourself)

## Gathering our data

[QS Access](https://itunes.apple.com/us/app/qs-access/id920297614?mt=8) is a simple iphone app that accesses the health app in your iphone. You'll want to allow the health app to have read access to all of the other apps on your phone tracking your calories, exercise, meditation minutes, etc. because QS Access only works with the health app directly. If you currently aren't using your phone to track your personal health, your phone likely already has some information QS Access can pull, namely your sleep hours and distance walked/step data. I for example mostly just have my step data and sleep data, I have some records of calories from way back, and otherwise just have the past couple of months of me weighing myself semi-regularly. I would recommend first downloading your data as days since that will include the most metrics coming from your devices. From there you can upload this csv file to your own personal computer either via dropbox, icloud, google drive or by email. Now that our data has been captured, we can transform it into something comprehensible.

## Starting out with Python

If you're brand new to Python, I would highly recommend downloading and installing the [Anaconda](https://www.anaconda.com/download/) distribution. There's a part in the installation process that asks weather or not you want to put Anaconda on your PATH variable. If this is your first time using Python, I'd recommend putting checking that box and putting Python on your PATH. I know the installer says this isn't recommended but really unless your managing multiple python interpreters (if you don't know what that means your probably not). Then putting Anaconda on your PATH will make things smoother moving forward. By downloading Anaconda, we'll have pretty much all the tools we'll need to make it through the series plus at the time of writing this post Anaconda comes with the option of downloading VS Code, a great text editor for writing code, and the Jupyter project which is ubiquitous tool in the Data Science world, as well as being a rather beginner friendly environment to learn python in.

# Beyond Helloworld: Loading a csv file

Place your csv file in a dedicated folder for example in a folder called _Quant_yourself_ on your C: drive or Home directory if you're using mac. If your using Linux then you know what your doing.

```bash
C:\Quant_yourself\health_data.csv
```

or if your on a unix based system like linux or mac

```bash
/home/Quant_yourself/health_data.csv
```

We'll be using this Quant_yourself folder as our project directory. If I say put x y or z file in your project directory this is the folder I'm talking about.
Next we'll get started with our python script that'll extract, load, and transfor our dataset.
While many beginner python tutorials shy away from using external packages, I say if they make life easier let them make life easier.If you installed anaconda then the following should work without issue.
If you didn't for some reason then go to the command line and use pip to install the following to install pandas, a very widely used and robust python package for data analysis and processing.

```bash
pip install pandas
```

Once that's done or you already have anaconda installed (which includes pandas)
create a file called **main.py**
[^1]

## Extracting the Data

Ok so in our main.py script start the script with the following:

```python
import pandas as pd
import numpy as np

import os

df = pd.read_csv("health_data.csv")

```

The first line of python code simply tells python to import in the code for pandas
and then tells python to use the alias pd to refer to pandas.

Generally people use the alias, pd for pandas so if you happen across some python code
where you see **pd** you can mostly assume it's referring to pandas.

The same goes for the next line which imports the python package **numpy** or numerical python.

note: if you installed pandas you should have also gotten numpy since it's a dependency for pandas

The fourth line is where we encounter something interesting. This line will load
our csv file named _health_data.csv_ and store it in a variable called **df**
which is the convention for people working in pandas to call variables that hold a
pandas **dataframe** which you can think of as being analgous to a excel spreadsheet.
"health_data.csv" is as you may see the name of the csv file saved in our folder, if you're csv file of health data is named something else, change that part to what you have on your machine.

## Reading out information about our dataset

Next add these lines to the script

```python
print("information about your dataset")
print(df.info())
print("first 5 rows of the data")
print(df.head(5))

```

These next few lines will output general information about the dataframe and list out the first five lines of the dataset.
The **print** statments that each command is wrapped it tells python to read out whatever is placed between the parenthesis to into the terminal which we'll touch on shortly. Generally python functions are invoked pretty much in the same way as you would say an excel function:

```python
function(parameter, parameter,...)
```

The first print function will simply read out the statement "information about your dataset" to the terminal. Anything written between single quotes or double quotes will be interpretted as a string by python. A _string_ is simply text that isn't meant to be interpretted as code, pretty much all programming languages support string datatypes.

The next print statement doesn't contain a string but instead calls a method from our **df** object. A method tells python to activate a particular action from an object. methods in python are accessed with a period or what is sometimes refered to as dot notation.

```python
object.method()
```

In this case and in the **df.head()** method we are telling python to access the **.info()** and **.head()** methods of a pandas dataframe. .info() will provide a simple read out of what pandas is interpretting as our dataframe. It's a good idea to always call this method and check it's read out when performing data analysis to make sure that the computer is seeing things the same way we are. .head serves a similar function, as it prints out the first **n** rows of a dataframe, where **n** is the number placed within the method's parameters. In this example that would be 5. Something useful to keep in my when programming is that while the computer is very obedient and fast, it's not as clever as we may often hope. Pandas is pretty robust but you should expect to encounter some issues eventually especially when say tweaking the pd.read_csv command to only access specific parts of the original csv file.

## Running our first python script

At this point it's worthwhile to give our python script a test to run to see that we've set everything up correctly.
This means that you'll have to access the command line. On mac or linux that's going to be the terminal program. On windows I'd highly recommend installing git and using the git bash terminal, which will come included when you install git. If for some reason you cannot install git then use Windows powershell, not the cmd.exe unless you know what you're doing.

Once you have the command line open you'll need to navigate to the folder where you have your csv file and .py file. The conventional way to do this is to use the **cd** command which stands for "change directory" in unix based systems. Macs are unix based while the git bash terminal on windows mimics one. Powershell contains some but not all unix commands luckily it does have the cd command

```bash
cd Quant_yourself
```

this assumes that you placed your Quant_yourself folder in either your home directory on mac or C: drive on windows. If you put it someplace else say within a folder in your home directory, then you'll have to prefix the file path like this:

```bash
cd projects/Quant_yourself
```

Once you've navigated to your project directory, you should see something like this in your terminal

```bash
YourComputer:~/Quant_yourself$
```

The specifics will be different between operating systems and terminal programs but generally you should see the Quant_yourself text to the left of your cursor.

Next you'll call python and have it run your first script. Type in the following command:

```bash
YourComputer:~/Quant_yourself$ python main.py
```

press enter and you should see the following being read out

```bash
yourcomputer:Quant_yourself$ python main.py
information on your dataset
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 865 entries, 0 to 864
Data columns (total 7 columns):
Start                     865 non-null object
Finish                    865 non-null object
Active Calories (kcal)    865 non-null float64
Dietary Calories (cal)    865 non-null float64
Distance (mi)             865 non-null float64
Steps (count)             865 non-null float64
Weight (lb)               865 non-null float64
dtypes: float64(5), object(2)
memory usage: 47.4+ KB
first 5 rows of the data
               Start             Finish     ...       Steps (count)  Weight (lb)
0  15-Aug-2016 00:00  16-Aug-2016 00:00     ...             25305.0          0.0
1  16-Aug-2016 00:00  17-Aug-2016 00:00     ...             12475.0          0.0
2  17-Aug-2016 00:00  18-Aug-2016 00:00     ...             14898.0          0.0
3  18-Aug-2016 00:00  19-Aug-2016 00:00     ...              6656.0          0.0
4  19-Aug-2016 00:00  20-Aug-2016 00:00     ...              6886.0          0.0

```

If you got something similar to the above, then congradulations you just ran your first python script!!

This may not be much but it's a start. In this next section we'll start to move beyond what we frankly could have just done in excel.
