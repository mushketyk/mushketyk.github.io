---
layout: post
title: 	"Python data structures idioms"
date: 2015-12-21 08:59:24 +0000
comments: true
categories: [python]
keywords: "python, data structures, list, dictionary, idiom"
description: "This post describes multiple idioms that are used to manipulate data structures in Python"
---

Significant portion of our time we as a developers spend writing code that manipulates basic data structures: traverse a list, create a map, filter elements in a collection. Therefore it is important to know how effectively do it in Python and make your code more readable and efficient.

<!--more-->

# Using lists

## Iterate over a list

There are many ways to iterate over a list in Python. And the simplest way would be just to maintain current position in list and increment it on each iteration:

```python
## SO WRONG
l = [1, 2, 3, 4, 5]
i = 0
while i < len(l):
	print l[i]
	i += 1

```

This works, but Python provides a more convenient way to do using [**range**](https://docs.python.org/2/library/functions.html#range) function. **range** function can be used to generate numbers from 0 to N and this can be used as an analog of a **for** loop in C:

```python
## STILL WRONG
for i in range(len(l)):
	print l[i]

```

While this is more concise, there is a better way to do it since Python let us iterate over a list directly, similarly to **foreach** loops in other languages:

```python
# RIGHT
for v in l:
  print v

```

## Iterate a list in reverse order

How can we iterate a list in the reverse order? One way to do it would be to use an unreadable 3 arguments version of the **range** function and provide position of the last element in a list (first argument), position of an element before the first element in the list (second argument) and negative step to go in reverse order (third argument):

```python
# WRONG
for i in range(len(l) - 1, -1, -1):
	print l[i]

```

But as you've may already guessed Python should offer a much better way to do it. We can just use [**reversed**](https://docs.python.org/2/library/functions.html#reversed) function in a **for** loop:

```python
# RIGHT
for i in reversed(l):
	print i

```

## Access the last element

A commonly used idiom to access the last element in a list would be: get length of a list, subtract 1 from it, use result number as a position of the last element:

```python
# WRONG
l = [1, 2, 3, 4, 5]
>>> l[len(l) - 1]
5

```

This is cumbersome in Python since it supports negative indexes to access elements from the end of the list. So -1 is the last element:

```python
# RIGHT
>>> l[-1]
5
```

Negative indexes can also be used to access a next to last element and so on:

```python
# RIGHT
>>> l[-2]
4
>>> l[-3]
3
```

## Use sequence unpacking

A common way to extract values from a list to multiple variables in other programming languages would be to use indexes:

```python
# WRONG
l1 = l[0]
l2 = l[1]
l3 = l[2]
```

But Python supports sequence unpacking that lets us to extract values from a list to multiple variables:

```python
# RIGHT
l1, l2, l3 = [1, 2, 3]

>>> l1
1
>>> l2
2
>>> l3
3
```

You can read more about sequence unpacking [here]({% post_url 2015-12-14-sequence-unpacking-in-python %}).

## Use lists comprehensions

Let's say we want to filter all grades for a movie posted by users of age 18 or bellow.

How many times did you write code like this:

```python
# WRONG
under_18_grades = []
for grade in grades:
	if grade.age <= 18:
		under_18_grades.append(grade)

```

Do it no more in Python and use list comprehensions with **if** statement instead.

```python
# RIGHT
under_18_grades = [grade for grade in grades if grade.age <= 18]
```

## Use enumerate function

Sometimes you need to iterate over a list and keep track of a position of each element. Say, if you need to display a menu items in a shell you can simply use the **range** function:

```python
# WRONG
for i in range(len(menu_items)):
	menu_items = menu_items[i]
	print "{}. {}".format(i, menu_items)
```

A better way to do it would be to use [**enumerate**](https://docs.python.org/2/library/functions.html#enumerate) function. It is a iterator that returns pairs each of which contains position of an element and the element itself:

```python
# RIGHT
for i, menu_items in enumerate(menu_items):
	print "{}. {}".format(i, menu_items)
```

## Use keys to sort

A typical way to sort elements in other programming languages is to provide a function that compares two objects along with a collection to sort. In Python it would look like:

```python
people = [Person('John', 30), Person('Peter', 28), Person('Joe', 42)]

# WRONG
def compare_people(p1, p2):
	if p1.age < p2.age:
		return -1
	if p1.age > p2.age:
		return 1
	return 0

sorted(people, cmp=compare_people)

[Person(name='Peter', age=28), Person(name='John', age=30), Person(name='Joe', age=42)]
```

But this is not the best way to do it. Since all we need to do to compare two instances of **Person** class is to compare values of their **age** field. Why should we write a complex compare function for this?

Specifically for this case [**sorted**](https://docs.python.org/2/library/functions.html#sorted) function accepts **key** function that is used to extract a key that will be used to compare two instances of an object:

```python
# RIGHT
sorted(people, key=lambda p: p.age)
[Person(name='Peter', age=28), Person(name='John', age=30), Person(name='Joe', age=42)]
```

## Use all/any functions

If you want to check if all or any value in a collection is True one way would be iterate over a list:

```python
# WRONG
def all_true(lst):
	for v in lst:
		if not v:
			return False
	return True
```

But Python already has [**all**](https://docs.python.org/2/library/functions.html#all), [**any**](https://docs.python.org/2/library/functions.html#any) functions for that. **all** returns True if all values in an iterable passed to it are True, while **any** returns True if at least one of values passed to it is True:

```python
# RIGHT
all([True, False])
>> False

any([True, False])
>> True
```

To check if all items comply with a certain condition, you can convert a list of arbitrary objects to a list of booleans using list comprehension:

```python
all([person.age > 18 for person in people])
```

Or you can pass a generator (just omit square braces around the list comprehension):

```python
all(person.age > 18 for person in people)
```

Not only this will save you two keystrokes it will also omit creation of an intermediate list (more about this later).

## Use slicing

You can take part of a list using a technique called slicing. Instead of providing a single index in a square brackets when accessing a list you can provide the following three values

```python
lst[start:end:step]
```

All of these parameters are optional and you can get different parts of a list if you omit some of them. If only start position is provided it will return all elements in a list starting from the specified index:

```python
# RIGHT
>>> lst = range(10)
>>> lst[3:]
[3, 4, 5, 6, 7, 8, 9]
```

If only end position is provided slicing will return all elements up to the provided position:

```python
>>> lst[:-3]
[0, 1, 2, 3, 4, 5, 6]
```

You can also get part of a list between two indexes:

```python
>>> lst[3:6]
[3, 4, 5]
```

By default step in slicing is equal to one which mean that all elements between start and end positions are returned. If you want to get only every second element or every third element you need to provide a step value:

```python
>>> lst[2:8:2]
[2, 4, 6]
```
# Do not create unnecessary objects

## Use xrange

**range** is a useful function if you need to generate consistent integer values in a range, but it has one drawback: it returns a list with all generated values:

```python
# WRONG
# Returns a too big list
for i in range(1000000000):
	...
```

Solution here is to use [**xrange**](https://docs.python.org/2/library/functions.html#xrange) function. It immediately return an iterator instead of creating a list:

```python
# RIGHT
# Returns an iterator
for i in xrange(1000000000):
	...
```

The drawback of **xrange** comparing to the **range** function is that it's output can be iterated only once.

### New in Python 3

In Python 3 **xrange** was removed and **range** function behaves like **xrange** in Python 2.x. If you need to iterate over an output of **range** in Python 3 multiple times you can convert its output in to a list:

```python
>>> list(range(10))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## Use izip

If you need to generate pairs from elements in two collections, one way to do it would be to use the **zip** function:

```python
# WRONG
names = ['Joe', 'Kate', 'Peter']
ages = [30, 28, 41]
# Creates a list
zip(names, ages)

[('Joe', 30), ('Kate', 28), ('Peter', 41)]
```

Instead we can use the [**izip**](https://docs.python.org/2/library/itertools.html#itertools.izip) function that would return a return an iterator instead of creating a new list:

```python
# RIGHT
from itertools import izip
# Creates an iterator
it = izip(names, ages)
```
### New in Python 3

In Python 3 **izip** function is removed and **zip** behaves like **izip** function in Python 2.x.

## Use generators

Lists comprehensions is a powerful tool in Python, but since it can use extensive amount of memory since each list comprehension will create a new list:

```python
# WRONG

# Original list
lst = range(10)
# This will create a new list
lst_1 = [i + 1 for i in lst]
# This will create another list
lst_2 = [i ** 2 for i in lst_1]

```

A way to avoid this is to use generators instead of list comprehensions. The difference in syntax is minimal: you should use parenthesis instead of square brackets, but the difference is crucial. The following example does not create any intermediate lists:

```python
# RIGHT

# Original list
lst = range(10)
# Won't create a new list
lst_1 = (i + 1 for i in lst)
# Won't create another list
lst_2 = (i ** 2 for i in lst_1)

```

This is especially handy if you may need to process only part of the result collection to get a result, say to find a first element that match a certain condition.


# Use dictionaries idiomatically

## Avoid using keys() function

If you need to iterate over keys in a dictionary you may be inclined to use **keys** function on a hash map:

```python
# WRONG
for k in d.keys():
	print k

```

But there is a better way, you use iterate over a dictionary it performs iteration over its keys, so you can do simply:

```python
# RIGHT
for k in d:
	print k
```

Not only it will save you some typing it will prevent from creating a copy of all keys in a dict as **keys** method does.


## Iterate over keys and values

If you use **keys** method it's really easy to iterate keys and values in a dictionary like this:

```python

#WRONG
for k in d:
  v = d[k]
	print k, v

```

But there is a better way. You can use **items** function that returns key-value pairs from a dictionary:

```python
# RIGHT
for k, v in d.items():
	print k, v

```

Not only this method is more concise, it's a more efficient too.

## Use dictionaries comprehension

One way to create a dictionary is to assign values to it one-by-one:

```python
# WRONG

d = {}
for person in people:
	d[person.name] = person


```

Instead you can use a dictionary comprehension to turn this into a one liner:

```python
# RIGHT
d = {person.name: person for person in people}
```



# Use collections module

## Use namedtuple

If you need a struct like type you may just define a class with an **__init__** method and a bunch of fields:

```python
# WRONG
class Point(object):
	def __init__(self, x, y):
		self.x = x
		self.y = y
```

However [**collections**](https://docs.python.org/2/library/collections.html) module from Python library provides a [**namedtuple**](https://docs.python.org/2/library/collections.html#collections.namedtuple) type that turns this into a one-liner:

```python
# RIGHT
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
```

In addition **namedtuple** implements **\_\_str\_\_**, **\_\_repr\_\_**, and **\_\_eq\_\_** methods:


```python
>>> Point(1, 2)
Point(x=1, y=2)
>>> Point(1, 2) == Point(1, 2)
True
```

## Use defaultdict

If we need to count a number of times an element is encountered in a collection, we can use a common approach:

```python
# WRONG
d = {}
for v in lst:
	if v not in d:
		d[v] = 1
	else:
		d[v] += 1
```

**collections** module provides a very handy class for this case which is called **defaultdict**. It's constructor accepts a function that will be used to calculate a value for a non-existing key:

```python
>>> d = defaultdict(lambda: 42)
>>> d['key']
42
```

To rewrite counting example we can pass the **int** function to **defaultdict** which returns zero if called with no arguments:

```python
# RIGHT
from collections import defaultdict
d = defaultdict(int)
for v in lst:
	d[v] += 1
```

**defaultdict** is useful when you need to create any kind of grouping of items in a collection, but you just need to get count of elements you may use **Counter** class instead:

```python
# RIGHT
from collections import Counter

>>> counter = Counter(lst)
>>> counter
Counter({4: 3, 1: 2, 2: 1, 3: 1, 5: 1})
```
