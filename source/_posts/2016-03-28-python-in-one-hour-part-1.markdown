---
layout: post
title: "Python in one hour. Part 1"
date: 2016-03-28 20:09:40 +0100
comments: true
categories: [python]
keywords: "tutorial, python, introduction"
description:
---

This is a relatively short and concise article that will give you all that you need to know to start reading/writing Python code. It will start with defining a variable and will discuss types, data structures, if-else statements, loops, and functions.

This tutorial does not require any prior knowledge of Python. The only prerequisite for this article is knowledge of basics of any other object-oriented programming language.

Most of the examples in this article are applicable in both Python 2 and Python 3. I point to differences between Python versions where applicable.

<!--more-->

If you want to know how to set up Python please read this [introduction post]({% post_url 2016-03-27-introduction-to-python %}).

# How to follow the examples

To follow examples from this article you can type them in an interactive Python shell. A definite benefit of this approach is that it outputs the result values of every executed expression.

If you want to write a Python script you will need to explicitly output values you are interested in. To do it simply use the **print** operator followed by a value to print:

```python
>>> print "Hello, world!"
Hello, world!
```

Notice that there is no need for a semicolon after a statement. Adding a semicolon would not change the outcome of a statement, but is discouraged.

Comments are started with **#** symbol and are ignored by both interpeter and interactive shells:

```python
# comment
```

## Python 3

In Python 3 operator **print** was removed and was replaced with **print** function. So "Hello world" example will look like this:

```python
>>> print("Hello, world!")
Hello, world!
```

# Variables

To define a variable in Python just write **var_name = value**:

```python
foo = 123
```

Python is a dynamic programming language, so there is no restriction on types of values that can be assigned to a particular variable:

```python
foo = 123
foo = "Hello, world!"
```

Python supports all usual basic types that can be found in other programming languages:

```python
123     # integer
123L    # long
123.45  # float
"str"   # string
True    # boolean
False   # boolean
None    # NoneType analog of null in other languages
```

