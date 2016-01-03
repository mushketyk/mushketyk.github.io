---
layout: post
title: "Avoid comparisons in if statements"
date: 2015-11-30 22:10:25 +0000
comments: true
categories: [python]
description: "Why you should avoid comparisons in if statements in Python"
keywords: "python, idiom, if"
---


If you come from Java or C# background, your instincts may tell you to write a code like this:

```python

def is_list_empty(node):
  """
  Check if a linked list is empty.
  """
  if node != None:
    return False

  return True

```
<!--more-->

While this would be a perfectly valid code in other languages it looks non-idiomatic in Python and it can be rewritten as:

```python

def is_list_empty(node):
  if node:
    return False

  return True

```

It turns out that in Python an object can be evaluated as True or False depending on it's value. None is evaluated as False in if/while statements, while an object (a non-null reference) is evaluated as True.

Python has simple rules about what values are considered to be False in if statements:

```python
None    # None reference
False   # False boolean value
''      # empty string
0, 0.0  # zero
[]      # empty array
()      # empty tuple
{}      # empty dictionary
set()   # empty set

```

Everything else is considered to be True.


## Determine how custom object is evaluated in a boolean context

Default Python rules are good for built-in types, but what if we need to define how an instance of our class is evaluated in a boolean context?

Let's say we have a matrix class that can be created with a set height and width and two methods to get and set values from it:

```python

class Matrix(object):
	def __init__(self, height, width):
		self.height = height
		self.width = width
    # Create table height x width and fill it with zeros
		self.matrix = [[0] * width for i in range(height)]

	def get(self, row, col):
		return self.matrix[row][col]

	def set(self, row, col, value):
		self.matrix[row][col] = value
```

It would be nice if an empty matrix would be treated as False in a boolean context, but as we know by default all object are treated as True in Python.

To change this behavior we need to implement **\_\_nonzero\_\_** method in our class. This method will be called by the Python interpretor when an object is used in boolean context in if/while statements:

```python
class Matrix(object):
  ...
	def __nonzero__(self):
    # This will return True only if both "height" and "width" are non-zero
		return self.height and self.width
```

Now it should work as expected:

```python
>>> m = Matrix(0, 0)
>>> bool(m)
False

>>> if not m:
	print "False"

False

>>> if Matrix(1, 1):
	print "True"

True

```
