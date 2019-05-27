---
layout: post
title: Starter Data Science Project The Quantified Self, part 4: Using Excel as a front-end
published: false
---

## Introducing Xlwings

In this final part of this tutorial series, we'll go over how to make a basic yet functional front end for what we've created so far in this tutorial series.
Typically when we refer to the *front-end* of our application we are referring to how a user interacts with the program we've written. A front end can take the form 
of a *graphical user interface* (GUI) which runs on a user's own machine, or more commonly these days on a mobile platform (iPhone, android** or on a web site as a web 
application. For data science projects the web app is usually the optimal way to distribute your work out to the wider world. If you're looking to bee line to that step I'd suggest looking into Flask for the web development aspect
and heroku for deployment. 

**Note this tutorial is only for windows, I haven't tested any of it on OS X and excel isn't supported on Linux as far as I know***

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

you should have see the above code already provided because we used `xlwings quickstart`. The first function is an example of a function activated by the VBA Module RunPython while the second function is an example of a user defined function. To start understanding what's going on under the hood, open up the excel file as well. 

First verify that the bellow addition to your command ribbon is available, this will indicate that the plugin was installed properly. 

![xlwings ribbon](https://docs.xlwings.org/en/stable/_images/ribbon.png)

Next open up the VBA editor from excel. Either from the Developer tab, find the Code panel, and click the Visual Basic button. On the Controls panel of the Developer tab, click View Code. If it’s not visible, click “File,” “Options” and then “Customize Ribbon.” Click the “Developer” check box within the Main Tabs list and click the "OK" button when you’re finished. You can also just use the keyboard shortcut ALT + F11 and on a mac the shortcut is Fn + ⌥ + F11

If you have other excel files open you may have to navigate around to find the pre-made xlwings macro but it most likely will be in sheet1 of this file. You should see a macro already filled out for you with the code bellow
```vba

Sub hello_xlwings()
        
    mymodule = Left(ThisWorkbook.name, (InStrRev(ThisWorkbook.name, ".", -1, vbTextCompare) - 1))
    
    RunPython ("import " & mymodule & "; " & mymodule & ".hello_xlwings(" & sFile & ")")

End Sub

```
We don't have to worry too much about the specifics of this VBA macro, basically it's already hard coded to find and then run our hello_xlwings() function in our python file. You can try it out by activating the hello_xlwings macro. Go back to your excel file and add a button to your excel sheet. To do so go back to the "developer" tab in the Controls group, click Insert, and then under Form Controls, click Button. Click on a location in the sheet where you want the button to appear. The Assign Macro popup window should appear. If it doesn't, right click on the button and select "assing macro" from the drop down menu. Select hello_xlwings, and click "ok". Now when you click on that button you should see "hello xlwings" in cell A1. 

Go back to your python script and lets go line by line to see what happened. 

```python
wb = xw.Book.caller()
```
This tells xlwings to basically import the excel workbook that called the function, as an object into our python environment. You can think of the process as being similar to how we import our csv files into python with pandas. Both create an **object** that then gets stored in a variable. You can also tell xlwings which file to interact with directly by simply supplying it a file path like this. 

```python
wb = xw.Book('quant_yourself_excel.xlsm')
```

The next line gives us a basic idea of how xlwings will serve as our front end. 

```python
wb.sheets[0].range("A1").value = "Hello xlwings!"
```

The code above tells xlwings to select the first sheet in the workbook through an integer system, with the `.sheets` method. If you'd rather use the sheet name as it appears in the bottom tabs of your excel file you can supply those too as a string. After that the `.range('A1')` method call specifies an excel range using the same conventions that you'd use in excel. Finally to assign a value to an excel range you have to declare the `.value` attribute of our `range`, and this value is "Hello xlwings!". 

## Importing our dataset
The normal methodology of using pd.read_csv would work just fine in this situation but I wanted to also include a bit of VBA that will allow users to open up a file with the standard GUI and have that be passed to our xlwings function. 

Go back to your macro in excel and add the following code
TODO verify VBA code
```vba

Sub hello_xlwings()
    'FILE-SIDE PREPARATION
    'Returns file path string or boolean false
    vFile = Application.GetOpenFilename(FileFilter:="Excel Workbooks (*.XLSX), *.XLSX", Title:="Select File To Be Opened")
    
    'Error handling; easier to handle out of getWorkbook
    If vFile = False Then
        MsgBox ("Analysis Cancelled.")
        Exit Sub
    Else
        'replace / with // to avoid unicode decoding errors
        File = "'" & Replace(CStr(vFile), "\", "//") & "'"
        
    End If
    
    mymodule = Left(ThisWorkbook.name, (InStrRev(ThisWorkbook.name, ".", -1, vbTextCompare) - 1))
    
    RunPython ("import " & mymodule & "; " & mymodule & ".main(" & File & ")")

End Sub
```

This will prompt excel to allow the user to open up the file explorer and select a file. Its path is then cleaned to fit windows pathing conditions and then finally passed into our python module as a function parameter using vba's string concatenation methods. I wanted to include this additional step, because I found it to be extremely handy when it came to passing xlwings projects off to non technical users. You as a programmer will have to become extremely familiar with file paths but these are often very frustrating to write out for many and the only alternative would be to restrict users to keeping their input data in one file location.

Next open up your python file and create a new function called `main` with the following code.

```python
def main(file):
    wb = xw.Book.caller()
    front_page = wb.sheets[0]
    df = pd.read_csv(file, parse_dates=['Start'])
    df = df.replace(np.int(0), np.nan)
```

## Basic Output
The above will initialize xlwings connection to the workbook, pass the first excel sheet to a variable called `front_page`, read our csv file into pandas, and replace the zeros with Nan values. With that we should be able to do our first test run. To make things more interesting add some code to process the pandas dataframe and send it to the front page. Bellow is only a suggestion, basically any pandas dataframe object should work with this formula. 

```python
front_page.range('A8').value = "Statistics"
front_page.range('A10').value = df.describe().T

front_page.range('A22').value = 'Correlation Table'
front_page.range('A23').value = df.corr()
```

When you click on the button we set up earlier you should be asked to open up a file, open up your health data csv and if everything is set up properly you should see the descriptive stats and correlation table outputted to your excel sheet. Note that xlwings is smart enough to expand the output range to properly contain the dimensions of a pandas dataframe or restrict the range when passed just a single cell's worth of information.

## User options
