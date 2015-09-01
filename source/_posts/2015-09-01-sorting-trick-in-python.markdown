---
layout: post
title: "Sorting trick in Python"
date: 2015-09-01 08:28:07 +0100
comments: true
categories: [Python]
---

How would we sort a list of instances of a class like this?

```python
class Person(object):
     def __init__(self, name, age):
         self.name = name
         self.age = age
     def __repr__(self):
         return "Person(name='{}', age={})".format(self.name, self.age)

```

One way would be to define a "less" operator that can be used by a sorting function:

<!--more-->

```python
class Person(object):
     def __init__(self, name, age):
         ...
     def __lt__(self, other):
         return self.name < other.name
     def __repr__(self):
         ...

Person('John', 32) < Person('Boris', 29)
>>> False

Person('John', 32) > Person('Boris', 29)
>>> True
```

With this we can sort a list:

```python
people = [Person('John', 32), Person('Boris', 29), Person('Ervin', 26)]

sorted(people)
>>>
[Person(name='Boris', age=29),
 Person(name='Ervin', age=26),
 Person(name='John', age=32)]
```

But what if want to sort by different fields in different cases?

Let's say we have class Point with fields "x" and "y":

```python
class Point(object):
     def __init__(self, x, y):
         self.x = x
         self.y = y
     def __repr__(self):
         return "({}, {})".format(self.x, self.y)

```

If we need to find [closest pair of points](http://www.geeksforgeeks.org/closest-pair-of-points/) we may need to sort an array of points in different cases by "x" or by "y" field. Since we can only implement one version of \_\_lt\_\_ method we should find another option.

Let's see how we can overcome this limitation.

At first we can create a function that will extract a value of a field that will be used for sorting:

```python
# "getter" function returns value of "x" field
getter = lambda point: point.x

getter(Point(1, 2))
>>> 1
```

Now this function can be used by "sorted" function to sort a list of points by "x" field:

```python
sorted(points, key=lambda point: point.x)
>>> [(-1, 4), (0, 6), (1, 42), (1, 2), (1, 5)]
```

This works fine, but in Python it can be done with less keystrokes.

Python has the "attrgetter" function in "operator" module that can be used to generate a function that we've implemented manually:

```python
from operator import attrgetter

# "getter" is a function that can extract value of field "x" from
getter = attrgetter('x')

getter(Point(1, 2))
>>> 1
```

Now we can pass this function to "sorted" function as before:

```python
points = [Point(1, 42), Point(1, 2), Point(-1, 4), Point(1, 5), Point(0, 6)]

sorted(points, key=attrgetter('x'))
>>> [(-1, 4), (0, 6), (1, 42), (1, 2), (1, 5)]
```

If we would like to sort a list by "y" field all we need to do is to pass another parameter to "attrgetter" function:

```python
sorted(points, key=attrgetter('y'))
>>> [(1, 2), (-1, 4), (1, 5), (0, 6), (1, 42)]
```

An additional benefit of the "attrgetter" function is that it can sort items in a list by several fields.

If we want items first to be sorted by "x" and then by "y" field we can pass names of those two fields in "attrgetter":

```python
sorted(points, key=attrgetter('x', 'y'))
>>> [(-1, 4), (0, 6), (1, 2), (1, 5), (1, 42)]
```

Notice that all points with "x" value equal to 1 are now sorted by "y" field.

It's interesting to find out how sorting by multiple fields works.

First of all if several arguments are passed to "attrgetter" it returns an array of values instead of a single value:

```python
getter = attrgetter('x', 'y')

getter(Point(1, 2))
>>> (1, 2)
```

So "sorted" function compares arrays of values when it need to decide if one object is "smaller" than another one.

When Python compares arrays it first compares element in position 0, if they are different an array with smaller element would be considered to be "smaller":

```python
(1, 2) < (2, 2)
>>> True

(1, 2) < (0, 2)
>>> False
```

If values in position 0 in both arrays are the same Python would check elements in position 1, then 2, and so on:

```python
(2, 2) < (2, 3)
>>> True

(2, 2) < (2, 2)
>>> False
```

While "attrgetter" is useful for sorting arrays of objects Python also comes with a function that is useful for sorting an array of dictionaries:

```python
from operator import itemgetter

# Function to extract value associated with "key"
getter = itemgetter('key')

getter({'key': 42})
>>> 42
```

We can use this function for sorting similarly to how we used "attrgetter":

```python
lst = [{'key': 5}, {'key': 0}, {'key': 40}]

sorted(lst, key=itemgetter('key'))
>>> [{'key': 0}, {'key': 5}, {'key': 40}]
```
