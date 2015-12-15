---
layout: post
title: "else: block in Python loops"
date: 2015-12-10 22:11:12 +0000
comments: true
categories: [python]
description: "Even some seasoned Python programmers do not know is that Python supports else blocks in for and while loops."
keywords: "python, idiom, else"
---

In most of programming languages **else** keyword can only be used in **if/else** constructs. But what even some seasoned Python programmers do not know is that Python supports **else** blocks in **for** and **while** loops.

<!--more-->

**else** block after a loop may be misleading but it's simply a block that is being called if execution of a loop was not interrupted by a **break** statement.

Let's take a look at an example:

```python

for i in range(3):
	print i
else:
	print "in else"


0
1
2
in else
```

In this case since **for** loop was not interrupted and the **else** block was executed.

Now let's add a break statement in a for loop and see if **else** block is being executed:

```python
for i in range(3):
	print i
	break
else:
	print "in else"
print "after loop"


0
after loop

```

Beware that **else** block will be executed even if a loop was not executed at all:

```python
for i in []:
	print "Never ever"
else:
	print "In else"


In else
```

## More realistic example

Usually **else** blocks are used after loops that are searching for a specific element in a sequence and should be called if an element was not found.

Let's say we have a function that iterates over a list and check if all items match a regex. If one of the items does not match the regex a function would display a message and return. If all items comply with a regex the function would print a different message. We can implement this using conventional constructions:

```python
>>> def check_items(lst, regex):
	found = False
	for item in lst:
		if not regex.match(item):
			print "'{}' does not match regex".format(item)
			found = True
			break
	if not found:
		print "all strings match the regex"


>>> check_items(["1", "23", "abc"], re.compile("[0-9]+"))
'abc' does not match regex
>>> check_items(["1", "23", "1234"], re.compile("[0-9]+"))
all strings match the regex
```

In this case we need a flag to track if an element was found in a list. If **else** block is used there would be no need for a flag and this would make this function a bit shorter:

```python

>>> def check_items(lst, regex):
	for item in lst:
		if not regex.match(item):
			print "'{}' does not match regex".format(item)
			break
	else:
		print "all strings match the regex"


>>> check_items(["1", "23", "abc"], re.compile("[0-9]+"))
'abc' does not match regex
>>> check_items(["1", "23", "1234"], re.compile("[0-9]+"))
all strings match the regex

```


## A bit of history

**else** block in loops was invented by Donald Knuth, who suggested that it can be used to avoid some GOTO statements.

The name **else** for the block was selected because when a loop is executed it has an implicit condition that is checked before every iteration. If this condition is not satisfied, Python interpretor would not execute loop's body and will execute **else** block if it exists, hence the weird name.

Nowadays Python community agrees that the name is not intuitive and that it would be better to name it **nobreak**, but it's too late to change it, since it will break backward compatibility.
