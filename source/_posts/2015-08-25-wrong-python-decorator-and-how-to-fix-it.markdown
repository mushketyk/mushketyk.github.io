---
layout: post
title: "Wrong Python decorator and how to fix it"
date: 2015-08-25 20:19:02 +0100
comments: true
categories: [python]
description: "This post describes a correct way to implement Python decorator"
keywords: "python, decorator, bug"
---

Let's say you need to write a decorator that measures how long does it take to execute a function. For this task you may write something like this:

<!--more-->


```python
def timeit(func):
	def inner(*args, **kwargs):
		before = datetime.now()
		result = func(*args, **kwargs)
		after = datetime.now()
		diff = after.microsecond - before.microsecond
		print "It took {} ms to execute {}".format(diff, func.__name__)

		return result
	return inner
```

So now you can apply this decorator:

```python
@timeit
def fib(n):
	"""Returns n-th fibonachi number"""
	if n < 0:
		raise ValueError("Input should be non-negative")
	if n == 0 or n == 1:
		return n
	a, b = 0, 1
	for i in range(1, n+1):
		a, b = b, a + b
	return b
```

Which seems to work:

```python
>>> fib(100)
It took 156 ms to execute fib
573147844013817084101L
```

Looks fine, but let's try to poke our function in the interpreter:

```python
>>> fib.__name__
'inner'
>>> fib.__doc__
>>>
```

It doesn't make sense! This function is "fib" and not "inner" and it definitely has a docstring.

Luckily we can easily fix it. We can copy necessary fields from a decorated function:

```python
def timeit(func):
	def inner(*args, **kwargs):
    ...
	inner.__name__ = func.__name__
	inner.__doc__ = func.__doc__
	inner.__module__ = func.__module__
	return inner
```

Now it works as expected:

```python
>>> fib.__doc__
'Returns n-th fibonachi number'
>>> fib.__module__
'__main__'
```

Alternatively we can use [wraps](https://docs.python.org/2/library/functools.html#functools.wraps) function from [functools](https://docs.python.org/2/library/functools.html) module:

```python
from functools import wraps

def timeit(func):
  @wraps(func)
	def inner(*args, **kwargs):
    ...
	return inner
```

Which provides the same result.
