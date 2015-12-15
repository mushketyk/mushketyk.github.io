---
layout: post
title: "Generators in Python"
date: 2015-09-21 21:03:13 +0100
comments: true
categories: [python]
description: ""
keywords: "python, iterator, generator"
---


In previous articles I've wrote about how to create an iterator in Python by implementing [iterator protocol]({% post_url 2015-09-07-anatomy-of-python-iterator %}) or using [**yield** keyword]({% post_url 2015-09-10-efficient-iterators-in-python-with-yield %}). In this article I'll describe generators: a piece of Python syntax that can turn many iterators into one-liners.

<!--more-->

Generators syntax is very similar to list comprehension. The only difference is usage of parentheses instead of square braces:

```python
>>> gen = (i for i in range(3))
>>> gen
<generator object <genexpr> at 0x7f4faa870960>

```

As opposed to the list comprehension generator expression does not generate all values at once. But instead it returns an iterator that can be used to get values one-by-one:

```python
>>> gen.next()
0
>>> gen.next()
1
>>> gen.next()
2
>>> gen.next()

Traceback (most recent call last):
  File "<pyshell#14>", line 1, in <module>
    gen.next()
StopIteration
```

Generators may be especially useful in situations when creation of each value requires extensive amount of computation. Instead of generating all values in advance generators create the next value only when it is requested (when the **next** method is called on the result iterator).

Let's say we want to generate first few Fibonacci numbers. At first we need to implement a function that will calculate n-th Fibonacci number:

```python

def fib(n):
    # Print arguments so we know when a function is called
    print "fib({})".format(n)

    # Regular Fibonacci number calculation
    if n < 0:
        raise ValueError("Input should be non-negative")
    if n == 0 or n == 1:
        return n
    a, b = 0, 1
    for i in range(1, n+1):
        a, b = b, a + b
    return b

>>> fib(2)
fib(2)
2
>>> fib(11)
fib(11)
144

```

Now we can create an iterator for the first 100 Fibonacci numbers.

```python

>>> it = (fib(i) for i in range(100))
>>> it
<generator object <genexpr> at 0x7f4faa870370>

```

As you may notice there is no output from the **fib** function. This is because all calls to it were postponed until actual values are requested:

```python

>>> it.next()
fib(0)
0
>>> it.next()
fib(1)
1

```

In contrast, if we use list comprehension all 100 calls to the fib function would be performed before the result list is created:


```python
>>> lst = [fib(i) for i in range(100)]
fib(0)
fib(1)
fib(2)
...
fib(98)
fib(99)
>>> lst
[0, 1, 2, 3, 5, 8, ..., 218922995834555169026L, 354224848179261915075L]
>>>
```

As you can see all numbers were created in advance.

We can build more complicated generators using the same constructions we can use for list comprehensions. Similarly we can can filter some values using **if** statement at the end of a generator expression:

```python

it = (expression for x in ... if cond)

```

For example if we want to generate all numbers less than 100 that can be divided by 9 we can write code like:

```python
>>> gen = (i for i in range(100) if i % 9 == 0)
>>> gen.next()
0
>>> gen.next()
9
>>> gen.next()
18
...
```

We can also combine multiple loops in one generator using the following construction:

```python
it = (expression for i in a if cond1
      j for j in b if cond2
      ...
      final_condition)

```

Which is equivalent to the following code:

```python
    for i in a:
        if cond1:
            for j in b:
                if cond2:
                    ...
                    if final_condition:
                        yield expression
```

## References

[Generator Tricks for Systems Programmers](http://www.dabeaz.com/generators/)
