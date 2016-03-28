---
layout: post
title: "Introduction to Python"
date: 2016-03-27 22:36:17 +0100
comments: true
categories: [python]
keywords: "python, introduction, installation"
description: "Quick introduction into how to start using Python"
---

This short article provides a quick introduction into Python programming language, describes differences between different flavors of Python, and demonstrates how to start working with Python.

<!--more-->

# Why Python

Python is an extremely popular dynamic programming language. It is currently used in multiple areas such as automation, education, scientific computations, websites development and many others. Python was adopted by many IT companies including Netflix, Google, Spotify, Dropbox, Quora and many others.

At the time of writing, Python occupies the 5th place by language popularity according to the [TIOBE index](http://www.tiobe.com/tiobe_index).

In addition to that Python has an amazing community that has created more than 70 thousand packages for working with databases, developing websites, performing scientific computations, etc.

# Why so many Pythons

It turns out that there is no such thing as a single Python language, but rather two Python languages: Python 2 and Python 3.

Python 2.7 is the version of Python that is commonly used in production today. Python 3 is a newer version of this language which is still gaining popularity among developers. Despite the fact that these languages are very similar Python 3 is not backward compatible, meaning that programs written in Python 2 are not valid Python 3 programs.

Even though these languages are different Python 2.7 developer can start code in Python 3 almost immediately, so learning Python 2.7 is still a good time investment.

In addition to having two slightly different languages Python has several different implementation:

* **CPython** - this is the default Python implementation that is executed when you run **python** command on your machine. It's the most supported and widely used Python implementation and I would recommend using this version when you learning Python.
* **PyPy** - is an alternative Python implementation that usually works faster and occupies much less RAM than CPython. Unfortunately, PyPy is not fully compatible with CPython so not every program that works with CPython will work using PyPy. You can read more about compatibility between CPython and PyPy [here](http://pypy.org/compat.html).
* **Jython** - is a project that lets run Python programs on a Java virtual machine. A benefit of this is that you can use both Java and Python modules in the same program
* **IronPython** - similarly to Jython this projects lets us run Python programs on a .NET virtual machine.

# Installation

## Python

By default, Python is installed on most Linux machines, but if you don't have it you can install it with a simple command:

```
sudo apt-get install python
```

On the other hand Python 3 is usually not installed by default. To install it run the following command:

```
sudo apt-get install python3
```
## pip

One of the benefits of Python is that it has thousands and thousands of useful packages that can be used in your programs. The simplest way to install them in your system is by using the [**pip**](https://pypi.python.org/pypi/pip) tool. To install it run the following command:

```
sudo apt-get install python-pip
```

Now to install a Python package simply run **pip install <package>**. For example, this will install a popular plotting Python library **matplotlib**:

```
sudo pip install matplotlib
```

## virtualenv

When you will start working on multiple Python projects at the same time you may find yourself in a situation when different projects require different versions of the same package. Since **pip** allows to have only a single version of a package in the system there is a need to somehow isolate different projects.

**virtualenv** does just that. It can be used to create a separate Python execution environment with its own set of dependencies. To install it simply run following commands:

```
# Install dependencies for virtualenv
sudo apt-get install python-dev build-essential

# Install virtualenv
sudo pip install virtualenv virtualenvwrapper
```

After this you can easily create a virtual environment:

```
# Create a virtual environment in folder "env"
virtualenv env

# Activate newly created environment
source env/bin/activate
```

Now if you run **pip install <package>** it will install a package in the newly created isolated environment and won't be visible outside of it. You can read more about virtualenv [here](https://virtualenv.pypa.io/en/latest/userguide.html).

# Interactive Python shells

The simplest way to start experimenting with Python is by using an interactive shell. If you run **python** (or **python3**) with no arguments it will start an interactive shell where you can execute Python commands:

```
% python                                                                                                                                                                                                                                     ~
Python 2.7.6 (default, Jun 22 2015, 17:58:13)
[GCC 4.8.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> print "Hello, world!"
Hello, world!
```

Unfortunately, default Python shell is very limited. A better option is to use [**IPython**](https://ipython.org/) shell that provides tab-completion, object introspection, system shell access and persistent command history. To install it simply run the following command:

```
sudo pip install ipython
```

Now you can run it with the **ipython** command:

```
% ipython
...

IPython 4.1.2 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]:
```

If you are planning to use Python for scientific applications a better choice could be to use Jupyter that you can use either [online](https://try.jupyter.org/) or install it on your machine with **pip**:

```
sudo pip install -U jupyter
```

Python also has a standard GUI interactive shell called [IDLE](https://docs.python.org/2/library/idle.html). It can be installed with a single command:

```
sudo apt-get install idle
```

To start it just run the **idle** command in your shell and you will see an interactive Python session:

{% img center /images/idle.png %}


To run interactive Python 3 session you need to install a separate **idle3** package:

```
sudo apt-get install idle3
```

To start IDLE for Python 3, run **idle3** command in your shell.

# Development environments

## Editors

There are multiple editors suitable for Python development. In addition to classical vim and emacs that have a number of plug-ins for Python development modern editors such as Sublime or Atom provide good support for Python development.

Here are few useful links if you want to turn your favorite editor into a Python IDE:

* [Setting Up Sublime Text 3 for Full Stack Python Development](https://realpython.com/blog/python/setting-up-sublime-text-3-for-full-stack-python-development/)
* [Install and Configure the Atom Editor for Python](http://www.marinamele.com/install-and-configure-atom-editor-for-python)
* [Configuring vim for Python](https://www.fullstackpython.com/vim.html)
* [Emacs as a Python IDE](http://www.jesshamrick.com/2012/09/18/emacs-as-a-python-ide/)

## IDEs

### PyCharm

[PyCharm](https://www.jetbrains.com/pycharm/) is an IDE created by IntelliJ and has two versions: free Community edition and paid Full version.

{% img center https://upload.wikimedia.org/wikipedia/commons/0/05/PyCharm_4.5.1.png %}

Paid version has more features including web development support, remote development and database support.

### PyDev

PyDev is a plug-in for the Eclipse IDE.

{% img center https://upload.wikimedia.org/wikipedia/commons/c/c9/Screenshot_Vrapper.png %}

It has less features than paid version of PyCharm, but it's absolutely free.
