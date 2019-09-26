---
layout: post
title: Comparing Flask, Django, and Rails
feature-img: "assets/img/pexels/frameworks_feature.png"
thumbnail: "assets/img/pexels/frameworks_feature.png"
image: "assets/img/pexels/frameworks_feature.png"
tags: [Flask, Frameworks, Rails, Django, Comparison]
author-id: lehbyte
---

Flask is a very simple python-based micro framework for developing web apps. <br/> 
It's light, flexible, and easy to learn and extensible. <br/>
It's as capable as `django` or `bottle` and other python-frameworks. <br/>

That being said, I think when it comes quickly building a database heavy web app, you'd better off with a framework like `django`. <br />
Why? Flask simply lacks a database abstraction layer to streamline app development. <br/>
While that should not deter anyone from using it, it makes for time consuming setups, time that should be spent developing the app. <br />

But what flask lacks in database abstraction it makes up for in a dedicated extension library.<br/> These are python packages that have been created to work with flask. <br/>

Their purposes range from mail handling to `login` and `database` management. <br /> It's on of the reasons why `flask` is regarded as an extensible, flexible and light weight framework.

>“Micro” does not mean that your whole web application has to fit into a single Python file (although it certainly can), nor does it mean that Flask is lacking in functionality. The “micro” in microframework means Flask aims to keep the core simple but extensible. Flask won’t make many decisions for you, such as what database to use. Those decisions that it does make, such as what templating engine to use, are easy to change. Everything else is up to you, so that Flask can be everything you need and nothing you don’t. - *Pallets*

With an active community, expect to see more extensions as you begin your `flask` development journey. 

**app.py**

```python
from flask import Flask
    app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__=='__main__':
    app.run()
```

## Origins

Flask is based on `Werkzeug` for the server and `Jinja2` for the `html` template engine. <br/>
Werkzeug is a comprehensive WSGI web application library. The most attractive things about werkzeug are; 
1. An interactive debugger that allows for stack trace inspections, 
2. A fully-featured request `object`
3. A response object
4. A routing system for matching `URLs` to endpoints and generating `URLs` for endpoints, with an extensible system for capturing variables from `URL`, a threaded WSGI server for use while developing applications locally, 
5. A test client for simulating `HTTP` requests during testing without requireing running a server.


**Here's an example of a werkzeug server**;

```python
from werkzeug.wrappers import Request, Response

@Request.application
def application(request):
    return Response('Hello, World!')

if __name__ =='__main__':
    from werkzeug.serving import run_simple
    run_simple('localhost', 4000, application)
```

What it does is simply render `Hello, World!` in your browser window, very similar to what our `app.py` did. <br/>
There multiple usages of this Flask code for example in `flask/flask/cli.py`

(around line 702);

**flask/flask/cli.py**
```python
def run_command(info, host, port, reload, debugger, eager_loading,
                with_threads, cert):
    """Run a local development server.

    This server is for development purposes only. It does not provide
    the stability, security, or performance of production WSGI servers.

    The reloader and debugger are enabled by default if
    FLASK_ENV=development or FLASK_DEBUG=1.
    """
    debug = get_debug_flag()

    if reload is None:
        reload = debug

    if debugger is None:
        debugger = debug

    if eager_loading is None:
        eager_loading = not reload

    show_server_banner(get_env(), debug, info.app_import_path, eager_loading)
    app = DispatchingApp(info.load_app, use_eager_loading=eager_loading)

    from werkzeug.serving import run_simple
*       run_simple(host, port, app, use_reloader=reload, use_debugger=debugger,
               threaded=with_threads, ssl_context=cert)
```

## Jinja2

Jinja is a python-based template engine written by Flask’s creator.

**Sample code**
```python
{% raw %}
{% extends "layout.html" %}
{% block body %}
  <ul>
  {% for user in users %}
    <li><a href="{{ user.url }}">{{ user.username }}</a></li>
  {% endfor %}
  </ul>
{% endblock %}
{% endraw %}
```

### Jinja features

- Sandboxed execution mode
- Powerful automatic HTML escaping system
- Template inheritance
- High  performance
- Optional ahead-of-time compilation
- many more

---

# Ruby-on-Rails

Ruby on rails or rails is perhaps one of the most widely used server-side web application frameworks. Unlike Flask, rails provides default structures for a database, web service and webpages and follows the MVC architectural pattern.

The main philosophy behind rails is;

- *DRY (Do not repeat yourself)*
- *COC (Convention over configuration)*

Suppose your app has two seperate pages that perfom two opperations that form a single continuous process or are part of a multi stage process. Rather than control them seperately, it would be better to have one controller and combine those two actions into one in one view.

## Starting with Rails

To start using rails, you have to first make sure that you have ruby installed. You can do so buy running ruby -v. If ruby is installed then the result will be a short description of the version of ruby installed on your system.

I recommend using `rvm` - ruby version manager - which is similar to `pip` for python. <br/>
Below is a list of steps to get started with `ruby`;<br/>

```bash
sqlite3 --version
gem install rails
rails --version
rails new blog
cd blog
bin/rails server
bin/rails generate controller Welcome index
config/routes.rb

bin/rails generate controller Articles
class ArticlesController < ApplicationController
```

The basic structure of your apps directory should be;
```yml
    app/
    bin/
    config
    config.ru
    db/
    Gemfile and Gemfile.lock
    lib/
    log/
    package.json
    public/
    Rakefule
    README.md [optional]
    test/
    tmp/
    vendor/
    .gitignore
    .ruby-version
```

Lets break this down some more;

### `app/`
This is for the controllers, models, views, helpers, mailers, channels, jobs, and assets of your application.

