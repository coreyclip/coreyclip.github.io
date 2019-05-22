---
layout: post
title: Starter Data Science Project The Quantified Self, part 4: Using Excel as a front-end
published: false
---

## Introducing Xlwings

In this final part of this tutorial series, we'll go over how to make a basic yet functional front end for what we've created so far in this tutorial series.
Typically when we refer to the *front-end* of our application we are referring to how a user interacts with the program we've written. A front end can take the form 
of a *graphical user interface* (GUI) which runs on a user's own machine, or more commonly these days on a mobile platform (iPhone, android) or on a web site as a web 
application. For data science projects the web app is usually the optimal way to distribute your work out to the wider world. If you're looking to bee line to that step I'd suggest looking into Flask for the web development aspect
and heroku for deployment. 

But particularly in business situations, excel is probably the most common way people interact with data and it's an uphill and not particularly necessary battle to get them to part from it. Excel is limited by the number of records it can reasonably process and by the functionalities programmed into it. What Excel has going for it, and what it excels at is user manipulation of data and representation. With the python library **xlwings** you can connect Python's libraries and speed with Excel's ready made user interface. 

[Xlwings](https://docs.xlwings.org/en/stable/) bridges python and excel with two features: user defined functions which operate in a similar manner as excel functions but are written in python and the VBA RunPython module which invokes a python function from a VBA macro. For this tutorial we'll create a simple yet effective analytics dashboard using xlwing's RunPython function and what we've learned so far in this tutorial series. To start install xlwings with pip 
```bash 
pip install xlwings
```
Beyond just installing xlwings there's a few additional steps that we need to take to get our environment up and running, so keep your terminal open. First let's install the excel plugin for xlwings. 
```bash
xlwings plugin install
```
if you encounter issues check [this page](https://docs.xlwings.org/en/stable/addin.html#xlwings-addin) of xlwings for troubleshooting the plugin installation. On [this page](https://docs.xlwings.org/en/stable/quickstart.html) the documentation suggests that we use another xlwings command to quickly spin up an xlwings project. Note the final argument of the command can be whatever you like, it just gives a name to your project, though I'd suggest making it sensible because unless you configure it otherwise later your scripts will need to conform to this naming convention.
```bash 
xlwings quickstart quant_yourself_excel
```

After running this command you should find a folder named 'quant_yourself_excel' or whatever you called your project in the directory that you ran the command in. In that folder there should be an excel file and a python file both named 'quant_yourself_excel'' open up the python file with your text editor.

```python 
import xlwings as xw

def hello_xlwings():
    wb = xw.Book.caller()
    # sheets can be called via an integer value 
    wb.sheets[0].range("A1").value = "Hello xlwings!"

@xw.func
def hello(name):
    return "hello {0}".format(name)


```

you should have see the above code already provided because we used `xlwings quickstart`. The first function is an example of the 
