---
layout: post
title: "Anatomy of a Python Iterator"
date: 2015-09-07 21:52:41 +0100
comments: true
categories: [python, iterator, __iter__, internals]
---

Iterator is a powerful pattern that was recognized at least as early as 1994 and since then it was incorporated in syntax of almost every modern programming language.

Python also implements this pattern providing a pithy and concise syntax to iterate over lists, maps, dictionaries and other data structures:

```python
for i in [1, 2, 3, 4]:
    print i

```


In this article I will write about how an iterator is used in Python, how to implement your own iterator and what types of iterators exist in Python.

<!--more-->

## How iterator works

Iterable types in Python are not limited to built-ins. Any object can be iterated over but it needs to obey a special protocol.

This means the following:

* An object should define the **\_\_iter\_\_** method that should return an iterator object that will be used to obtain items of a collection one by one

We can use **list** type to illustrate this:

```python
l = [1, 2, 3, 4]
l.__iter__()
>>> <listiterator at 0x7fb685954cd0>

```

* An iterator (returned by the **\_\_iter\_\_** method) should implement the **next** method. Each call to the **next** method should return the next element in a collection

Let's see how it works with a list in Python:

```python
it.next()
>>> 1

it.next()
>>> 2

it.next()
>>> 3

 it.next()
>>> 4

```

* When there are no more items to return the **next** method should raise a **StopIteration** exception.

Let's see if **list** obeys this protocol:

```python
it.next()
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-11-54f0920595b2> in <module>()
---->
```

So when we write the following code:

```python
lst = [1, 2, 3, 4]
for i in lst:
    print i

```

Python actually executes something like this:

```python

# Get iterator for the list
it = lst.__iter__()
while True:
    try:
        # Get next element of the list
        i = it.next()

        # This is a user-defined code that is written in a "for" loop
        print i
    except StopIteration:
        break
```

With this knowledge we are ready to implement our own iterator.

## How to implement an iterator

To make this palpable, let's implement our own simplistic data structure. For simplicity it would be just a wrapper around Python's list:

```python
class MyList(object):
     def __init__(self, lst):
         self.lst = lst
```

Now, let's implement the first part of Python's iterator protocol: define the **\_\_iter\_\_** method:

```python
class MyList(object):
     def __init__(self, lst):
         self.lst = lst

     def __iter__(self):
         return MyIterator(self.lst)

```

We return an instance of iterator via the **\_\_iter\_\_** method. Now let's implement the second part of the iterator protocol: define **MyIterator** with the **next** method:

```python

class MyIterator(object):
    def __init__(self, lst):
        self.lst = lst
        self.pos = 0

    def next(self):
        item = self.lst[self.pos]
        self.pos += 1
        return item

```

Now we are almost done. The only thing that we still need to do is to raise the **StopIteration** exception if there are no more elements to return:

```python

class MyIterator(object):
    def __init__(self, lst):
        self.lst = lst
        self.pos = 0

    def next(self):
        if self.pos == len(self.lst):
            raise StopIteration()
        item = self.lst[self.pos]
        self.pos += 1
        return item

```


Let's check if it works as expected:

```python

for i in MyList([1, 2, 3, 4]):
    print i
   ....:
1
2
3
4

```

## Are we done?

It looks like our iterator is done, but let's try another example with a list:

```python

for i in iter([1, 2, 3, 4]):
    print i

1
2
3
4

```

In this example we get an iterator for a list using **iter** function (that just calls **\_\_iter\_\_** method) and iterate over it. If we try to do the same with the data structure that was written in the previous section it would not work:

```python
for i in iter(MyList([1, 2, 3, 4])):
    print i


Traceback (most recent call last):
  File "<pyshell#43>", line 1, in <module>
    for i in iter(MyList([1, 2, 3, 4])):
TypeError: 'MyList' object is not iterable
```

Why doesn't it work? Let's see what happens.

First of all the **iter** function calls the **\_\_iter\_\_** method to get an instance of **MyIterator**. After this Python attempts to execute something like:

