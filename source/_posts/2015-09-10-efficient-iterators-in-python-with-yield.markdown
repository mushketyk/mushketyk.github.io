---
layout: post
title: 'Efficient iterators in Python with "yield"'
date: 2015-09-10 08:43:18 +0100
comments: true
categories: [python, generator, iterator, yield]
---

While you can write iterators in Python by implementing [iterator protocol]({% post_url 2015-09-07-anatomy-of-python-iterator %}) it usually requires a lot of code and looks cumbersome. To facilitate this task Python provides a powerful syntax to create iterators. By using these constructions we can write complex iterators using just few lines of code.

<!--more-->

## The power of yield

The first Python feature that can be used to create iterators is the **yield** keyword. **yieled** can be used in a function similarly to the **return** keyword.

```python

def create_generator():
    for i in range(5):
        yield i
```

But while it looks like a regular function it behaves in a completely different way.
If we call this function it won't return 0 or any of number at all. Instead it returns an an iterator:

```python
>>> gen = create_generator()
>>> gen
<generator object create_generator at 0x7f4faa870370>

```

If we call the **next** method on the result iterator it would return all values generated in the loop one by one:

```python
>>> gen.next()
0
>>> gen.next()
1
>>> gen.next()
2
>>> gen.next()
3
>>> gen.next()
4
>>> gen.next()

Traceback (most recent call last):
  File "<pyshell#44>", line 1, in <module>
    gen.next()
StopIteration

```

So instead of return a value directly the **yield** statement determines what values will be returned by the **next** method in the created iterator.

Using **yield** in this case is already beneficial at least in terms of the number of lines of code we need to write to implement this task. Here is an example of a class that implement the same iterator using low-level iterator protocol:

```python
class Iter(object):
    def __init__(self, num):
        self.num = num
        self.curr = 0

    def __iter__(self):
        return self

    def next(self):
        if self.curr == self.num:
            raise StopIteration()
        self.curr += 1
        # Return previous value
        return self.curr - 1

>>> it = iter(Iter(5))
>>> it.next()
0
```

An important feature of a function that is written using **yield** is that when it's called it is not executed till completion. It behaves like if after an execution the **yield** statement a value on the right of **yield** is returned and execution of a function is paused.
Let's write a small example to verify it.

```python

def create_generator():
    for i in range(1, 5):
        # These "print" statements will help us to find out what statements in
        # this function were executed so far
        print "Creating first number"
        yield i
        print "Creating second number"
        yield i * 10
        print "Creating third number"
        yield 1 * 100


>>> gen = create_generator()
>>> gen.next()
Creating first number
1
>>> gen.next()
Creating second number
10
>>> gen.next()
Creating third number
100
>>> gen.next()
Creating first number
2
>>>

```

As we can see when we call the **next** method for the first time only the first **print** statement and the first **yield** statements were executed. The execution of the rest of the function was postponed until we execute the **next** method again.

## More complex iterator using "yield"

The previous example shows that by using **yield** statement we can write iterators that require state management almost effortlessly. So lets try to write a more complex iterator for a binary tree.
Few words about what we are about to do. A [binary tree](https://en.wikipedia.org/wiki/Binary_tree), is a simple recursive data structure. It consists of nodes pointing to other nodes. Every node has a data item associated with it, and pointers to the left and right nodes that have the same structure. The top most node in the tree is called a "root node". Nodes that current node is pointing too are called children or sub-trees.
An important feature of a tree is that nodes pointers should not form a cycle.

```python
class Node(object):
    # left/right pointers are optional since a tree should end somewhere
    def __init__(self, data, left=None, right=None):
        self.data = data
        # Pointer to a left sub-tree
        self.left = left
        # Pointer to a right sub-tree
        self.right = right
```

To implement an iterator for a binary tree we first need to decide in which order we want to go though all nodes. In this article we will implement [in-order traversal](https://en.wikipedia.org/wiki/Tree_traversal#In-order_.28symmetric.29). While there are few others possible orders for the purpose of this article the selection is arbitrary.
In the in-order traversal our iterator should at first return data items from the left sub-tree, then an item from the root node and then all items from the right sub-tree.

This mean that for the following tree:
```python
Node(2, left=Node(1), right=Node(3))
```

our iterator should first return data item from left subtree: 1, then it should return data point from the root node and return 2. At the end it should process the right subtree and return 3.

In a more complex case when left or right sub-tree have own child:

```python
left_sub_tree = Node(2, left=Node(1), right=Node(3))
right_sub_tree = Node(6, left=Node(5), right=Node(7))
Node(4, left=left_sub_tree, right_sub_tree=right_sub_tree)
```

the in-order traversal should at first return all data items from the left sub-tree just as in the previous example: 1, 2, 3. Then it should return data item from the root node: 4, and then it should return elements for the right sub-tree, which should result in the following sequence: 5, 6, 7.

With the **yield** it is incredibly simple to write an iterator for this traversal. It directly follows the description of the algorithm:

```python
def inorder(tree):
    if tree:
        # Recursively iterate over elements in the left sub-tree
        for l_child in inorder(tree.left):
            # Return left sub-tree data elements one-by-one
            yield l_child
        # Return data element from current node
        yield tree.data
        # Recursively iterate over elements in the right sub-tree
        for r_child in inorder(tree.right):
            # Return right sub-tree data elements one-by-one
            yield r_child


>>> tree = Node(2, left=Node(1), right=Node(3))
>>> list(inorder(tree))
[1, 2, 3]
>>> tree = Node(4, left=Node(2, left=Node(1), right=Node(3)), right=Node(6, left=Node(5), right=Node(7)))
>>> list(inorder(tree))
[1, 2, 3, 4, 5, 6, 7]

```

As an exercise, try to implement this iterator using low-level iterator protocol.
