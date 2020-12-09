---
layout: post
title: Quote Me Dev Diary Part 1
---
## Quote Me: The Quoting Moody Therapist of Your Dreams
Hello everyone this is the first chapter of the development diary for a meandering app idea I have and wanted to work on more as fun side project rather than anything that could possibly ever make me money. I'm a huge fan of quotes, like the kinds you see slapped on motivational posters. Mantras, sayings, proverbs etc. I tend to journal irregularly but what I do keep close to me are both digital and physical collections of quotes I've collected over the years. I unironically can spend hours scrolling through wikiquote.org looking for new ones to add to my collection. Recently I thought to myself that it would be cool if I could speak to my computer about my current mood or thoughts and have it respond to me with a relevant quote. Pondering this I thought it would be a good application to practice my existing skillset and expand into new ones. 

## Plan
Thinking about it I want the app to suprise me. No preloading my own selection of quotes, I'm too lazy for that but silly enough to think I can automate the collection process. So first I'm going to try to either scrape or parse some body of text with quotes in it. Next I want the application to be able to decipher a user's feelings and intentions and respond accordingly. While there are a lot of advanced NLP methods that could be utilized for this part. I think as a start I'll try out sentiment analysis on both the scraped quotes and user input. So that say if a user inputs something happy and upbeat the app can return a quote that is similar in tone. Being on topic can be an improvement later on. Finally I want to actually create a front end that is nice to look at and interactive. Maybe a I can create a nifty wiseman sprite that will give out the responses. 

Here's a bad graph of my initial plans
![Application Diagram](/images/quote-me_application_diagram.odg)

In the next couple of posts I'll start documenting my planning, implimentation, and evolution of this application. 
First I'll setup the web scraper and store quotes in a database, probably as a first pass we'll just use csv files but later move to a proper sql database. From there I'll set up the sentiment analyzer that will go through the quotes and add sentiment scores. After that we'll get a basic web UI set up with Flask and probably a basic form for now. After that we'll start iterating to improve each piece of the application. 
