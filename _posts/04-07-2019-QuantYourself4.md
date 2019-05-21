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

[Xlwings]()