```python
for i in MyIterator([1, 2, 3, 4]):
    print i

```

And as we know when Python executes "**for** ... **in** ..." it tries to call method **\_\_iter\_\_** on an object that is placed on the right side of the **in** operator. So the issue is that **MyIterator** does not define **\_\_iter\_\_** method.

To make our iterator work like a built-in type we need to add **\_\_iter\_\_** method to it that would return... itself:

```python

class MyIterator(object):
    def __init__(self, lst):
        self.lst = lst;
        self.pos = 0

    def __iter__(self):
        return self
    ...

```

Now it should work just as built-in type:

```python

for i in iter(MyList([1, 2, 3, 4])):
	print i

1
2
3
4

```

## Reverse iterator

If you need to iterate over a collection in the reverse order an idiomatic way would be to use the **reversed** function:

```python

for i in reversed([1, 2, 3, 4]):
    print i
   ....:
4
3
2
1

```

The **reversed** function would work with any implementation of an iterator, but since an iterator can be used to traverse collection only in one direction, the **reversed** function would need to store all items from the iterator before it would be able to return the first element (which is the last element in the original collection). This is very inefficient especially if we need to iterate over a long collection.

Fortunately in addition to the **\_\_iter\_\_** function Python provides the **\_\_reversed\_\_** function that should return a reversed iterator to go through a collections in the opposite way:

```python

l = [1, 2, 3, 4]
it = l.__reversed__()

it.next()
>>> 4
it.next()
>>> 3
it.next()
>>> 2
it.next()
>>> 1
it.next()
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-21-54f0920595b2> in <module>()
----> 1 it.next()

StopIteration:

```

The implementation of the reversed iterator looks very similar. Lets update existing collection to return a reversed iterator:

```python

class MyList(object):
    def __init__(self, lst):
        self.lst = lst
    ...
    def __reversed__(self):
        return MyReversedIterator(self.lst)
```

Iterator implementation is almost the same. The only difference is that we start from the end of the list and go to the first element:

```python
class MyReversedIterator(object):
    def __init__(self, lst):
        self.lst = lst
        self.pos = len(lst) - 1

    def __iter__(self):
        return self

    def next(self):
        if self.pos < 0:
            raise StopIteration()
        elem = self.lst[self.pos]
        self.pos -= 1
        return elem
```

Now we can use our object with the **reversed** function:

```python

for i in reversed(MyList([1, 2, 3, 4])):
    print i

4
3
2
1
```


## An iterator is a one-time thing

Beware that usually you can use an iterator only once and generally there is no way to reset it. This can be a problem especially if you pass an iterator to a function that needs to traverse a collection more than once.

To illustrate it let's say we need to calculate a minimum and a maximum numbers in a sequence:

```python
def min_and_max(seq):
    return min(seq), max(seq)

```

Now if we pass a list to this function it will work as expected:

```python

lst = [5, 2, 10, 34, 0]
min_and_max(lst)
>>> (0, 34)

```

But if we pass an iterator we would receive an error instead:

```python
lst = [5, 2, 10, 34, 0]
min_and_max(iter(lst))

Traceback (most recent call last):
  File "<pyshell#65>", line 1, in <module>
    min_and_max(iter(lst))
  File "<pyshell#57>", line 2, in min_and_max
    return min(seq), max(seq)
ValueError: max() arg is an empty sequence
```

This is because the **seq** at first is iterated by the **min** function that iterates over all items in it. After that when it is passed to the **max** function, there is no more elements to return.

The correct way to solve this issue would be to store all items from an iterator to a list that can be traversed multiple times:

```python
def min_and_max(seq):
    seq = list(seq)
    return min(seq), max(seq)
```

This works because if **list** function is applied to a list it create a copy of a list and if it is applied to an iterator it copies all items from an iterator to a list.

Now it works as expected:

```python
lst = [5, 2, 10, 34, 0]
min_and_max(iter(lst))
(0, 34)
```
