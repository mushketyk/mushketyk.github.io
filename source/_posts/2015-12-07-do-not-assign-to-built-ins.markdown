---
layout: post
title: "Do not assign to built-ins"
date: 2015-12-07 08:48:58 +0000
comments: true
categories: [python]
---

A common mistake in Python is to assign a value to a built-in function.

Python allows you to assign a value to a variable with any name, and if you choose one of the names of Python built-in functions, this function won't be available anymore:

```python
>>> len([1, 2, 3])
3
>>> len = 5
# "len" function is no longer available
>>> len([1, 2, 3])

Traceback (most recent call last):
  File "<pyshell#2>", line 1, in <module>
    len([1, 2, 3])
TypeError: 'int' object is not callable
```

A similar issue can arise without explicitly using an assignment. If you use a name of a built-in function for a function's argument a built-in function won't be available:

```python
def split(str):
  # "str" function is no longer available in this function
  ...
```
