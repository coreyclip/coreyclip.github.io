---
layout: post
title: Creating a basic ETL Framework with Pandas and SqlAlchemy
published: false
---

## From Excel to Database
In this post we'll do a hands on walk through of processing the data from a typical excel file into a format for our a MySQL database to consume. A lot of data lives in excel but often that is neither a scalable or particularly convenient permanent home for our data. 

One of the biggest steps towards moving an process from excel to a more modern tech stack involves transferring our data from disparate sheets and formats into standardized SQL tables. This is one of many ETL tasks one can find themselves in. From my own experience ETL tasks are nearly ubiquitous in data world. You'll commonly hear that the joke is that data science is about 85% gathering and cleaning data while the really cool stuff like Machine Learning and visualization takes up only 15% of the work. It's a joke because it's really more like 90% percent of the effort. Especially if you're working solo or in a traditionally non-technical environment, ETL is going to be very much a part of your projects.  ETL refers to the Extract, Transform, Load and sums up the three steps we'll need to perform to get our data in an accessible and easy to work with form for other things we want to do such as ML, data visualization, or other kinds of analysis. Here's a basic rundown on what each of these steps mean. 

* Extract: Move the data from it's raw form and into python.
* Transform: Manipulate our data into a format that we need it to be in.
* Load: Moving our data from python and into a more useful format for later.

As you can imagine this can encompass much more than just moving from excel to SQL. This can involve moving text files to json for a web visualization or moving data from a database, running some transformations and outputting back into excel. Either way I think it's worthwhile to think about such tasks in terms of three distinct processes and focus on allowing for variances between how each of these steps take place while clearly defining the differences in concern between them. 

If you're new to object oriented programming an ETL pipeline is a good first use case for creating your first objects. One of the main advantages of using object oriented programming is that it promotes *separation of concerns* and *code resusability*. When I was starting out, how this all works out was kinda confusing. Hopefully by the end of this post, we'll see how using python classes fits into OOP (object oriented programming) and code reuse and separation of concern. Our ETL pipeline should be laid out where the extraction is one concern, transformation another, and finally load. We'll be using class inheritance to not only link all of the ETL steps together but also allow for methods we create in one class to be shared by other classes. 

## Setup 

