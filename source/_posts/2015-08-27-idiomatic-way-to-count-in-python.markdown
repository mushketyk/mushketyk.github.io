---
layout: post
title: "Idiomatic way to count in Python"
date: 2015-08-27 20:07:29 +0100
comments: true
categories: [python, idiomatic, idiom, count, pythonic]
---

Let's say we want to count how many time each item appears in a Python list:

```python
lst = ['fred', 'john', 'mike', 'fred']
```

and we expect the following result:

```python
{'mike': 1, 'john': 1, 'fred': 2}
```

Here is a simple but not idiomatic solution:

<!--more-->

```python
def count_1(lst):
	res = {}
	for v in lst:
		# if this is the first time we see this value
		if v not in res:
			# set count to 1
			res[v] = 1
		else:
			# increment existing counter
			res[v] += 1
	return res

>>> count_1(lst)
{'mike': 1, 'john': 1, 'fred': 2}

```

This solution works, but it is so unPythonic. There should be a way to get rid of this ugly if statement inside the loop.

One way would be to use dict.get() method that can return a specified value if a requested key is not presented in a dictionary:

```python
>>> d = {'key': 'val'}
>>> d.get('key')
'val'
>>> d.get('non-existing', 42)
42
```

Now we can use is to rewrite our function:

```python
def count_2(lst):
	res = {}
	for v in lst:
		res[v] = res.get(v, 0) + 1
	return res

>>> count_2(lst)
{'mike': 1, 'john': 1, 'fred': 2}
```

This seems to be much better, but we are not done yet. Python has a specialized type of a dictionary that can return a default value for a non existing key. To specify what default value should be returned we can optionally pass a function:

```python
>>> d = defaultdict(lambda: 42)
>>> d['key'] = 'value'
>>> d['non-existing']
42
>>> d['key']
'value'
```

Now we can rewrite this function using defaultdict:

```python
>>> def count_3(lst):
	res = defaultdict(lambda: 0)
	for v in lst:
		res[v] += 1
	return res

>>> dict(count_3(l))
{'mike': 1, 'john': 1, 'fred': 2}
```

This is much better. But to be really idiomatic we can use Counter class from  [collections](https://docs.python.org/2/library/collections.html) library:

```python
>>> count = Counter(l)
>>> dict(count)
{'mike': 1, 'john': 1, 'fred': 2}
```

In addtion Counter class also provides more useful methods to work with counts [operations](https://docs.python.org/2/library/collections.html#collections.Counter) such as addition of counters, subtraction, intersection, etc.
