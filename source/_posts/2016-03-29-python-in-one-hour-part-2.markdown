---
layout: post
title: "Python in One hour. Part 2"
date: 2016-03-29 08:56:28 +0100
comments: true
categories: [python]
keywords: "tutorial, python, introduction"
description: "Short Python tutorial for people familiar with programming"
---

This is the second part of the "Python in 1 hour" tutorial. It will go into more advanced Python features that will help you to develop complex and robust applications.

The only prerequisite for this article is that you should be familiar with the content of the part of this tutorial that you can find [here]({% post_url 2016-03-28-python-in-one-hour-part-1 %}).

<!--more-->

# Comprehensions

If you have been programming for some time you probably wrote a lot of loops where need to change every value in a collection like:

```python
res = []
for s in strings:
  res.append(s.lower())
```

Probably even more often you are writing code that needs to filter a collection based on some criteria:

```python
non_empty = []
for s in strings:
  if s != "":
    non_empty.append(s)
```

Python has a very powerful syntax that can help you to do just that. For instance, the first example in this section can be rewritten like this:

```python
res = [s.lower() for s in strings]
```

This construction called a list comprehensions and it allows us to transform a list in just one line of code. It has two parts. The left part before the **for** keyword defines the transformation of every element in the original collection. The right part defines what collection should be processed and a variable name that will be used to access every element of this collection.

Notice that this will not change a list in-place but will create a new list instead. To update a list in-place you need to use a **for** loop.

To filter a collection we just to change the comprehension expression slightly:

```python
res = [s for s in strings if s != ""]
```

This construction will only return those elements for which the expression in the **if** clause is true.

Filtering and updating can be combined together in a single list comprehension. We can do this with the following expression.

```python
res = [s.lower for s in strings if s != "foo"]
```

Comprehensions can be written not only for lists but also for dictionaries and sets. To make a comprehension expression that will return a set, simply use curly brackets instead of square once:

```python
>>> numbers = {1, 2, 3, 4, 5, 6, 7, 8}
>>> even = {n for n in numbers if n % 2 == 0}
>>> even
set([8, 2, 4, 6])
```

To create a dictionary comprehension use the following expression:

```python
{n: n ** 2 for n in [1, 2, 3]}
{1: 1, 2: 4, 3: 9}
```

You could expect that using parentheses instead of curly or square brackets in comprehensions will generate a tuple:

```python
(s.lower for s in strings if s != "foo")
```

Despite the fact that this resembles the list comprehension it will create an iterator. Such an expression is called "generator" in Python. Generators is a more advanced Python concept and you can read about them [here]({% post_url 2015-09-21-generators-in-python %}).


# Imports

Soon after you start programming in Python you will need to use someone else code.

The process of using a third-party code in your program is called importing. This tutorial assumes that you have already installed a third-party code that you are going to use and it's visible to your Python interpreter. To read more about how to do it read this [introduction]({% post_url 2016-03-27-introduction-to-python %}) to Python.

The basic block of code reuse in Python is a module. Put simply, a module is a file where a developer can define types, constants, and functions. For example, let's assume that someone wrote a module with a two functions and you want to use it in your program:

```python
# my_module.py

def filter_odd(lst):
  return [num for num in lst if num % 2 == 1]


def filter_even(lst):
  return [num for num in lst if num % 2 == 0]

```

There are two ways we can use functions defined in it. The first one is to import the whole module into out code using the **import** statement. In this case, we can access items from a module using construction **my_module.filter_even**:

```python
>>> import my_module
>>> my_module.filter_even([1, 2, 3, 4, 5, 6])
[2, 4, 6]
```

Sometimes writing a module name and a function name may be cumbersome and it's better to write just a function name. To do this we can import just a single function from a module using **from my_module import filter_even**:

```python
>>> from my_module import filter_even
>>> filter_even([1, 2, 3, 4, 5, 6])
[2, 4, 6]
```

It's possible to import multiple functions from a module:

```python
from my_module import filter_even, filter_odd
```

Or to import all functions defined in it:

```python
from my_module import *
```