### `bin/`
Contains scripts for starting your app, setups, update, deployment.
You will also use to generate controllers for for your views
```bash
$ bin/rails server
$ bin/rails generate controller About index
```

### `config/`
This is where you will configure your application’s routes, database, and more.  It contains files such as;

```yml
    application.rb (initialization code)
    boot.rb
    cable.yml
    credentials.yml.enc
    database.yml
    environment.rb
    environments/
    initializers/
    locales/
    master.key
    puma.rb (Web server built for concurrency)
    routes.rb (where you’ll add routes for your controllers)
    spring.rb
    storage.yml
```

### `config.ru`
Used by rack-based servers to start the application. Rack is a webserver interface  between webservers that support ruby and ruby frameworks.

### `db/`
Contains your database schema as well as db migrations. `schema.rb`
```ruby
ActiveRecord::Schema.define(version: 0) do
    enable_extension "plpgsql"
end
```
### Gemfile and Gemfile.lock

Used by gem. Allows you to add gem dependencies to you rails application.

### `lib/`
Additional modules for your application

### `log/`
Where application errors are stored.

### `package.json`
Specifies npm dependecies you rails application needs. For example if you want to use react, angular for your frontend you would specify such in this file.

### `public/`
Contains static files and compiled assets.

### Rakefile
Locates and loads tasks that can be run from the command line.

### README.md

### `test/`
Unit tests, fixtures, and other test apparatus

### `tmp/`
Temporary files

### `vendor/`
Where third-party code resides.

### `.gitignore`
what to exclude from your git repo.

### `.ruby-version`
contains the default ruby version for your application

 
Unlike `Flask`, Rails comes a lot of inbuilt features. Getting started is just as easy as configuring a few things. <br/> And, you can integrate nodejs into the `frontend` and let ruby handle the backend easily. <br/> This is not as easy to do in `Flask` or `Django`.  

---

# Django

Like Flask, Django is also python based. But unlike Flask Django does everything a framework does.  It tries to support as many features on all database backends.

Use a database that supports these features;

- Data memory processing,
- categorization,
- storage,
- text standardization,
- automatic deletion,
- web or browser and API access,
- real time transactino processing,
- good dashborads or visuals.


These are some of the features Django tries to support;

- Encoding
- Persistent connections
- Server-side cursors
- Manually-specifying values of auto-incrementing primary keys
- Test database templates
- e.t.c

Django follows the M-V-T (Model-View-Template) architectual pattern which is very similar to the M-V-C pattern followed by ruby and other popular frameworks.

By supporting a large number of database features Django seeks to make the creation of complex database-driven website as easy as possible.

Like rails, there’s an emphasis on reusability of components, less code, and the DRY principle. You can easily build a complex app with less code with Django.

## Writing a Simple Django App

There is a really useful tutorial hosted on the Django website for beginers of Django.

> [Writing your first django app](https://docs.djangoproject.com/en/2.2/intro/tutorial01/)

But before you can begin I suggest you switch the documentation version (in the lower right corner of the document) to match the version of python you’re running. You should be fine of your version of python > 3.5.

Below is a code listing of the entire process of setting up a django app

```yml
pip install virtualenv
virtualenv denv && cd denv
python -m django --version (2.7 < 1.11 )
django-admin startproject mysite
python manage.py runserver

mysite/
manage.py
mysite/
__init__.py
settings.py
urls.py
wsgi.py

manage.py:

mysite/__init__.py:
mysite/settings.py:
mysite/urls.py:
mysite/wsgi.py:

python manage.py startapp yourapp
```

---

# Recommendations for beginers

## Flask

If you are starting out with flask I’d recommend Miguel Grinberg’s tutorial which is free.  He initially wrote a tutorial about building a simple micro-blogging app back in 2014.  Its popularity lead to users supporting him to dedicate his time and knowledge in developing “The New and Improved Flask Mega Tutorial” which he made available online for free!

The tutorial is really well organized and easy to follow and in case you run in to any problems chances are that someone in the comments section has had the same problem and it has probably been answered.
Miguel runs the site on which the tutorial is hosted and responds pretty fast to comments so check the comment section before running off to google or Stack Overflow.

**I highly recommend the following books** – in order;

> 1. [The New and Improved Flask Mega Tutorial](https://amzn.to/2PAyc4Q) by ***Miguel Grinberg***
> 2. [ Flask Framework Cookbook ](https://amzn.to/2IXmorY) by ***Shalab A***

The second book is for `flask` programmers who want to go further with `flask`. 


## Ruby

For beginners, I would like to recommend;
> [Learn Ruby For Web Development: Learn Rails The Fast and Easy Way](https://amzn.to/2DAiSAp) ***by John Elder***
[The Rails 5](https://amzn.to/2IJixQ4) - ***Obie Fernandez***


## Django

> 1. [Django for Benginners: Build websites with Python and Django](https://amzn.to/2W3Odm6) by ***William S Vincent***
> 2. [Test Driven Development with Python](https://amzn.to/2XMq6Jk) by ***Harry Percival***
> 3. [Django for APIs: Build web API's with Python and Django](https://amzn.to/2Dzpy1v) by ***William S Vincent***

---

# Final thoughts

By now you should have a pretty decent picture of the benefits of using `flask`, `rails` and `django`. 

Some of you may find somethings easier in one framework and others in another.<br/> The goal is to test the limits of each as much as you can, wherever you can. <br/> This will allow you to hit the ground running upon beginning a new project. <br/>

Ultimately, the choice of what framework to use will depend on your needs/requirements.<br/>
Do not sacrifice ease of use for `extensibility`. Choose wisely.

Lastly, if you spot any grammatical errors or punctuation errors, please point them out in the comment section bellow and as always, happy coding.  :)