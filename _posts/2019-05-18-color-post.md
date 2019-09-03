---
layout: post
title: Frontend - Flask From Scratch Part 5
tags: [Flask, Scratch, Programming]
color: brown
---

# Getting our frontend ready

If you have been following these tutorials, this is part 5 of our restaurant app.

1. Setup
2. Organization
3. Admin views
4. Backend; Database connections
5. **Frontend; CSS Outline**

Last time we saw how to connect an sqlite database to our app, today we will be working on the Frontend of the app.

## Tools

1. Flask
1. Flask-Scss
3. Virtualenv

Lets install Sass via pip;

```yml
$(pyenv) pip install flask-Scss
```

After that we can import it and pass our app to the Sass object but before we get there, we have to make the `app/assets/scss/` directory and `static/css/` directory.  Our sass files will go in the `app/assets/scss/` directory and the compiled css will go in the `app/static/css/` directory or the `app/static/` directory.

Basically there needs to be an `app/assets/` directory for scss and the `app/static` directory for the compiled css.

## Modified directory

```yml
├── app
│   ├── assets
│   │   └── scss
│   ├── __init__.py
│   ├── __init__.pyc
│   ├── static
│   │   ├── css
│   ├── templates
│   │   ├── contact.html
│   │   ├── index.html
│   │   └── order.html
│   ├── views.py
│   └── views.pyc
├── config.py
├── guni.py
├── README.md
├── requirements.txt
└── werk.py
```

`app/__init__.py`

```yml
from flask import Flask
from flask_admin import Admin
from flask_scss import Scss
from flask_sqlalchemy import SQLAlchemy

Raj = Flask(__name__)
Scss(Raj)
db = SQLAlchemy(Raj)
admin = Admin(Raj, name="Raj's Restaurant.")

from app import views

```

Notice that we removed the SQLALCHEMY configuration line
`Raj.config["SQLALCHEMY_DATABASE_URI"]='sqlite:////tmp/raj.db'`
That is because we already define it in our config.py file;

```yml
import os
basedir=os.path.abspath(os.path.dirname(__file__))

MAINTENANCE=False
ENV='development'
TEMPLATES_AUTO_RELOAD=True

SQLALCHEMY_DATABASE_URI='sqlite:///' + os.path.join(basedir, 'app.db')
SQLALCHEMY_MIGRATE_REPO=os.path.join(basedir, 'db_repository')
SQLALCHEMY_TRACK_MODIFICATIONS=False

WTF_CSRF_ENABLED=True
SECRET_KEY='secret'
WHOOSH_BASE=os.path.join(basedir, 'app.db')

```

`/werk.py`


```yml
#!../../pyenv/bin/python
from app import Raj
if __name__ == '__main__':
    Raj.run(host='lab', debug=True)
```

Now we can run our app; `./werk.py`
If everything is setup right you will receive a notification that Pyscss loaded!
`TEMPLATES_AUTO_RELOAD=True`
Ensures that the page reloads whenever we make a change to the template.
`ENV='development'` just makes sure that we are running a development server.

## Creating our templates
Our templates are empty at the moment so lets add some generic structure to them. We can use one template as a base and extend it for others because Flask supports inheritance.

Here’s our base template;

```yml
<!DOCTYPE html>
<head>
    <title>{{ title }}</title>
    <meta charset="utf-8" />
    <link rel="stylesheet" href="static/css/style.css" />
</head>
<html>
    <body>
        <main>
        <h1> {{ title }} </h1>
            <div class="container">
                <p>Welcome to raj\'s restaurant.</p>
               
                <input id="search" class="search"
                i   for="search" name="search" placeholder="search Menu" />
 
                <input id="search" class="search"
                    for="search" name="search" placeholder="search Appetizers" />
 
                <input id="search" class="search"
                    for="search" name="search" placeholder="ZIP Code" />
   
                <h2>Menu</h2>
                <div class="menu">
 
                </div>
            </div>
        </main>
    </body>
</html>
```

## Menu
Lets create the menu in a seperate file which we will include here.