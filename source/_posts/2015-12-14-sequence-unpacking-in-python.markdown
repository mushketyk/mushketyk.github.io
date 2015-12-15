---
layout: post
title: "Sequence unpacking in Python"
date: 2015-12-14 20:44:26 +0000
comments: true
categories: [python]
description: "One of the unusual features of Python that other programming languages usually don't have is sequence unpacking."
categories: "python, idiomatic, sequence, unpacking"
---


One of the unusual features of Python that other programming languages usually don't have is sequence unpacking. Not only using it makes code more Pythonic it also makes it shorter, safer and easier to understand.


<!--more-->


Let's say we have a sequence of variables that represent information about a person and we would like to unpack it to a set of variables, such as "name", "surname", etc.

```python
person = ("John", "Doe", 28, True)

```

One way to do it would be to assign values from a list to other variables one by one:

```python
name = person[0]
surname = person[1]
age = person[2]
male = person[3]
```

But Python has a special syntax specifically for this case. It's called "sequence unpacking" and lets us assign all values from a tuple to a set of variables:

```python

>>> name, surname, age, male = person
>>> name
'John'

```

It works with lists, deques and strings too:

```python
>>> a, b, c = [1, 2, 3]
>>> a, b, c
(1, 2, 3)

>>> deque([1, 2, 3])
deque([1, 2, 3])
>>> a, b, c = deque([1, 2, 3])

>>> a, b, c = 'abc'
>>> a, b, c
('a', 'b', 'c')
```

If a number of elements on the left and right sides of an assignment are different, Python does not attempt to assign anything and just raises an exception:

```python
>>> a, b = (1, 2, 3)

Traceback (most recent call last):
  File "<pyshell#11>", line 1, in <module>
    a, b = (1, 2, 3)
ValueError: too many values to unpack

>>> a, b, c = (1, 2)

Traceback (most recent call last):
  File "<pyshell#12>", line 1, in <module>
    a, b, c = (1, 2)
ValueError: need more than 2 values to unpack
```

## Other usages

Sequence unpacking can not only be used to extract values from a sequence. It can be used to swap values of two variables:

```python
x, y = y, x
```

See how simple it is comparing to a common way of swapping values of two variables using a temporary one:

```python
t = x
x = y
y = t
```

It also can be used to assign new values to a set of variables

```python
x, y = x + y, x
```

This is especially handy if we have a multi-step algorithm where on every step we should calculate a new set of values using values received from a previous step. For example this technique can be used to calculate n-th number of [Fibonacci sequence](https://en.wikipedia.org/wiki/Fibonacci_number):

```python
def fib(n):
	a, b = 0, 1
	for i in range(1, n+1):
		a, b = b, a + b
	return b
```

## Sequence unpacking of a custom type

To implement sequence unpacking in your own custom type, all you need to do is to implement an **__iter__** method that would return an iterator for your object:

```python
class List(object):

	def __init__(self, lst):
		self.lst = lst

	def __iter__(self):
		return iter(self.lst)
```

Now our custom type can behave just as built-in types:

```python
>>> a, b, c = List([1, 2, 3])
>>> a
1
>>> b
2
>>> c
3
```

You can read more about iterators in Python [here]({% post_url 2015-09-07-anatomy-of-python-iterator %}).

## Changes in Python 3

Python 3 brings a new feature called "Extended Iterable Unpacking" defined in [PEP 3132](https://www.python.org/dev/peps/pep-3132/).

Python 3 introduced a "catch-all" syntax that can be used to assign a part of sequence to a new sequence. It allows for example to separate a first element of a list from other elements:

```python
>>> first, *rest = range(5)
>>> first
0
>>> rest
[1, 2, 3, 4]
```

Last element of a list:

```python
>>> *a, b = range(5)
>>> a
[0, 1, 2, 3]
>>> b
4
```

Or both:

```python
>>> a, *b, c = range(5)
>>> a
0
>>> b
[1, 2, 3]
>>> c
4
>>>
```

You can see how much simpler it is comparing to Python 2:

```python
>>> l = [1, 2, 3]
>>> first, rest = l[0], l[1:]

>>> first
1
>>> rest
[2, 3]
```
