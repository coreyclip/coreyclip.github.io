---
layout: post
title: Processing User Input, with Flask-WTForms
---

## Adding user comments to your website

In this post I'll go over how to expand a basic html serving flask website to a more dynamic website including content that you the web developer has put in and content added by users of the site. While creating comments is the clearest use case for these techniques. The broad strokes apply to most cases when you want information from your front end to communicate with your backend. 

To start out consider this basic flask website structure 

```
.
├── app
│   ├── __init__.py
│   ├── routes.py
│   ├── static
│   └── templates
│       └── index.html
├── config.py
├── MyApp.py

```
To those of you have perhaps only worked with flask from a single script, the above structure is adopted from [miguel grinberg's flask mega tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world) 

I'd highly recommend giving the entire series a read as he does a fantastic job of going through all the steps of creating a web application with Flask. 

## Introducing WTForms 

WTForms is a python library for handling web form input and validation. WTForms lets you define your web forms as python objects which you can then render to your front end. The python library Flask-WTForms simplifies interactions between Flask and WTForms. To get started use pip to install Flask-WTForms into your virtual environment (surely you know that you should have one of those set up). Note doing this will also install WTForms so you don't have to worry about install it as well. 

```bash
pip install Flask-WTF
```

## Coding your forms
Now make a new python file called forms.py in your app directory 



```
.
├── app
│   ├── __init__.py
│   ├── forms.py <---right here
│   ├── routes.py
│   ├── stati
│   └── templates
│       └── index.html
├── config.py
├── MyApp.py

```

And input the following imports  

```python3
from flask_wtf import FlaskForm
from wtforms import SubmitField, TextField
from wtforms.validators import DataRequired
from wtforms.widgets import TextArea
```

First we need the FlaskForm class from flask_wtf this will serve as the *parent* of our form object we'll be creating. 

From the wtforms library we'll also want to pick out he kinds of form fields that we'll want to use. Refer to [WTForms Docs](https://wtforms.readthedocs.io/en/stable/crash_course.html) for a complete list

We'll also want to import in validators from wtforms. Validators ensure that the user uses the forms properly. DataRequired is an attribute that we can add to our form fields to force the users to fill out a specific part of the form. 

Finally we are going to import in TextArea from wtforms.widgets, this will let us create a nice text box for our users to use. 


Now let's create our actual form 


```python3
from flask_wtf import FlaskForm
from wtforms import SubmitField, TextField
from wtforms.validators import DataRequired
from wtforms.widgets import TextArea

class CommentForm(FlaskForm):
    comment = TextField('Comment', widget=TextArea(),validators=[DataRequired()])
    submit = SubmitField('Submit')

```

Our CommentForm is defined as a python class that inherits the FlaskForm class. If you're unfamiliar with object inheritance. Essentially our new CommentForm contains all of the functionality and features of FlaskForm along with the comment and submit attributes we defined as well with TextField and SubmitField. 


For both TextField and SubmitField the first argument is the label that we can use to show our users what the form field is for. TextField is supplied additional arguments for a widget and validators. Note that because you can have multiple validators for a single field the argument supplied to it is a list. 

With that settled move to your routes.py script. 

## Handling forms within a route 

Bellow I have a simple home route for flask located in app/routes.py

```python 

from flask import render_template
from app import app

@app.route('/', methods=['GET'])
def home():
    return render_template('index.html')
```

First we need to import our new CommentForm object and adjust our route to accept POST requests. By default a flask route only accepts GET requests meaning the a request to a specific route can get information from the server. With POST enabled a user can submit information to the server. In our case this means the user can fill out our form and submit it to be read by our application. 


```python 

from flask import render_template
from app import app
from app.forms import CommentForm

@app.route('/', methods=['GET', 'POST'])
def home():
    return render_template('index.html') 

```

If your a bit confused about why I'm importing the forms from app when routes.py is in the same directory as forms.py, note that our application is in fact running above this directory from MyApp.py so it needs to be directed to the directory app none the less. 

## Reading and writing comments 

In a more fleshed out app in production you'd want to store information you're getting from users with a database but for development it's perfectly fine to use json or csv files to act as placeholders for the time being. Ultimately I think it's best to define your app and it's functionality first and then design your database solution around your app rather than design around your database structure. 

Create a comments.json file in your static directory and a templates directory if you haven't 
```bash
$ mkdir app/static
$ mkdir app/templates
$ touch app/templates/index.html
$ touch app/static/index.html
```

In comments.json you can just put an empty json literally just a pair of curly brackets i.e 
```json
{}
```

Back in your route open that json file and then add in the code for handling user POST requests from the form. 

```python

from flask import render_template, url_for
from app import app
from app.forms import CommentForm
import json
from datetime import datetime as dt

@app.route('/', methods=['GET', 'POST'])
def home():
    with open("app/static/comments.json", 'r') as file:
        comments = json.load(file)

    form = CommentForm()
    if form.validate_on_submit():
        comment = form.comment.data.strip()
        timestamp = dt.now().strftime('%c')
        comments[timestamp] = comment
        with open('app/static/comments.json' , 'w') as outfile:
            json.dump(comments, outfile)

    return render_template('index.html', form=form, comments=comments) 
```

So after you open up the json file next you create an instance of the CommentForm object we made in forms.py. From there we use an if statement on a method in our form object called: validate_on_submit this is a method CommentForm inherits from the FlaskForm class that we imported from Flask-WTF. This method validates our form by calling all of the validators in our form fields. In our case DataRequired for our comment field back in forms.py. This method also checks to see if the form was submitted, which also handles weather or not we are receiving a POST request rather than a GET request. 

Bellow that conditional the script unpacks the data from the comment field: *form.comment.data* .strip(** simply removes any white space from the resulting string. Then we create a timestamp from the datetime module and use it as a key for our comment and store it into our comments dictionary we read previously. Before we render the html we store the comment we just read back into our json file. 


## Templating with Jinja
If you're unfamiliar with jinja templates and generally how flask handles templating for now just note that render_template **can take any number of arguments** after you feed it the html file you're rendering. These arguments can be named anything and you can pass strings, data structures like lists or dictionaries, as well as full blown objects to render_template for use in templating and I'll show you how to do that next 


Open up your landing page at app/templates/index.html 

Add a form tag for our comment form followed by an unordered list tag for rendering our comments

```html
<h1>Make a Comment about Waffles</h1>
<form></form>
<ul></ul> 
```

This will serve as the structure of our front end. In order to get the form to correctly point back to our back end we need
to give it a route and method for interacting with that route, this is done with the action and method arguments respectively 


```html
<h1>Make a Comment about Waffles</h1>
<form action='/' method='post'></form>
<ul></ul> 
```

Now we'll start using jinja to work with the elements we passed to our template back in routes.py 
Jinja is itself it's own language, so while it has some pythonic flair it is it's own distinct tool. To render an object passed via render_template we need to use a pair of curly brackets like this 

```jinja 
{{ my_variable }}
```

This translates to the following in order to use our WTForms object we created back in forms.py 

```jinja
<h1>Make a Comment about Waffles</h1>
<form action="/" method="post" novalidate>
    {{ form.hidden_tag() }}
    <p>
        {{ form.comment(_class='myclass') }}

    </p>
            <p>{{ form.submit() }}</p>
</form>
```

The first curly bracket accesses the hidden_tag method of the WTForms form. This is a built in security feature of WTForms that our CommentForm class inherited from FlaskForm object. You should always include this in any form you render to keep your site and users safe from malicious actors. Next we access the Comment attribute, while we aren't going to actually implementing any css I did want to show you how to add a css class to WTForms form. Note that you have to put an underscore prior to the class argument. And finally there's the submit attribute which will render a button to submit the form your users fill out. 

Next We'll render our actual comments. Note that jinja's loop syntax is similar to python's, you can iterate over the key value pairs in the comments dict much in the same manner as you would with python but instead of double curly brackets your jinja code is wrapped in single curly brackets and a percentage sign also you need to explicitly end the for loop with an end for statement. An example is bellow:

```html
            <ul>
                {% for timestamp, comment in comments.items() %}
                <li>Date: {{ timestamp }}</li>
                <li>Comment: {{ comment }}</li>
                {% endfor %}
            </ul>
```


With that all set up you should have a working site with comments enabled. In a production setting, you'd definitely want to switch away from json files and instead use a database like Mysql, POSTGRE SQL, or Mongo DB. But for the flask logic and jinja templating the overall system remains the same. 