Both methods will make functions **filter_odd** or **filter_even** available in our code:

```python
>>> filter_odd([1, 2, 3, 4, 5, 6])
[1, 3, 5]
>>> filter_even([1, 2, 3, 4, 5, 6])
[2, 4, 6]
```

To organize multiple modules they can be placed in packages or simply hierarchical directories:

```
.
└── package_1
    ├── __init__.py
    ├── package_2
    │   ├── __init__.py
    │   └── my_module.py
    └── package_3
        └── __init__.py
```

Importing modules in packages is not much different from importing a single module. Just import a module from a particular package:


```python
from package_1.package_2 import my_module
>>> module.filter_odd([1, 2, 3, 4, 5, 6])
[1, 3, 5]
```

As with the regular module import you can import separate function:

```python
from package_1.package_2.my_module import filter_odd, filter_even
```

Not every directory is considered to be a package by Python. Every package should contain **__init__.py** file in order for Python interpreter to see it as a package. This file is usually empty and is serves only as a marker.

# Classes

As in many other programming languages, classes in Python is a way to combine code and data together to facilitate code reuse. As an exercise let's define a class for a user that will have name, surname as fields and will have a method to generate a full name:

```python
>>> class Person(object):
	def __init__(self, name, surname):
		self.name = name
		self.surname = surname

	def get_full_name(self):
		res = ""
		res += self.name
		res += " "
		res += self.surname
		return res
```

Now we can use this class:

```python
>>> p = Person("John", "Doe")
>>> p.get_full_name()
'John Doe'
```

There is a lot going on in this simple class definition, so let's go through this line by line.

In the first line we define a type **Person** and inherit it from **object** type:

```python
class Person(object):
  ...
```

Inheriting from the **object** type looks awkward, but it's a way that was introduced in Python 2 to use so-called **new-style** classes. If you simply write:

```python
class Person:
  ...
```

you will define a class, but this will be a so-called **old-style** class. New-style classes were introduced in Python 2.2 to unify type system. It's worth noting that Python 3 does not have this separation and only have new-style classes that can be created with a simpler version of this syntax:

```python
class Person:
  ...
```

The second line defines a constructor of our class:

```python
  def __init__(self, name, surname):
    ...
```

The first argument is an analog of a **this** reference in other programming languages and should be explicitly be added as the first argument in every method. After the  **self** argument we need to define a list of arguments to a class constructor that we pass when we create an instance of this class, e.g.:

```python
p = Person("John", "Doe")
```

Except the fact that constructor should not return a value, it is similar to any other function. You can use named arguments and default values as described [here]({% post_url 2016-03-28-python-in-one-hour-part-1 %}).

If a constructor is not defined or have no arguments except **self** you can create an instance without passing any values to it:

```python

class Person(object):
  # no constructor
  ...

p = Person()
```