In addition to that Python supports [complex numbers](https://en.wikipedia.org/wiki/Complex_number) that have real and imaginary part:

```python
1 + 1j  # complex number
2j      # just imaginary part
```

As can be expected Python supports all common mathematical operators:

```python
>>> 1 + 2         # sum
3
>>> 2 - 1         # difference
1
>>> -1            # negated value
-1
>>> +1            # unchanged value
1
>>> 3 * 4         # product
12
>>> 4 / 3         # quotient
1
>>> 5 % 3         # reminder of
2
>>> abs(-3)       # absolute value
3
>>> int(3.5)      # convert to integer
3
>>> long(3.5)     # convert to long
3L
>>> float(1)      # convert to float
1.0
>>> complex(1,2)  # create complex number
(1+2j)
>>> pow(2, 4)     # to the power of
16
>>> 2 ** 4        # to the power of
16
```

... as well as bitwise operators for integers and longs:

```python
>>> 1 & 2     # bitwise and
0
>>> 1 | 2     # bitwise or
3
>>> 3 ^ 1     # bitwise xor
2
>>> 2 << 1    # shift left
4
>>> 2 >> 1    # shift right
1
>>> ~2        # invert bits
-3
```

As many other imperative programming languages Python supports shortcuts for reassigning variables:

```python
a += b # a = a + b
a -= b # a = a - b
a *= b # a = a * b
a /= b # a = a / b
a %= b # a = a % b
a **= b # a = a ** b
a //= b # a = a // b
```

In Python it's possible to assign values to several variables in one line:

```python
>>> a, b = 1, 2
>>> a
1
>>> b
2
```

Which is used in a common Python one-liner for swapping values of two variables:

```python
a, b = b, a
```

## Python 3

In Python 3 the behavior of the **/** operator is different comparing to Python 3. Instead of performing an integer division it performs a floating point division:

```python
# Python 2
>>> 4/3
1

# Python 3
>>> 4 / 3
1.3333333333333333
```

# Strings

Strings can be defined either with double quotes or with single quotes to avoid excessive usage of escape characters:

```python
"hello 'username'"
'hello "username"'

# intead of
"hello \"username\""
# or
'hello \'username\''
```

Additionally, Python supports multi-line strings that can be defined with either three double quotes characters or with three single quote characters:

```python
>>> s = """line1
line2
"""
>>> s
'line1\nline2\n'

>>> s = '''line1
line2
'''
>>> s
'line1\nline2\n'
```

The **len** function can be used to get a length of a string:

```python
>>> len('string')
6
```

To get an n-th character from a string you can use [] operator:

```python
>>> s = 'string'
>>> s[1]
't'
```

[] operator is more versatile than in other languages. For instance, if you pass a negative number to a [] operator you will receive n-th element from the end of the string:

```python
>>> s[-1]
'g'
>>> s[-2]
'n'
```

But this is just the beginning. The second version of the [] operator allows you to get a substring by providing two values separated by a column: start index and stop index (not inclusive).

```python
>>> s[0:3]
'str'
```

Second index can be a negative number. For example to remove last character of from a string we can write:

```python
>>> s = 'string'
>>> s[0:-1]
'strin'
```

The third and less used version of the [] operator lets you provide three parameters: start index, stop index, and a step:

```python
>>> s[0:len(s):1]
'string'
>>> s[0:len(s):2]
'srn'
```

To check if a string contains a substring you can use **in** operator or **not in** operator:

```python
>>> s = 'string'
>>> 'tri' in s
True
>>> 'aaa' not in s
True
```

Strings are immutable so every function that performs any changes to a string will create a new string instance. Here are few methods that can use to change a string:

```python
>>> '   aaa'.lstrip()
'aaa'
>>> 'aaa    '.rstrip()
'aaa'
>>> '   aaa   '.strip()
'aaa'
>>> 'Hello'.swapcase()
'hELLO'
>>> 'Hello'.upper()
'HELLO'
>>> 'Hello'.lower()
'hello'
```

Notice that all of them return a new value instead of changing a string in-place.

**+** or **+=** operators are used to concatenate strings:

```python
>>> s = "Hello,"
>>> s = s + " John"
>>> s += " Doe"
>>> s
'Hello, John Doe'
```

# Data structures

Python has 4 basic built-in data structures: list, tuple, dictionary and set

## list

List in Python is a dynamic array. You can create a list with a short and concise syntax by simply listing all elements of a list in square brackets separated by commas:

```python
[1, 2, 3] # list with three elements
[] # empty List
list() # same as []
```

Lists in Python are not restricted to elements of the same type and can contain values of different types:

```python
[1, 3.0, True, "string", [1, 2, 3]]
```

To get an element of a list one need to use [] operator:

```python
>>> l = [1, 2, 3]
>>> l[2]
3
```

List supports the same forms of [] operator as string does:

```python
>>> l = [1, 2, 3, 4, 5]
>>> l[0:2]
[1, 2]
>>> l[0:5:2]
[1, 3, 5]
```

If an item of list's range is accessed an IndexError exception will be raised:

```python
>>> l[4]

Traceback (most recent call last):
  File "<pyshell#43>", line 1, in <module>
    l[4]
IndexError: list index out of range
```

To add an element to the end of the list one can use **append** function or **extend** function to add multiple elements:

```python
>>> l = [1]
>>> l.append(2)
>>> l.extend([3, 4, 5])
>>> l
[1, 2, 3, 4, 5]
```

To change an element in a list one need just to assign a value to an n-th element:

```python
l[3] = 5
```

Notice that you can use only the basic version of the [] operator for assignment. For example, it's not allowed to assign value to a range of elements:

```python
>>> l[0:3] = 4

Traceback (most recent call last):
  File "<pyshell#10>", line 1, in <module>
    l[0:3] = 4
TypeError: can only assign an iterable
```

As with the string the **len** function is used to get length of a list:

```python
len([1, 2, 3, 4])
```

Another thing to keep in mind that when that list is not copied during an assignment. Every variable contains a reference to a list:

```python
>>> a = [1, 2, 3]
>>> b = a
>>> b.append(4)
>>> b
[1, 2, 3, 4]
>>> a
[1, 2, 3, 4]
```

To copy a list (as well as any other built-in data structure) you need to use the **copy** module:

```python
import copy

>>> a = [1, 2, [3, 4, 5]]
>>> b = copy.copy(a) # Shallow copy
>>> c = copy.deepcopy(a) # Deep copy

# Will not change the original list
>>> b.append(6)
# Will change the original list
>>> b[2].append(7)

# Will not change the original list
>>> c.append(8)
>>> c[2].append(9)
>>> a
[1, 2, [3, 4, 5, 7]]
>>> b
[1, 2, [3, 4, 5, 7], 6]
>>> c
[1, 2, [3, 4, 5, 9], 8]
```

## tuple

Tuples are similar to lists, with the difference that tuples are immutable and cannot be changed after creation. They are created similarly to lists but with parentheses instead of square brackets:

```python
>>> t = (1, 2, 3)
>>> t
(1, 2, 3)
>>> t[2]
3
```

Changing elements of a tuple is not allowed:

```python
>>> t[2] = 123

Traceback (most recent call last):
  File "<pyshell#55>", line 1, in <module>
    t[2] = 123
TypeError: 'tuple' object does not support item assignment
```

typles syntax is a bit tricky when it comes to creating an empty set or a set with just one element:

```python
>>> empty = ()
>>> single = 123, # note the coma at the end
>>> single2 = (123,)
>>> len(empty)
0
>>> len(single)
1
```

Note that simple surrounding a single element with parenthesises is not sufficient to create a tuple:

```python
>>> v = (123) # need a come to make it a tuple
>>> type(v)
<type 'int'>
```

## dictionary

Dictionary is a key-value table in Python. You can create one with the following JSON-like syntax:

```python
d = {
  "key": "value",
  "key2": "value2"
}
```

To get a value you can use either the **get** function or [] operator:

```python
>>> d["key"]
'value'
>>> d.get("key")
'value'
```

These functions are different in how they handle access to a non-existing key. The **get** function will return **None**, while [] operator will raise an exception:

```python
>>> d["no_such_key"]

Traceback (most recent call last):
  File "<pyshell#34>", line 1, in <module>
    d["no_such_key"]
KeyError: 'no_such_key'
>>> print d.get("no_such_key")
None
```

To add an element into a dictionary you can either use the **put** method or an assignment operator:

```python
d.put("key3", "value3")
d["key3"] = "value3"
```

Both methods are equivalent.

To remove an item from a dictionary we can use either **del** operator or the **pop** function. The difference between them is that the **pop** function returns a value associated with the return key, while **del** does not.

```python
>>> del d["key"] # Returns nothing
>>> d.pop("key")
'val'
```

Both will raise a **KeyError** exception if a key does not exist.

Python provides two ways to check if a hash map contains a particular key: the **has_key** method and **in** operator:

```python
>>> d = {"key": "value"}
>>> d.has_key("key")
True
>>> "key" in d
True
```

We can also use **not in** operator to check if a key is not in a dictionary:

```python
>>> "no_such_key" in d
True
```

To get a list of keys simply uses the **keys** method on a dictionary:

```python
>>> d.keys()
['key']
```

### Python 3

Python 3 introduced a change into dictionary interface.

First of all **has_key** method was removed in Python 3, so now the **in** operator is the only way to test if a key exists in a dictionary.

The second difference is that in Python 2 a dictionary has two sets of methods **iterkeys**, **itervalues**, **iteritems** that return iterators for keys, values, and key-value pairs and **keys**, **values**, **items** that return copies of keys, values and key-value pairs. In contrast, Python 3 has only **keys**, **values**, and **items** that returns iterators instead of copies.

## set

Set is an unordered data structure that guarantees no duplicates. To create a set one need to list elements in curly brackets separated by commas:

```python
s = {1, 2, 3}
```

Alternatively, a set can be created with the **set** function that expects a collection (e.g. list) to be passed to it:

```python
s = set([1, 2, 3])
```

Care must be taken when an empty set should be created. Empty curly brackets (**{}**) will create an empty dictionary. To create an empty set simply call the **set** function without arguments:

```python
s = set() # empty set
```

To add an element to a set we can use the **add** method:

```python
>>> s.add(1)
>>> s
set([1])
```

To remove an element from a set we can chose either the **remove** or the **discard** methods. The first one will raise KeyError if an element is not present in the set, while the second one will finish successfully:

```python
>>> s = {1, 2, 3}
>>> s.remove(1)
>>> s.remove(1)

Traceback (most recent call last):
  File "<pyshell#83>", line 1, in <module>
    s.remove(1)
KeyError: 1
>>> s.discard(2)
>>> s.discard(2)
```

As list and dictionary, set supports **in**, **not in** operators and the **len** function:

```python
>>> 1 in {1, 2, 3}
True
>>> 3 not in {1, 2, 3}
False
```

Python supports a number of [operations on sets](https://en.wikipedia.org/wiki/Set_%28mathematics%29#Basic_operations), such as union, intersection, and difference:

```python
>>> s1 = {1, 2, 3}
>>> s2 = {3, 4, 5}
>>> s1 <= {1}         # same as s1.issubset({1})
False
>>> s1 >= {1}         # same as s1.issuperset({1})
True
>>> s1 | s2           # same as s1.union(s2)
set([1, 2, 3, 4, 5])
>>> s1 & s2           # same as s1.intersection(s2)
set([3])
>>> s1 - s2           # same as s1.difference(s2)
set([1, 2])
>>> s1 ^ s2           # same as s1.symmetric_difference(s2)
set([1, 2, 4, 5])
```  

# if-else

To use an if-statement in Python simply write "if condition:":

```python
if True:
    print "Hello, world!"
```

Notice that instead of using brackets or reserved words to group statements in a single block Python considers all statements with the same indentation as a single code block. Statements in an if-block need to start with several whitespaces or a tab character.

To execute several statements in a single if-block simply add few more statements with the same number of spaces or tabs in front of them:

```python
if True:
    print "Hello, world!"
    print "Hello, world!"
```

Do not mix statements indented with a different number of spaces and don't mix tabs and spaces together. This will probably result in an IndentationError exception:

```python
>>> if True:
  print "Hello, world!"
 print "Hello, world!"

  File "<pyshell#18>", line 3
    print "Hello, world!"
                        ^
IndentationError: unindent does not match any outer indentation level
```

Python supports a number of comparison operators that can be used in if-statements:

```python
>>> 1 < 2
True
>>> 1 <= 2
True
>>> 1 > 2
False
>>> 1 >= 2
False
>>> 1 == 1
True
>>> 1 != 1
False
```

Keep in mind that **==** operator will return **True** if two different objects are structurally identical:

```python
>>> l1 = [1, 2, 3]
>>> l2 = [1, 2, 3]
>>> l1 == l2
True
```

To check if two references are pointing to the same object Python provide **is** and **is not** operators:

```python
>>> a =[1, 2, 3]
>>> b = a
>>> a is b
True
>>> a is not b
False
```

Python also supports **elif** and **else** keywords that can be used with **if** statement:

```python
if frequency < 20:
	sound_type = "infrasound"
elif frequency > 20000:
	sound_type = "ultrasound"
else:
	sound_type = "hearing range"
```

Boolean operations are supported with three operators: **and**, **or**, and **not**:

```python
>>> True and False
False
>>> False or True
True
>>> not True
False
```

Beware that **not a == b** is interpreted as **not (a == b)** and not as **not(a) == b**. **a == not b** will result in a syntax error:

```python
>>> True == not False
SyntaxError: invalid syntax
```

## Python 3

**!=** operator has an alternative in Python 2: **<>**. The later is used for backward compatibility only in Python 2 and was removed in Python 3.

# Loops

Python has two types of loops in it: **while** and **for**.

**while** loop works pretty much the same as in other languages it has a condition and a block of code that will be executed while the condition is true:

```python
>>> s = 'str'
>>> i = 0
>>> while i < len(s):
	print s[i]
	i += 1


s
t
r
```

**for** loop is used only for iterating over collections. It has a form **for var_name in collection**:

```python
>>> for i in [1, 2, 3, 4]:
	print i ** i


1
4
27
256
```

This can be used to iterate over characters in a string, or items in lists, tuples or sets.

If **for** loop is used with a dictionary it will iterate over its keys:

```python
>>> d = {"key_1": "value_1", "key_2": "value_2"}
>>> for k in d:
	print k


key_1
key_2
```

If you've been coding in C-like languages you should be familiar with a form of a for loop that can be used to iterate over a consequent range of numbers:

```c
for (int i = 0; i < n; i++) {...}
```

Python does not provide a separate type of loop for this purpose. Instead, we can use the built-in **range** function that can generate an ordered collections that can be iterated over. If it is called with a single argument **n** it will create a list of numbers from **0** to **n-1**:

```python
>>> range(5)
[0, 1, 2, 3, 4]
>>> for i in range(5):
	print i


0
1
2
3
4
```

**range** can also be called with two arguments **n** and **m** it will return a sequence with numbers from **n** to **m-1**:

```python
>>> range(2, 6)
[2, 3, 4, 5]
```

If called with 3 arguments it will return values from **n** to **m-1** with a defined step:

```python
>>> range(2, 10, 2)
[2, 4, 6, 8]
```

Python also provides the **xrange** function that does not generate a list in advance but creates an iterator instead. Since creating an iterator is usually faster and occupies less space it's preferable to use **xrange** instead of **range**.

## Python 3

In Python 3 there is no **xrange** function while **range** behaves exactly like **xrange** in Python 2.

# Functions

**def** operator is used to define a function in Python. It should be followed by a function name, list of arguments, and a function body:

```python
>>> def strip_strings(strs):
	res = []
	for s in strs:
		res.append(s.strip())
	return res

>>> strip_strings([' abc', 'def ', ' foo '])
['abc', 'def', 'foo']
```

You can also define default values for parameters:

```python
>>> def convert(strs, uppercase=True):
	res = []
	for s in strs:
		if uppercase:
			res.append(s.upper())
		else:
			res.append(s.lower())
	return res
```

Passing a value for an argument with a default value is optional. If it's not passed the default value will be used instead:

```python
>>> convert(['abc', 'def'])
['ABC', 'DEF']
>>> convert(['abc', 'def'], True)
['ABC', 'DEF']
```

The only restriction on default arguments is that they should be placed after all non-default arguments in a function definition:

```python
>>> def convert(uppercase=True, strs):
...
SyntaxError: non-default argument follows default argument
```

Values can be passed to a function in two ways: via positional argument or via key-value arguments. Positional arguments is the common way of passing arguments like in other programming languages:

```python
convert(strings, True)
```


With key-value arguments values can be passed in any order. The following calls are identical:

```python
convert(strs=['abc'], uppercase=True)
convert(uppercase=True, strs=['abc'])
```

The only restriction on key-value arguments is that key-value arguments should be passed after all positional arguments:

```python
>>> convert(strs=['abc'], True)
SyntaxError: non-keyword arg after keyword arg
```

# Conclusion

This articles covered basics of Python and you should be ready to write your own code.

Please write your thoughts on this article in the comments section and share if you liked it.
