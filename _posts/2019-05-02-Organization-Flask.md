---
layout: post
title: Organizing our flask app - [ 2 ]
feature-img: "assets/img/pexels/organizing.jpeg"
thumbnail: "assets/img/pexels/organizing.jpeg"
image: "assets/img/pexels/organizing.jpeg"
tags: [Flask, Scratch, Two, Backend, Organization]
author-id: lehbyte
---

This is part 2 of building a flask app.  If you haven’t done so I suggest you go take a look at the first tutorial at the bottom. 

## Getting started

Navigate to our project and activate the virtual environment;
` $ source ../fenv/bin/activate`

If you remember we wrote our flask app in one script. That code looked like this;
**app.py**

```python
#!../fenv/bin/python
from flask import Flask

Raj = Flask(__name__)

@Raj.route("/", methods=["GET"])
def index():
    return ("Welcome to Raj's flask application.")

Raj.run("localhost", port=8000, debug=True)
```

Lets try to separate things here.

First off lets put our view in it’s own file which we will build on later on. We shall name it views.py

This is what views.py should look like;

```python
@Raj.route("/", methods=["GET"])
@Raj.route("/index", methods=["GET"])
def index():
    return ("Welcome to Raj's flask application.")
```

Lets create a folder for views.py  it’s also where we will put our application `templates` and `static` files.


```python
$ (fenv) mkdir app
$ (fenv) mv views.py app/
```

Since we’ve moved our views into another folder, we have to import so that we can route requests;
**views.py;**

```python
from app import Raj
@Raj.route("/", methods=["GET"])
@Raj.route("/index", methods=["GET"])
def index():
    return ("Welcome to Raj's fl
```

So now our `app.py` should look like so;

```python
#!../fenv/bin/python
from flask import Flask
Raj = Flask(__name__) 
Raj.run("localhost", port=8000, debug=True)
```

## Entry point
Lets create an entry point for our app by creating `app/​__init__.py`
`$ (fenv) vi app/__init__.py`
Cut the following code and add it to `app/​__init__.py`

```python
from flask import Flask
Raj = Flask(__name__)
from app import views
```

Now our original `app.py` looks like so;

```python
#!../fenv/bin/python
Raj.run("localhost", port=8000, debug=True)
```

And our directory structure is thus;

```bash
fenv/
myapp/
----app/
--------__init__.py
--------views.py
----app.py
----tenv/
```

## Further breakdown
Now all app.py is doing is just running our app – Raj.
But in order to run the app it has to know where the app is so specify it (`app.py`);

```python
#!../fenv/bin/python
from app import Raj
Raj.run("localhost", port=8000, debug=True)
```

Since all  **app.py** does now is run our application, it’s better to rename it to something like; 

`$ (fenv) mv app.py run.py`

You can also get rid of `tenv/` if you’re not using it; $ (fenv) rm -rf tenv
And our directory structure looks like this;

```bash
fenv/
myapp/
----app/
--------__init__.py
--------views.py
----run.py
----tenv/
```

## Recap

Your `app/__init__.py`

```python
from flask import Flask
Raj = Flask(__name__)
from app import views

```

Your `​app/views.py`
```python
from app import Raj
@Raj.route("/", methods=["GET"])
@Raj.route("/index", methods=["GET"])
def index():
    return ("Welcome to Raj's flask application")

```

And your `run.py`

```python
#!../fenv/bin/python
from app import Raj
    if __name__=='__main__':
Raj.run("localhost", port=8000, debug=True)
```
Go a head and run the app to make sure that it still runs as expected.

## Jinja2 templates

Currently our view is just a string rendered as HTML.
To add HTML files to our app we have to create a directory called `templates` just for HTML files
If we want to also include `css stylesheets` and `JavaScript` files we would also create another asset `static` that would contain these files.

```bash
$ (fenv) mkdir app/templates
$ (fenv) mkdir app/static
```

Go a head and create a new `HTML`file in the templates folder named `index.html`. It can contain anything, just make sure it’s an HTML file. Here is an example;

```html
{% raw %}
<!DOCTYPE>
<html>
<head>
    <title>{{ title }}</title>
<link
    rek="stylesheet"
    type="text/css"
    href="{{url_for('static', filename='style.css')}}"
/>
</head>

<body>
    <h1>Welcome to my page</h1>
    <p>Add text here</p>
</body>
</html>
{% endraw %}
```

## Passing python variables to your `HTML` document
`{% raw %}{{ title }}{% endraw %}` is an embedded python variable that we will be converted to `HTML` by the `Jinja2` template engine.
We shall pass this variable as a parameter to the `render_template` function.


##  Including `css` files in your `HTML` files
```html
{% raw %}
<link href=”{{url_for(‘static’, filename=’style.css’)}}” />
{% endraw %}
```

**url_for** is a flask function that helps us locate static resources. Your would have to include in in `app/views.py`
You have to make sure that there is a `style.css` file in your `static` folder. It could contain some as simple as;
`body{background-color:orange;}
You can also add `JavaScript` files/libraries similarly.

just below `from app import Raj` add the following;

`from flask import url_for`

Now to render our HTML  file we will need to import the `render_template` function from flask;
Modify the previous line so that it says; 

`from flask import url_for, render_template`

Now edit our `index()` in `views.py`;

```python
def():
    return render_template("index.html", title="Raj's application")
```

Save the file and run your app.
It should now display the HTML file you included/created in the `app/templates` folder.

## Updated app directory
```python
/fenv
/myapp
----app/
--------__init__.py
--------views.py
--------static/
------------style.css
--------templates
------------index.html
----run.py
----tenv
```

That’s it for now. In the next part, I will talk about using `flask` extensions and how to use `blueprints`.
Until then; Happy coding 🙂 .