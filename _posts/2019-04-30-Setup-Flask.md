---
layout: post
title: Setup - Flask From Scratch - [ 1 ]
feature-img: "assets/img/pexels/python.jpeg"
thumbnail: "assets/img/pexels/python.jpeg"
image: "assets/img/pexels/python.jpeg"
tags: [Flask, Scratch, Two, Backend, Organization]
author-id: lehbyte
---

Flask is one of the best known python micro-frameworks. You can use it to build a simple app and have it up and running in very little time by just writing one python script.

It’s flexible nature makes learning and building web applications fun.
And if you are rusty on your knowledge of python then there’s nothing to fear.
Flask is simple enough for even a beginner to pick up.

This tutorial is for you if you have never built a flask app before or you’ve built one but still don’t understand how flask works.

We will build a flask app together  from scratch.
You will learn about flask basic functions runs as we tweak and shape our app into something better.
After this tutorial, you should be able to build and launch your own flask application without issues.

Ideally, the end goal is to help you acquire knowledge that will allow you to spend more time building your flask app and less time looking through books or googling for things. Lets begin.
Setup your system

There are a number of things you need to have before you can begin this tutorial.
For starters you will need to be on a Linux-based system or something similar.
Not Windows. Okay you can also follow along if you have Windows, the difference is that you will need to navigate to Scripts to activate your virtual environment.
You will obviously have to install python and pip on your Windows system if both are not installed. Everything else is similar.

For this tutorial I’m going to be using Ubuntu. If you are not on a Linux-based system, I  suggest using a Linux-based virtual machine or container otherwise you’re going to end up having issues in subsequent tutorials. For this tutorial it’s okay.

If you are on a Linux-based operating system, then Make sure you have the following installed;

    Python (I am using 2.7)
    Virtualenv
    vim (Optional)

Python usually ships with Ubuntu.
You can check if you have python by typing $ python --version.

If there’s python on your system then that command will return the version of python that’s available on your system. In my case I have python 2.7.16 you might have a different version of python. If you do, make sure that its above 2.7

If you don’t have python installed on your system  then execute the following command;$ sudo apt-get install python
If you have Windows, then just go to the python website to download the latest python installer installer or the one corresponding to 2.7.16

You can also check and  install pip similarly in case it is not installed. For Windows users you can run the pip installer executable to install pip on your system.
You might also need to edit your system path to include both python and pip.

## Getting started
Now that you have python and pip installed create a folder for your app;
```bash
$ cd ~/Documents
$ mkdir myapp
$ cd myapp
```

myapp is where your app is going to reside.

Now create and activate a virtual environment for your app

```bash
$ virtualenv myenv
$ source myenv/bin/activate
```
The first command will install your virtual environment inside your application folder. python, pip are some of the things that will be installed in your virtual environment.

That second command will activate your virtual environment. Your console will look something like this;

`$ (myenv) `

Now when you type python your virtual environment will invoke the python that’s native to that virtual environment not your system.
Once your virtual environment is activated, install the flask package by running this;

`$ (myenv) pip install flask`

This will allow us  to create our flask application.
You can also specify the type of server you want your application to run on
$ export FLASK_ENV=development If you don’t specify this, you app will run on a production server.

## On to the application

Now that we have the basic setup in place, lets try to create a simple flask app.
Create and edit a simple python script using an editor of  your choice (I’m using `vim`);

`$ (myenv) vi app.py`

Add the following code to it;

```python
1. #!myenv/bin/python
2. from flask import Flask
3.
4. app = Flask(__name__)
5. 
6. @app.route("/")
7. def index():
8.     return "Welcome to my flask application."
9. 
10. app.run("127.0.0.1")
```

Then make the script accessible and executable by running the following command:
`$ chmod a+x app.py`
Now run it; `$ ./app.py`
Navigate to `127.0.0.1:5000` or `localhost:5000` in your browser.
If you see a page displaying the words Welcome to my flask application congratulations.

## How is this working?

I will now describe what each line of code in our `app.py` script does.
That is lines; `1, 2, 4, 6, 7, 10`

## Line 1. #!myenv/bin/python

