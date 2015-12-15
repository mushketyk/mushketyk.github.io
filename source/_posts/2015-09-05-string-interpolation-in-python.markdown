---
layout: post
title: "How to implement string interpolation in Python"
date: 2015-09-05 15:32:28 +0100
comments: true
categories: [python]
description: "This post describes how to implement string interpolation one in Python"
categories: "python, string, interpolation"
---

String interpolation is a process of substituting values of local variables into placeholders in a string.

It is implemented in many programming languages such as Scala:

```scala
//Scala 2.10+
var name = "John";
println(s"My name is $name")
>>> My name is John
```

Perl:

```perl
my $name = "John";
print "My name is $name";
>>> My name is John
```

CoffeeScript:

```coffeescript
name = "John"
console.log "My name is #{name}"
>>> My name is John
```

and many others.

On the first sight, it doesn't seem that it's possible to use string interpolation in Python. However, we can implement it with just 2 lines of Python code.

<!--more-->

But let's start with basics. An idiomatic way to build a complex string in Python is to use the "format" function:

```python
print "Hi, I am {} and I am {} years old".format(name, age)
>>> Hi, I am John and I am 26 years old
```

Which is much cleaner than using string concatenation:

```python
print "Hi, I am " + name + " and I am " + str(age) + " years old"
Hi, I am John and I am 26 years old
```


But if you use the "format" function in this way the output depends on the order of arguments:

```python
print "Hi, I am {} and I am {} years old".format(age, name)
Hi, I am 26 and I am John years old
```

To avoid that we can pass key-value arguments to the "format" function:

```python
print "Hi, I am {name} and I am {age} years old".format(name="John", age=26)
Hi, I am John and I am 26 years old

print "Hi, I am {name} and I am {age} years old".format(age=26, name="John")
Hi, I am John and I am 26 years old
```

Here we had to pass all variables for string interpolation to the "format" function, but still we have not achieved what we wanted, because "name" and "age" are not local variables. Can "format" somehow access local variables?

To do it we can get a dictionary with all local variables using the "locals" function:

```python
name = "John"
age = 26

locals()
>>> {
 ...
 'age': 26,
 'name': 'John',
 ...
}
```

Now we just need to somehow pass it to the "format" function. Unfortunately we cannot just call it as "s.format(locals())":

```python
print "Hi, I am {name} and I am {age} years old".format(locals())
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
<ipython-input-5-0fb983071eb8> in <module>()
----> 1 print "Hi, I am {name} and I am {age} years old".format(locals())

KeyError: 'name'

```

This is because "locals()" returns a dictionary, while "format" expects key-value parameters.

Luckily we can convert a dictionary into key-value parameters using "\*\*" opeartor. If we have a function that expects key-value arguments:

```python
def foo(arg1=None, arg2=None):
    print "arg1 = " + str(arg1)
    print "arg2 = " + str(arg2)

```

We can pass parameters packed in a dictionary:

```python
d = {
    'arg1': 1,
    'arg2': 42
}

foo(**d)
>>> arg1 = 1
arg2 = 42

```

Now we can use this technique to implement our first version of string interpolation:

```python
print "Hi, I am {name} and I am {age} years old".format(**locals())
Hi, I am John and I am 26 years old
```

It works, but looks cumbersome. With this approach every time we need to interpolate our string we would need to write "format(\*\*locals())".
It would be great if we could write a function that would interpolate a string like this:

```python
# Can we implement inter() function in Python?
print inter("Hi, I am {name} and I am {age} years old")
>>> Hi, I am John and I am 26 years old
```

 At first it seems impossible, since if we move interpolation code to another function it would not be able to access local variables from a scope where it was called from:

```python
name = "John"
print inter("My name is {name}")

...

def inter(s):
  # How can we access "name" variable from here?
  return s.format(...)
```

And yet, it is possible. Python provides a way to inspect current stack with sys.\_getframe function:

```python
import sys

def foo():
     foo_var = 'foo'
     bar()


 def bar():
     # sys._getframe(0) would return frame for function "bar"
     # so we need to to access 1-st frame
     # to get local variables from "foo" function
     previous_frame = sys._getframe(1)
     previous_frame_locals = previous_frame.f_locals
     print previous_frame_locals['foo_var']


foo()
>>> foo

```

So the only thing that is left is to combine frames introspection with "format" function. Here are 2 lines of code that would do the trick:

```python
def inter(s):
    return s.format(**sys._getframe(1).f_locals)

name = "John"
age = 26

print inter("Hi, I am {name} and I am {age} years old")
>>> Hi, I am John and I am 26 years old

```
