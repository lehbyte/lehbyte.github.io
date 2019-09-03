---
layout: post
title: Configuration - Flask From Scratch, part 4
feature-img: "assets/img/pexels/header-bg2.png"
thumbnail: "assets/img/pexels/header-bg2.png"
image: "assets/img/pexels/header-bg2.png"
tags: [Flask, Scratch, Four, Backend, Database, Connections]
author-id: lehbyte
---

## Configuration: Flask From Scratch 4

In part 3 we saw how to add admin views to our app but there’s more to be done.
If I remember correctly, this is how we set up our database

`Raj.config["SQLALCHEMY_DATABASE_URI"]='sqlite:////tmp/raj.db'`

The problem is that we wouldn’t be able to deploy our app on heroku which favors postgresql over sqlite.
In fact heroku does not support sqlite.
Even with postgresql support we would have to figure out storage because heroku does not persist data stored
on the database.

We’d have to configure a database yaml and configure storage.
But first let us fix a number of things.

## Cross browser source forgery

Cross browser forgery facilitates man-in-the-middle attacks so let us make sure our app is protected by enabling it. We do so by setting `WTF_CSRF_ENABLED = True`

## Change our database to postgresql

The original way of configuring our database is prone to path problems. A better, more concrete way of doing it is by relying on the absolute path of the current directory to set the location of the database.
Your resulting `config.py` file should look something like this;

```python
import os
basedir = os.path.abspath(os.path.dirname(__file__))

SQLALCHEMY_DATABASE_URI = 'sqlite:///' + os.path.join(basedir, 'app.db')
SQLALCHEMY_MIGRATE_REPO = os.path.join(basedir, 'db_repository')
SQLALCHEMY_TRACK_MODIFICATIONS = False
WTF_CRF_ENABLED = True
SECRET_KEY = 'secret'
WHOOSH_BASE = os.path.join(basedir, 'app.db')
```

## Content
Our app currently contains no content, lets fix that. Since this is going to be a restaurant app lets thinl
about how to structure things.
We know how to render html templates, but how are we going to debug the looks? One solution is using
Chrome/Firefox dev tools. However this isn’t an optimal solution because you have to copy things back, I mean you could still make changes in realtime and override the default `style.css` file but it’s not good practice
because what works for one window won’t necessarily work for multiple screens in different orientations.

Instead, design for multiple screens. To do that, we will use a css library called`sass` and utilize the screen orientation/screen size option in firefox to view changes on different screen sizes.

We will also use `gulp` to `compile`, `watch`, and `sync` changes to the browser while we edit our `sass` files.

So to recap;
– We content in our html
– Gulpfil to compile, serve and watch files from our app, specifically stylesheets

## Front page

Lets create some content. Since this is going to be a restaurant app lets begin with the
general outline.

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

class ContactForm(FlaskForm):
email = StringField(u'Email', validators=[validators.DataRequired("Email")])
submit = SubmitField("submit")
firstname = StringField(u'Firstname', validators=[Required()])
lastname = StringField(u'Lastname', validators=[Required()])
website = StringField(u'Website', validators=[Required()])
message = TextAreaField(u'Message',[validators.DataRequired('Message')])
def __repr__(self):
return "<firstname=%s, lastname=%s, email=%s, website=%s>" % (
self.firstname, self.lastname, self.email, self.website)

@Raj.route("/", methods=["GET"])
def index():
return render_template('index.html', title="Raj's Restaurant")

@Raj.route("/order", methods=["GET","POST"])
def order():
form = FlaskForm()
if form.submit.data:
print "Yey"
return render_template("order.html", title="Order form")
@Raj.route("/contact", method=["GET","POST"])
def contact():
nl = Newsletter()
cf = ContactForm()
if nl.submit.data:
print str(nl.email.data)
if cf.submit.data:
print cf.firstname.data
print cf.lastname.data
print cf.__repr__
return render_template("contact.html", contactform=cf, newsletter=nl.title="Contact")

admin.add_view(ModelView(User, db.session))
```

And the html file `index.html`

```html
{% raw %}
<!DOCTYPE html>
<head>
<title>{{ title }}</title>
<meta charset="utf-8" />
<link type="stylesheets" href="static/style.css" />
</head>
<html>
<body>
<main>
<h1> Raj's restaurant</h1>
Welcome to raj's restaurant. 

Menu

</div>
</main>
</body>
</html>
{% endraw %}