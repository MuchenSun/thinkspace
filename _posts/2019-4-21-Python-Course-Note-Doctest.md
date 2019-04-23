---
layout: post
title: "Python Course Note: Doctest"
comments: true
description: "Notes about using doctest module in Python"
keywords: "Python, doctest"
---
According to the official Python3.7.3 document, doctest is a module which "searches for text that look like interactive Python sessions, and then executes those sessions to verify taht they work exactly as shown". 

## A simple example: step 1
As the first example, let's first define a simple Python function:

~~~ python
#!/usr/bin/env python3

__copyright__ = "Muchen Sun: sunmch15@lzu.edu.cn"
__license__ = "MIT"
__version__ = "0.1"

def simple_func(x):
	"""
	test 1: list
	>>> x = [1, 2, 3]
	>>> simple_func(x)
	12
	"""
	return 2 * sum(x)
~~~

First, look at the docstring under the function definition. Doctest module will automatically find this docstring, because it looks like interactive Python sessions. Then it will execute the content in there, and the first line serves as a comment and will be skipped.

## A simple example: step 2

However, in the last example, doctest is not called. To use doctest in one file, there are two ways available.

First, according to the official document, "the simplest way to start using doctest is to end each module with":

~~~ python
if __name__ = "__main__":
	import doctest
	doctest.testmod()
~~~

Then, each time when running the module as a script, the test cases in the docstring will be executed.

After adding this part into the last case, which now is:

~~~ python
#!/usr/bin/env python3

__copyright__ = "Muchen Sun: sunmch15@lzu.edu.cn"
__license__ = "MIT"
__version__ = "0.1"

def simple_func(x):
	"""
	test 1: list
	>>> x = [1, 2, 3]
	>>> simple_func(x)
	12
	"""
	return 2 * sum(x)
	
if __name__ = "__main__":
	import doctest
	doctest.testmod()
~~~

Save the file as "simple_test.py", then execute this module as script by typing:

~~~
python simple_test.py
~~~

and ... nothing happened! Of course nothing will happen, because all the cases pass the test. By adding the flag "-v" in the command, the log information can be output:

~~~
python simple_test.py -v
Trying:
    x = [1, 2, 3]
Expecting nothing
ok
Trying:
    simple_func(x)
Expecting:
    12
ok
1 items had no tests:
    __main__
1 items passed all tests:
   2 tests in __main__.simple_func
2 tests in 2 items.
2 passed and 0 failed.
Test passed.
~~~

Here the interesting thing is that doctest will consider every session as a test. Even though we consider this test as one, doctest will treat it as two.

Another way is to run doctest module directly and set the file name as input:

~~~
python -m doctest simple_test.py
~~~

Here, "-m" flag means executing a module, such as doctest and pip. If the log information is needed, add the "-v" flag in the end of the command.

In this way, it is not necessary to import doctest module and call the testmod method in the source code.

## A simple example: step 3

Now, add a new test case in the docstring:

~~~ python
#!/usr/bin/env python3

__copyright__ = "Muchen Sun: sunmch15@lzu.edu.cn"
__license__ = "MIT"
__version__ = "0.1"

def simple_func(x):
    """
    test 1: list
    >>> x = [1, 2, 3]
    >>> simple_func(x)
    12
    
    test 2: tuple
    >>> x = (1, 2, 3)
    >>> simple_func(x)
    Traceback (most recent call last):
    	...
    TypeError: Oh! Not tuple!
    """
    return 2 * sum(x)
    
if __name__ = "__main__":
    import doctest
    doctest.testmod()
~~~

With no doubt, the second test will fail. So the problem here is how to modify the implementation of the function to let it pass all the test. This kind of development mode is called "test-driven development".

It's an easy problem, one dirty solution is to check the type of input:

~~~ python
#!/usr/bin/env python3

__copyright__ = "Muchen Sun: sunmch15@lzu.edu.cn"
__license__ = "MIT"
__version__ = "0.1"

def simple_func(x):
    """
    test 1: list
    >>> x = [1, 2, 3]
    >>> simple_func(x)
    12
    
    test 2: tuple
    >>> x = (1, 2, 3)
    >>> simple_func(x)
    Traceback (most recent call last):
    	...
    TypeError: Oh! Not tuple!
    """
    if isinstance(x, tuple):
    	raise TypeError("Oh! Not tuple!)
    else:
    	return 2 * sum(x)
    
if __name__ = "__main__":
    import doctest
    doctest.testmod()
~~~

Then test it and output log information:

~~~
python simple_test.py -v
Trying:
    x = [1, 2, 3]
Expecting nothing
ok
Trying:
    simple_func(x)
Expecting:
    12
ok
Trying:
    x = (1, 2, 3)
Expecting nothing
ok
Trying:
    simple_func(x)
Expecting:
    Traceback (most recent call last):
        ...
    TypeError: Oh! Not tuple!
ok
1 items had no tests:
    __main__
1 items passed all tests:
   4 tests in __main__.simple_func
4 tests in 2 items.
4 passed and 0 failed.
Test passed.
~~~

When using "Traceback" in the test case, the traceback stack can be replaced by a "...", as shown in the last example. Matching exceptions in doctest is kind of tedious, if more information is needed, please read the [official document](https://docs.python.org/3/library/doctest.html#what-about-exceptions).

## Using test file
If the number of test cases is large, then put them all in docstring

## References
1. "doctest — Test interactive Python examples": [https://docs.python.org/3/library/doctest.html](https://docs.python.org/3/library/doctest.html)