The first line tells bash to execute your script using the instance of python installed in your virtual environment. You can use one virtual environment for multiple flask apps so it’s not necessary and is quite wasteful to have a virtual environment for every flask app you have (in my opinion).  So if you want to build two  or more flask applications using one virtual machine then it would have to reside one level above those flask applications.

For example;

```python
/fenv
----myapp/
-------- app.py
----myapp2/
-------- app.py
----myapp3/
-------- app.py
```

You can then specify on the first line of each app.py script above:

```bash
#!../fenv/bin/python
$ cd ../
```

then create a new virtual environment,
`$ virtualenv fenv`
then quickly go back to your flask app, activate it and install flask;

```bash
$ source ../fenv/bin/activate
$ (fenv) pip install flask
```
Don’t forget to change line 1 of your script so that it says;
`#!../fenv/bin/python`


## Line 2. from flask import Flask

Line 2 in our app.py  imports a Flask object from the flask module.
flask and Flask are two separate things.
flask as I’ve said is a module.

Flask is an **object** that implements a **WSGI** application and acts as the central object. It accepts the name of the module or  the package of the application. Once it is created it will act as a central registry for the view functions, the URL rules, template configuration and so on.

I wont get into what a **WSGI** application is, you can read more about that on Wikipedia if you want but I will say that Flask is based on `Jinja2` and `Werkzeug` which is a comprehensive library of tools for **WSGI** applications.


## Line 4. app = Flask(__name__)

The `Flask` object is what we use to create our app on `line 4`.

`4. app = Flask(__name__)`

The __name__ that you see refers to the name of the module, in this case flask.
You can also call your app anything, calling it app just makes things easier but you can do this;

```python
1. #!../fenv/bin/python
2. from flask import Flask
3. 
4. Raj = Flask(__name__)
5.
6. @Raj.route("/")
7. def page_one():
8.    return "Welcome to Raj's flask application."
9.
10. Raj.run("127.0.0.1")
```

Go ahead and try it out. `$ (fenv) ./app.py`

## Line 6. @app.route("/") or @Raj.route("/")

Line 6 is what is known as a decorator. A decorator is essential a method that is able to “alter” the way a function behaves without having to change the source code of the function.  If you already know what a decorator does then you can skip the code but here’s a brief example.

```python
>>> def D(func): # a decorator for A function 
>>>     print("Function has been decorated!")
>>>     return func
>>>
>>> @D
>>> def A():
>>>     print("I am A.")
>>>
>>> "Function has been decorated!"
>>> D(A())
>>> "I am A"
>>> "Function has been decorated!"
```

So our app’s route method decorates our `page_one()` function in line 7.  Raj.route takes in a URL rule, in this case “/”, and registers the view function – `page_one()`, for that URL rule – “/”.

In addition to the URL rule, we can also specify options such as;

`Raj.route("/", methods=["GET"])`

We didn’t have to specify that method because it’s the default method.

## Line 7. def index(): or page_one(): 
This is just a view function which returns the string "Welcome to Raj's flask application." It’s registered to the URL rule we specify in our app’s route decorator. We can also pass objects and strings to this function for example

```python
def page_one(global_string):
    print global_string

# or an object
def page_one(some_object):
    print some_object.get_message()
```

And so on. Of course you would have to define global_string and some_object

## Line 10. app.run("127.0.0.1") or Raj.run("127.0.0.1")

Raj.run("127.0.0.1") essentially runs our flask application on a local development server at the localhost which is an alias the ip address 127.0.0.1

Now this method can take in the host, port, debug option, and other variables. We only specified the host in our case. By default, the app runs on port 5000 and debugging is disabled.
So to run our app on a different port with debugging enabled we would have to specify it thus;

`Raj.run("localhost", port=8000, debug=True)`

The production environment is what runs by default. You would have to specify your option in a `FLASK_ENV` variable.

To specify a development environment for your application run;
`$ export FLASK_ENV=development`

Then you can either do;
```python
$ flask run or
$ ./run.py
```


## Final flask application code.

```python
#!../fenv/bin/python
from flask import Flask

Raj = Flask(__name__)

@Raj.route("/", methods=["GET"])
def index():
    return ("Welcome to Raj's flask application.")

Raj.run("localhost", port=8000, debug=True)
```

In the next tutorial I will talk about organization as we add more things onto our flask app.
I hope you found this tutorial helpful.  Happy coding 🙂 .