Notice that **\_\_init\_\_** name is surrounded with two **_** symbols. This is a naming format for so-called **magic methods** such as **\_\_eq\_\_** (define objects equality), **\_\_str\_\_** (to define object's string representation) and many [others](http://www.rafekettler.com/magicmethods.html).

In a constructor we can initialize an instance and assign values to it's fields:

```python
  def __init__(self, name, surname):
    # Initialize "name" field
    self.name = name
    # Initialize  "surname" field
    self.surname = surname
```

After a constructor, we defined a single method of our class. It's written as a usual function except the first **self** argument that will be used to access fields of an instance:

```python
	def get_full_name(self):
		res = ""
		res += self.name
		res += " "
		res += self.surname
		return res
```

Note that all fields defined in a constructor of a class are public and can be changed outside of methods:

```python
>>> p = Person("John", "Doe")
>>> p.name = "Foo"
>>> p.surname = "Bar"
>>> p.get_full_name()
'Foo Bar'
```

Common naming convention is to prepend a name of a "private" field with an underscore symbol:

```python
>>> class Person(object):
	def __init__(self, name, surname):
		self._name = name
		self._surname = surname

	def get_full_name(self):
		res = ""
		res += self._name
		res += " "
		res += self._surname
		return res
```

This won't stop a user of your class from changing a value of a "private" field, but it will be much more visible:

```python
>>> p = Person("John", "Doe")
>>> p._name = "John"
>>> p.get_full_name()
'John Doe'
```

# Inheritance

Let's extend the previous example and create a sub-class **Employee** that will have same fields as the **Person** class with an additional **salary** field:

```python
class Employee(Person):
	def __init__(self, name, surname, salary):
		super(Person, self).__init__(name, surname)
		self.salary = salary
```

The new class will have fields and methods from both classes:

```python
>>> e = Employee("John", "Doe", 10000)
>>> e.name
'John'
>>> e.surname
'Doe'
>>> e.salary
10000
>>> e.get_full_name()
'John Doe'
```

The only new element that is used in the definition of the **Employee** class is the **super** function:

```python
super(Person, self).__init__(name, surname)
```

It is used to call a constructor from a super class. **super(Person, self).\_\_init\_\_** returns a reference to a method from a superclass.


## Python 3

In Python 3 calling a method of a superclass was changed and now it looks much simpler:

```python
class Employee(Person):
	def __init__(self, name, surname, salary):
		super().__init__(name, surname)
		self.salary = salary
```

# Exceptions

To throw an exception in Python use the **raise** keyword followed by an instance of an exception to raise:

```python
>>> raise Exception("terrible thing has just happened")

Traceback (most recent call last):
  File "<pyshell#106>", line 1, in <module>
    raise Exception("terrible thing has just happened")
Exception: terrible thing has just happened
```

To catch an exception Python provides a **try/except** block that is slightly different than in other languages:

```python
f = None
try:
 f = open('filename')
except Exception as e:
 print e
else:
 print f.read()
finally:
 if f is not None:
   f.close()
```

**try** and **except** blocks similar to their analogs in many other programming languages. **try** block contains code that may raise an exception while **except** block contains code that will be executed if an exception was raised.

If we need to process different exceptions in a different way we can have multiple **except** blocks:

```python
except IOError as e:
 print "IOError"
except ValueError as e:
 print "ValueError"
except Exception as e:
 print "IOError"
```

Keep in mind that if an exception is raised except blocks are tried sequentially top to bottom. If an exception's type matches one of them it's executed and rest **except** blocks are ignored. This means that in the following case last two **except** block will never be executed:

```python
except Exception as e:
 print "Exception"
except IOError as e:
 print "IOError"
except ValueError as e:
 print "ValueError"
```

**else** block in the try/except is only executed if an exception was not raised in the **try** block. It is executed before the **finally** block and is used to separate code that can raise an exception from code that should not raise any. If an exception is raised in the **else** block it is not going to be processed by any of the **except** blocks.

The **finally** block will be executed in any case no matter if an exception was raised. It is used to close any resources that are used in **try** or **else** blocks.

If you do not need to access an instance of a caught exception you can ignore it by writing the **except** clause like:

```python
except Exception:
```

Python has a number of built-in exception types defined, here is a [list](https://docs.python.org/2/library/exceptions.html) of them.

An exception constructor can accept any number of arguments, but most of the built-in exceptions can be created by passing just an error message or no arguments at all:

```python
>>> raise ValueError("Didn't pass verification")

Traceback (most recent call last):
  File "<pyshell#112>", line 1, in <module>
    raise ValueError("Didn't pass verification")
ValueError: Didn't pass verification
>>> raise TypeError("Nope")

Traceback (most recent call last):
  File "<pyshell#113>", line 1, in <module>
    raise TypeError("Nope")
TypeError: Nope
```

## Python 3

Python 2 also supports an alternative syntax for raising exception:

```python
raise Exception, "error message"
```

And catching exceptions:

```python
except Exception, e:
  ...
```

Both were removed in Python 3.

# Properties

Many classes end up with having some methods for accessing and changing its state called "getters" and "setters". Python provides a special mechanism for writing getters and setters called "properties". Let's rewrite class from one of the previous sections using properties:

```python
class Person(object):
  # Same constructor as before
	def __init__(self, name, surname):
		self._name = name
		self._surname = surname

  # getter for "name" field  
	@property
	def name(self):
		return self._name

  # setter for "name" field
	@name.setter
	def name(self, new_name):
		if new_name is None:
			raise ValueError()
		self._name = new_name

  # getter for "surname" field  
	@property
	def surname(self):
		return self._surname

  # setter for "surname" field  
	@surname.setter
	def surname(self, new_surname):
		if new_surname is None:
			raise ValueError()
		self._surname = new_surname

  # getter for "full_name" that is not based on a single field
	@property
	def full_name(self):
		res = ""
		res += self._name
		res += " "
		res += self._surname
		return res
```

Now we can use this class bit differently:

```python
>>> p = Person('John', 'Doe')
>>> p.full_name
'John Doe'
# Use setters for the fields
>>> p.name = 'Foo'
>>> p.surname = 'Bar'
# Use getters for the fields
>>> p.name
'Foo'
>>> p.surname
'Bar'
>>> p.full_name
'Foo Bar'
```

Let's go through the main new elements in this code. The first few lines define a constructor just as in the original example. The next function defines a getter method for the **name** field:

```python
	@property
	def name(self):
		return self._name
```

Apart from the **@property** (it's called decorator in Python), this is a regular function. The only difference that to trigger it we do not need to write parentheses to trigger it:

```python
>>> p = Person('John', 'Doe')
>>> p.name
'John'
```

The name of the function is very important here because it defines the name of the property.

The next few lines define a setter method:

```python
	@name.setter
	def name(self, new_name):
		if new_name is None:
			raise ValueError()
		self._name = new_name
```

To make this method a property it should start with **@<property_name>.setter**. The name of the method should be the same as the name of the property. This function should not return anything and only change the internal state of an object.

The next few lines define the **surname** property and they are not much different from the **name** property.

The last function in the class defines the **full_name**. The only thing that is interesting about it is that it's not based on a single field, but rather generates a value using multiple fields from an object:

```python
	@property
	def full_name(self):
		res = ""
		res += self._name
		res += " "
		res += self._surname
		return res
```

Note that since we didn't define a setter for the **full_name** property we cannot assign values to it:

```python
>>> p = Person('John', 'Doe')
>>> p.full_name = 'abc'

Traceback (most recent call last):
  File "<pyshell#104>", line 1, in <module>
    p.full_name = 'abc'
AttributeError: can't set attribute
```

# Context manager

To ensure that all acquired resources are released one can use **try**/**finally** blocks like this:

```python
f = None
f = open('/etc/passwd')
try:
  print f.read()
finally:
  if f != None:
    f.close()
```

Similar code should be written if we need to interact with locks:

```python
import threading
lock = threading.Lock()

try:
	lock.acquire()
  print "foo"
finally:
	lock.release()
```

Fortunately, Python provides a better mechanism for that. The **with** keyword was introduced in Python 2.5 that helps to rewrite the previous example using just two lines of code:

```python
with lock:
  print "foo"
```

It encapsulates code for acquiring and releasing the resource and lets us concentrate on the business logic instead. In terms of implementation, context managers boil down to calling special methods **\_\_enter\_\_** and **\_\_exit\_\_** on an object before and after a code in a **with** block.

For some objects, you need to use a special function that will allow using an object with a context manager. To work with files using a context managers we need to use **closing** from the **contextlib**:

```python
from contextlib import closing
with closing(open('/etc/passwd')) as f:
	print f.read()
```

This function can be used with all objects that control resources and have **close** method that is used to release a resource.

## Conclusion

At this point, you should know basics of Python that should allow you to read and write quite sophisticated programs.

If you want to learn more about Python, you can read following articles:

[Anatomy of Python Iterator]({% post_url 2015-09-07-anatomy-of-python-iterator %})
[Generators in Python]({% post_url 2015-09-21-generators-in-python %})
[Python data structures idioms]({% post_url 2015-12-21-python-data-structures-idioms %})

Do not hesitate to write a comment if you have any questions about this article and share it if you like it.
