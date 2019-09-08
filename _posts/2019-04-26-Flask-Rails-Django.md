---
layout: post
title: Flask, Rails, and Django Compared
feature-img: "assets/img/pexels/codeview.jpeg"
thumbnail: "assets/img/pexels/codeview.jpeg"
image: "assets/img/pexels/codeview.jpeg"
tags: [Flask, Frameworks, Rails, Django, Comparison]
author-id: lehbyte
---

Flask is a very simple python-based framework – oops, micro-framework.
Yep, flask does not require any specific libraries or tools.
If your thinking of building a database heavy app, then look elsewhere because flask has no database abstraction layer to streamline your requests. And did I mention the lack of validation.

But what flask lacks in those areas, it makes up for in a dedicated extension library. The extensions range from handling mail to bootstrapping your flask app. And more are on the way given an active developer community.

Flask is easy too, just take a look at what it takes to have an up and running flask app;

**Sample flask code block**

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

Flask is based on Werkzeug and Jinja2

Werkzeug is a comprehensive WSGI web application library. The most attractive things about werkzeug are; an interactive debugger that allows for stack trace inspections, a fully-featured request object, a response object, a routing system for matching URLs to endpoints and generating URLs for endpoints, with an extensible system for capturing variables from url, a threaded WSGI server for use while developing applications locally, a test client for simulating HTTP requests during testing without requireing running a server.

example;

```python
from werkzeug.wrappers import Request, Response

@Request.application
def application(request):
    return Response('Hello, World!')

if __name__ =='__main__':
    from werkzeug.serving import run_simple
    run_simple('localhost', 4000, application
```

What that does is simply render ‘Hello, World!’ in your browser window.

There multiple usages of this Flask code for example in `flask/flask/cli.py`

(around line 702);

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

Jinja is a template engine for the Python programming language. It was written by Flask’s author. Sample code;

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

## Some recommendations

If you are starting out with flask I’d recommend Miguel Grinberg’s tutorial which is free.  He initially wrote a tutorial about building a simple micro-blogging app back in 2014.  Its popularity lead to users supporting him to dedicate his time and knowledge in developing “The New and Improved Flask Mega Tutorial” which he made available online for free!

The tutorial is really well organized and easy to follow and in case you run in to any problems chances are that someone in the comments section has had the same problem and it has probably been answered.
Miguel runs the site on which the tutorial is hosted and responds pretty fast to comments so check the comment section before running off to google or Stack Overflow.

**I highly recommend the following books** – in that order;

> 1. [The New and Improved Flask Mega Tutorial](https://amzn.to/2PAyc4Q) by ***Miguel Grinberg***
> 2. [ Flask Framework Cookbook ](https://amzn.to/2IXmorY) by ***Shalab A***

I recommend the second book if you are deeply familiar with flask and want to do something even more involve.

# Ruby-on-Rails

Ruby on rails or rails is perhaps one of the most widely used server-side web application frameworks. Unlike Flask, rails provides default structures for a database, web service and webpages and follows the MVC architectural pattern.

The main philosophy behind rails is;

- *DRY (Do not repeat yourself)*
- *COC (Convention over configuration)*

Suppose your app has two seperate pages that perfom two opperations that form a single continuous process or are part of a multi stage process. Rather than control them seperately, it would be better to have one controller and combine those two actions into one in one view.

## Beginning

For beginners, I would like to recommend;
> [Learn Ruby For Web Development: Learn Rails The Fast and Easy Way](https://amzn.to/2DAiSAp) ***by John Elder***

To start using rails, you have to first make sure that you have ruby installed. You can do so buy running ruby -v. If ruby is installed then the result will be a short description of the version of ruby installed on your system.

I recommend using rvm; ruby version manager, which is kind of similar to pip for python. Below is a list of steps to get started with ruby.

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

 
As you can see, compared to Flask, rails has a lot of inbuilt features. You can very easily use a variety of nodejs modules for your frontend something which I don’t think is easy with Flask or django.

If you are serious about developing rails apps  then I recommend;

> [The Rails 5](https://amzn.to/2IJixQ4) - ***Obie Fernandez***

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

**If you’re a beginner, I recommend**

> 1. [Django for Benginners: Build websites with Python and Django](https://amzn.to/2W3Odm6) by ***William S Vincent***
> 2. [Test Driven Development with Python](https://amzn.to/2XMq6Jk) by ***Harry Percival***
> 3. [Django for APIs: Build web API's with Python and Django](https://amzn.to/2Dzpy1v) by ***William S Vincent***

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

## Concluding

By now you should have a pretty decent picture of the advantages and disadvantages of using each one of these three frameworks. 

Somethings are easier done in one framework while others are easier done in another. The choice of what framework to use depends on your needs. You wouldn't want to sacrifice ease of use for extensibility. Choose wisely.

If you spot any grammatical errors or punctuation errors, please point them out in the comment section bellow and as always, suggestions are welcome. 

May the `code` be with you. :)