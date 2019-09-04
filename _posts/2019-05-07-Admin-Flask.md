---
layout: post
title: Admin - Flask from scratch - [ 3 ]
feature-img: "assets/img/pexels/admin.jpeg"
thumbnail: "assets/img/pexels/admin.jpeg"
image: "assets/img/pexels/admin.jpeg"
tags: [Flask, Scratch, Three, Backend, Admin]
author-id: lehbyte
---

Adding Admin Model Views
Navigate to your app and fire up your virtualenv
`source ../fenv/bin/activate`
Lets add some authentication and permissions to our app.
We will be using the following extensions;

1. Flask-SQLAlchemy
2. Flask-Admin

Flask-Admin will allow us to restrict some views to administrators only. 
This is going to turn out to be a restaurant app.
So lets start by setting up our admin extension.
Go ahead and fire up pip to install Flask-Admin

`pip install Flask-Admin`

Next  modify `app/__init__.py`

```python
from flask import Flask
from flask_admin import Admin

Raj = Flask(__name__)

admin = Admin(Raj, name="Raj's application.")

from app import views
```

You could also add additional options like so;

admin = Admin(Raj, name="Raj's Application", template_mode="bootstrap3")

But that needlessly complicates things.

What’s missing from our app are views for our Admins but putting those views in `__init__.py` throws off our app structure so lets do that in views.

Add admin right after Raj;  in `app/views.py;`

`from app import Raj, admin`

## Model Views

Next we will create Model Views
You can do this two ways, you can create the model views in a file named models.py and add those objects or you can do it inline in `app/views.py like so`;

```python
from flask_admin.contrib.sqla import ModelView
from flask_admin.contrib.sqla import ModelView
...
admin.add_view(ModelView(User, db.session))
admin.add_view(ModelVeiew(Post, db.session))
```

## SQLAlchemy and Flask-Security
We need a database to store our users so lets create it in `__init__.py`

```python
from flask import Flask
from flask_admin import Admin
from flask_sqlalchemy import SQLAlchemy

Raj = Flask(__name__)
Raj.config["SQLALCHEMY_DATABASE_URI"]='sqlite:////tmp/raj.db'
db = SQLAlchemy(Raj)
admin = Admin(Raj, name="My applcation")

from app import views
```

Now navigate to `app/views.py`, create our User and add them to our `admin` views.

```python
from app import Raj, admin, db
from flask_admin.contrib.sqla import ModelView
from flask import render_template, url_for

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    def __repr__(self):
         return '<User %r>' % self.username

@Raj.route("/", methods=["GET"])
admin.add_view(ModelView(User, db.session))
```

The last line adds our User view to the database but we don’t have a database yet.
Lets create one!
Fire up python `python` and create the database.

```python
>>> from app import db
>>> db.create_all()
>>> from app import User
>>> admin = User(username='admin', email='admin@example.com')
>>> guest = User(username='guest', email='guest@example.com')
>>> db.session.add(admin)
>>> db.session.add(guest)
>>> db.session.commit()
```

Now when you run your app navigate to `localhost:8000/admin`, you should be able to see a User tab where an `admin` and a `guest` will be listed along with their email addresses.
This was a quick post but will be making some changes to it.
Happy coding. 🙂