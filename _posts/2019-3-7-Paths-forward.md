---
layout: post
title: Paths Forward
tags: beginner blog technical
---

## Getting started in the Programming World
There are a myriad of ways to get started programming and in general I don't really prescribe to any notion that there's one or even a few **right** ways to start out. What I think is important is that you start getting into a process of producing code and seeing the results. And I think the best way to do that is to learn to code via projects. I've heard from countless people that they tried learning to program but quickly dropped off because they spent hours and hours writing scripts that really didn't do anything. I don't think it's their fault but a lot of programming tutorials out there seem to be fixated on covering all the basics until they moving on to producing actionable artifacts. I'm sure for the more orderly left brain people that makes sense, but for me personally no amount of will power is sufficient to keep me doing something that doesn't have a clear goal to strive for. What follows are a few options for what I think are pretty reasonable paths that I know of to get you programming and more importantly creating something that either you or others will find useful. I am primarily interested and practice Data Science but like many newer programmers I do a fair amount of web development work. So what I'm going offer are my recommendations for what to cover when starting out in these two fields.  

## Data Science
If you like me are interested in machine learning and finding the signal in the noise as Nate Silver would put it or you work in an industry that involves any kind of analytics, I'd start exploring the field of data science. While Python has by and large come out ahead of R in terms of people programming in it for data science. I'd choose between Python or R on the basis of where you're currently at in life. If you're looking to make programming your new bread and butter then I'd learn Python first. Python is designed to be a general purpose programming language and operates in the data science space mostly due to extensions being created for it in the form of downloadable packages or libraries as they're called. But there are a multitude of very well made libaries in Python for everything from web development to visualization, making it very flexible in terms of the kinds of tasks that can be performed in it. As the joke goes to write an essay with Python one simply has to run `import essay`. R in my experience seems to be favored by academics and is purpose built for data analytics and stats in general. R's syntax and feel is very different from other programming languages because of it's more narrow focus, which results in it being a bit more friendly out of the box. I personally have only dabbled with R but did find it enjoyable and intuitive.

### getting started with Python
* Download the [Anaconda distribution](www.anaconda.org) which includes out of the box a particular version of python, a bunch of commonly used data science orient libraries, the conda command line tool, git, sypder a decent internal development environment, and best of all jupyter notebooks
* start exploring data with Jupyter: Jupyter notebooks are an extension of ipython notebooks which unlike traditional code scripts are composed of blocks of text called cells. These cells can either hold code or markdown text so your code and notes can live in the same document. Also the code cells will all share the same variables and objects. So you could load data from a csv file in one cell and experiment with it in another cells. If this secondary cell runs into any errors you don't need to rerun the original cell that loaded the csv file. You can rearrange the cells in any manner you'd like and more easily get output from code blocks than you would with a traditional script. The next generation of interfaces from the Jupyter project is Jupyter lab which combines elements of a IDE like spyder with Jupyter notebooks. This is the primary way I use jupyter notebooks now but for a beginner it may be easier to manage to just start with jupyter notebooks.

### getting started with R
* By and large R studio is the most popular way to use R, you can download it and read more about it [here](https://support.rstudio.com/hc/en-us/articles/201141096-Getting-Started-with-R)
* When you download Anaconda from the Anaconda Navigator you can download both R and R studio, Also Jupyter notebooks supports R as well as Python. In fact Jupyter gets its name from **Ju**lia **py**thon and **R** which are the three programming languages jupyter notebooks were designed to work with.

### Project
A Jupyter notebook in either R or Python that describes a story being told within a dataset, details some kind of A/B testing situation, or tests a hypothesis, essentially a research project. I'd advize focusing on a topic you know well and/or are interested or someone close to you in interested in. Personally this is how I started programming, by making jupyter notebooks that I used in presentations to the Peace Corps. Start by making some simple summary statistics and with markdown comments and then move to creating graphs in matplotlib and seaborn. 

## Web Development
Unless you end up in some specialized field, most of you will become web developers. Web development covers pretty much everything that you come across that's on the internet. This is likely the salient realm of programming today and what most people getting into programming today end up working in. Also if you end up specializing in something like data science or blockchain technology, there's still a lot of utility for learning about web development. Because how else are people going to interact with your shiny new predictive model or see your beautify charts, if not on the web. There are a multitude of tools and frameworks for web development out today. But basically the three biggest languages you'll want to get aquainted with is Html, css, and javascript.

## getting started with HTML & CSS and JavaScript

These three are often included together for good reason. If your website is a car then HTML is the frame and axles of the car. HTML isn’t a programming language its a blueprint for the structure of a webpage. The acronym stands for Hyper Text Markup Language and operates around a system of wrapping text in tags that delegates on a very high level how particular text is displayed on a webpage.

CSS stands for Cascading Style Sheets, and is a notation system for modifying how specific HTML tags or groups of HTML tags are displayed. CSS would modify the color of a car and modify the leather on the interior. CSS is what makes your website look sleek and modern. Without it you’re website will look decidedly web 1.0.

When JavaScript first entered the scene it was mainly used to create popup ads. Now it’s definitely up there with Python and Java as one of the three most popular and useful programming languages out there. But unlike the other two JavaScript is oriented around creating functionality within the web browser and not back-end logic. Keeping with our car analogy JavaScript handles the button, knobs, peddles, and steering wheel of the car. It’s the wiring that connects our front end interface with logic happening behind the scenes. A modern website would use JavaScript to connect a button click on a website to a Python script that may execute some kind of calculation for example. Or have a click on one object on the webpage change another object. When you see something change color when you mouse over it on a webpage it’s quite likely that Its worth noting that Javascript and Java are completely different animals and do very different things, don’t confuse the two.

In my opinion the best way to get started with web development in general would
be to head over to a decent CSS framework, I recommend Material Design
Bootstrap, and grab a template from one of these frameworks. And then just start
playing around with it, adjusting the class attributes in the css and creating
something you find visually attractive. W3 schools has a pretty good run down on
HTML, CSS, and JavaScript among other topics, and you can find their resources
here. JavaScript can be embedded within a HTML script tag at the bottom of a
HTML document and I would recommend just starting out with that setup to keep
things simple. While opinions do vary on this topic, I find it much easier to
work with JavaScript via some of it’s more prominent frameworks. The granddaddy
of them all is JQuery, not only is it rather straight forward, it’s syntax is
often used in other JavaScript libraries because it’s familiar to most
JavaScript programmers. Once you feel comfortable with JQuery check out some of
the newer frameworks like React, Angular or Vue.

While it doesn't allow for javascript since it's a static website generator. You could try working with github pages and it's supported web framework: Jekyll. I personally built this blog using the Jekyll-now project which you can learn more about [here](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/)  

