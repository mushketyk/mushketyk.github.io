---
layout: post
title: "Do not use mutable values as default values"
date: 2015-12-19 23:10:31 +0000
comments: true
categories: [python]
keywords: "python, mutable values, default"
description: "This post describes why mutable variables should not be used as default values and how to avoid it."
---

In Python we can provide default values for function's arguments, which can make code more concise and readable:

```python

def greetings(name="anonymous"):
	print "Hi, {}".format(name)

>>> greetings("Joe")
Hi, Joe
>>> greetings()
Hi, anonymous

```

However a seemingly innocuous code like this may cause significant problems:

```python

def get_hostnames(hostnames=[]):
  hostnames.append('localhost')
  return hostnames

```

<!--more-->

If we call it, it returns the expected list with "localhost" string in it, so everything seems fine:

```python

>>> get_hostnames()
['localhost']

```

But lets call the same function few more times:

```python

>>> get_hostnames()
['localhost', 'localhost']
>>> get_hostnames()
['localhost', 'localhost', 'localhost']
>>> get_hostnames()
['localhost', 'localhost', 'localhost', 'localhost']

```

What has just happened? This is definitely not what we expected!

It turns out that Python interpreter does not evaluates default arguments for every function call. Instead it evaluates them only once during execution. It results in the same list instance being used over and over again for every single function call.

## How to avoid this issue

Try to use immutable objects if you can. Mutability is the cause of the problem here and if an object cannot be changed it doesn't matter how many instances were created by Python interpreter. That's why it's ok to use strings and integers as default arguments.

However if you need to use mutable arguments you can implement the desired behavior: create a new instance of a default argument for every function call:

```python

def get_hostnames(hostnames=None):
  if not hostnames:
    hostnames = []
  hostnames.append('localhost')
  return hostnames

```

Now everything works as expected:

```python
>>> get_hostnames()
['localhost']
>>> get_hostnames()
['localhost']
>>> get_hostnames()
['localhost']
```
