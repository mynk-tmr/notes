Features
- high-level, interpreted, general-purpose
- dynamically-typed and garbage-collected

A Python script is a sequence of Python instructions.

## Data Types

`int`
- no size limit, any number without decimal
- change base `0b10`, `0o10`, `0x10`
- `type(3)` : `<class 'int'>` bcoz int

 `float`
 - any decimal or `4.2e-2`
 - 64-bit “double-precision” values. Any num > `1.79e308` is shown as `1.8e3 inf`
 - `x.as_integer_ratio()` gives exact float

```python
format(math.pi, '.12g')  # give 12 significant digits
'3.14159265359'

format(math.pi, '.2f')   # give 2 digits after the point
'3.14'

repr(math.pi)  #'3.141592653589793'

round(.1 + .1 + .1, 10) == round(.3, 10) #bcoz floats are stored as appx, so direct equality check fails
```

`complex`
- specified as `2+3j` (real + imaginary) 

`str`
- "" or ''
- use `\` before newline to break strings
- raw string `r'a\nb'` (no escape)
- triple quoted strings `''' or """` (template literals)

| Escape Sequence | “Escaped” Interpretation                            |
| --------------- | --------------------------------------------------- |
| `\a`            | ASCII Bell (`BEL`) character                        |
| `\b`            | ASCII Backspace (`BS`) character                    |
| `\f`            | ASCII Formfeed (`FF`) character                     |
| `\n \r`         |                                                     |
| `\N{<name>}`    | Character from Unicode database with given `<name>` |
| `\t \v`         | tab                                                 |
| `\uxxxx`        | Unicode character with 16-bit hex value `xxxx`      |
| `\Uxxxxxxxx`    | Unicode character with 32-bit hex value `xxxxxxxx`  |
| `\ooo`          | Character with octal value `ooo`                    |
| `\xhh`          | Character with hex value `hh`                       |

`bool`
- `True/False`
- truthy/falsy - Boolean objects i.e. True/False

## Built-In Functions

### Math[](https://realpython.com/python-data-types/#math "Permanent link")

|Function|Description|
|---|---|
|[`abs()`](https://realpython.com/python-absolute-value/#using-the-built-in-abs-function-with-numbers)|Returns absolute value of a number|
|`divmod()`|Returns quotient and remainder of integer division|
|[`max()`](https://realpython.com/python-min-and-max/)|Returns the largest of the given arguments or items in an iterable|
|[`min()`](https://realpython.com/python-min-and-max/)|Returns the smallest of the given arguments or items in an iterable|
|`pow()`|Raises a number to a power|
|`round()`|Rounds a floating-point value|
|[`sum()`](https://realpython.com/python-sum-function/)|Sums the items of an iterable|

### Type Conversion[](https://realpython.com/python-data-types/#type-conversion "Permanent link")

| Function    | Description                                                          |
| ----------- | -------------------------------------------------------------------- |
| `ascii()`   | Returns a string containing a printable representation of an object  |
| `bin()`     | Converts an integer to a binary string                               |
| `bool()`    | Converts an argument to a Boolean value                              |
| `chr()`     | Returns string representation of character given by integer argument |
| `complex()` | Returns a complex number constructed from arguments                  |
| `float()`   | Returns a floating-point object constructed from a number or string  |
| `hex()`     | Converts an integer to a hexadecimal string                          |
| `int()`     | Returns an integer object constructed from a number or string        |
| `oct()`     | Converts an integer to an octal string                               |
| `ord()`     | Returns integer representation of a character                        |
| `repr()`    | obj -> str                                                           |
| `str()`     | obj -> str                                                           |
| `type()`    | Returns the type of an object or creates a new type object           |

### Iterables and Iterators[](https://realpython.com/python-data-types/#iterables-and-iterators "Permanent link")

| Function                                                     | Description                                                             |
| ------------------------------------------------------------ | ----------------------------------------------------------------------- |
| [`all()`](https://realpython.com/python-all/)                | Returns `True` if all elements of an iterable are true                  |
| [`any()`](https://realpython.com/any-python/)                | Returns `True` if any elements of an iterable are true                  |
| [`enumerate()`](https://realpython.com/python-enumerate/)    | Returns a list of tuples containing indices and values from an iterable |
| [`filter()`](https://realpython.com/python-filter-function/) | Filters elements from an iterable                                       |
| `iter()`                                                     | Returns an iterator object                                              |
| [`len()`](https://realpython.com/len-python-function/)       | Returns the length of an object                                         |
| [`map()`](https://realpython.com/python-map-function/)       | Applies a function to every item of an iterable                         |
| `next()`                                                     | Retrieves the next item from an iterator                                |
| `range()`                                                    | Generates a range of integer values                                     |
| `reversed()`                                                 | Returns a reverse iterator                                              |
| `slice()`                                                    | Returns a `slice` object                                                |
| [`sorted()`](https://realpython.com/python-sort/)            | Returns a sorted list from an iterable                                  |
| [`zip()`](https://realpython.com/python-zip-function/)       | Creates an iterator that aggregates elements from iterables             |

### Composite Data Type[](https://realpython.com/python-data-types/#composite-data-type "Permanent link")

| Function      | Description                                                                                                                          |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `bytearray()` | Creates and returns an object of the [`bytearray`](https://realpython.com/python-strings/#bytearray-objects) class                   |
| `bytes()`     | Creates and returns a [`bytes`](https://realpython.com/python-strings/#bytes-objects) object (similar to `bytearray`, but immutable) |
| `dict()`      | Creates a [`dict`](https://realpython.com/python-dicts/) object                                                                      |
| `frozenset()` | Creates a [`frozenset`](https://realpython.com/python-sets/#frozen-sets) object                                                      |
| `list()`      | Creates a [`list`](https://realpython.com/python-lists-tuples/) object                                                               |
| `object()`    | Creates a new featureless object                                                                                                     |
| `set()`       | Creates a [`set`](https://realpython.com/python-sets/) object                                                                        |
| `tuple()`     | Creates a [`tuple`](https://realpython.com/python-lists-tuples/) object                                                              |

### Classes, Attributes, and Inheritance[](https://realpython.com/python-data-types/#classes-attributes-and-inheritance "Permanent link")

|Function|Description|
|---|---|
|`classmethod()`|Returns a class method for a function|
|`delattr()`|Deletes an attribute from an object|
|`getattr()`|Returns the value of a named attribute of an object|
|`hasattr()`|Returns `True` if an object has a given attribute|
|`isinstance()`|Determines whether an object is an instance of a given class|
|`issubclass()`|Determines whether a class is a subclass of a given class|
|`property()`|Returns a property value of a class|
|`setattr()`|Sets the value of a named attribute of an object|
|[`super()`](https://realpython.com/python-super/)|Returns a proxy object that delegates method calls to a parent or sibling class|

### Input/Output[](https://realpython.com/python-data-types/#inputoutput "Permanent link")

|Function|Description|
|---|---|
|`format()`|Converts a value to a formatted representation|
|`input()`|Reads input from the console|
|`open()`|Opens a file and returns a file object|
|`print()`|Prints to a text stream or the console|

### Variables, References, and Scope[](https://realpython.com/python-data-types/#variables-references-and-scope "Permanent link")

|Function|Description|
|---|---|
|`dir()`|Returns a list of names in current local scope or a list of object attributes|
|`globals()`|Returns a dictionary representing the current global symbol table|
|`id()`|Returns the identity of an object|
|`locals()`|Updates and returns a dictionary representing current local symbol table|
|`vars()`|Returns `__dict__` attribute for a [module](https://realpython.com/python-modules-packages/), class, or object|

### Miscellaneous[](https://realpython.com/python-data-types/#miscellaneous "Permanent link")

|Function|Description|
|---|---|
|`callable()`|Returns `True` if object appears callable|
|`compile()`|Compiles source into a code or AST object|
|[`eval()`](https://realpython.com/python-eval-function/)|Evaluates a Python expression|
|`exec()`|Implements dynamic execution of Python code|
|`hash()`|Returns the hash value of an object|
|`help()`|Invokes the built-in help system|
|`memoryview()`|Returns a memory view object|
|`staticmethod()`|Returns a static method for a function|
|`__import__()`|Invoked by the `import` statement|

## Operators

mostly same as JS
`//` floor div
`and or not`
`is is not` : identity operator, check if 2 objects are same (memory loc)
`and or not()`

#### PRECEDENCE TABLE

| `()`                   |
| ---------------------- |
| `**`                   |
| `+x`  `-x`  `~x`       |
| `*`  `/`  `//`  `%`    |
| `+`  `-`               |
| `<<`  `>>`             |
| bitwise & ^ \|         |
| equality & memberships |
| `not`                  |
| `and`                  |
| `or`                   |

## Decorators

- HOF that add features to a function by taking it as input.
- implements `__call__()`  (callable) and returns a callable.
- can be chained (inner --> outer)

```python
def decor(fn):
    def inner():
        print(f"Output is {fn()}")
    return inner

logsum = decor(sum)

#@ provides decoration without having to store inner fn in a var

@decor
	def sum(x,y):
		return x+y
```

Sometimes, when you look at a function definition in Python, you might see that it takes two strange arguments: **`*args`** and **`**kwargs`**. If you’ve ever wondered what these peculiar [variables](https://realpython.com/python-variables/) are, or why your IDE defines them in [`main()`](https://realpython.com/python-main-function/), then this article is for you. You’ll learn how to use args and kwargs in Python to add more flexibility to your functions.

**By the end of the article, you’ll know:**

- What `*args` and `**kwargs` actually mean
- How to use `*args` and `**kwargs` in function definitions
- How to use a single asterisk (`*`) to unpack iterables
- How to use two asterisks (`**`) to unpack dictionaries

This article assumes that you already know how to [define Python functions](https://realpython.com/defining-your-own-python-function/) and work with [lists and dictionaries](https://realpython.com/lessons/mutable-data-structures-lists-dictionaries/).

**Free Bonus:** [Click here to get a Python Cheat Sheet](https://realpython.com/python-kwargs-and-args/) and learn the basics of Python 3, like working with data types, dictionaries, lists, and Python functions.

 ==**Take the Quiz:**== Test your knowledge with our interactive “Python args and kwargs: Demystified” quiz. You’ll receive a score upon completion to help you track your learning progress:

---

[

![Python args and kwargs: Demystified](https://files.realpython.com/media/args-and-kwargs-in-Python_Watermarked.508ab9494cb5.jpg)



](https://realpython.com/quizzes/python-args-and-kwargs/)

**Interactive Quiz**

[Python args and kwargs: Demystified](https://realpython.com/quizzes/python-args-and-kwargs/)

In this quiz, you'll test your understanding of how to use *args and **kwargs in Python. With this knowledge, you'll be able to add more flexibility to your functions.

## Passing Multiple Arguments to a Function[](https://realpython.com/python-kwargs-and-args/#passing-multiple-arguments-to-a-function "Permanent link")

**`*args`** and **`**kwargs`** allow you to pass multiple arguments or keyword arguments to a function. Consider the following example. This is a simple function that takes two arguments and returns their sum:

Python

`def my_sum(a, b):     return a + b`

This function works fine, but it’s limited to only two arguments. What if you need to sum a varying number of arguments, where the specific number of arguments passed is only determined at runtime? Wouldn’t it be great to create a function that could sum _all_ the integers passed to it, no matter how many there are?

[Remove ads](https://realpython.com/account/join/)

## Using the Python args Variable in Function Definitions[](https://realpython.com/python-kwargs-and-args/#using-the-python-args-variable-in-function-definitions "Permanent link")

There are a few ways you can pass a varying number of arguments to a function. The first way is often the most intuitive for people that have experience with collections. You simply pass a list or a [set](https://realpython.com/python-sets/) of all the arguments to your function. So for `my_sum()`, you could pass a list of all the integers you need to add:

Python`sum_integers_list.py`

`def my_sum(my_integers):     result = 0     for x in my_integers:         result += x     return result  list_of_integers = [1, 2, 3] print(my_sum(list_of_integers))`

This implementation works, but whenever you call this function you’ll also need to create a list of arguments to pass to it. This can be inconvenient, especially if you don’t know up front all the values that should go into the list.

This is where `*args` can be really useful, because it allows you to pass a varying number of positional arguments. Take the following example:

Python`sum_integers_args.py`

`def my_sum(*args):     result = 0     # Iterating over the Python args tuple     for x in args:         result += x     return result  print(my_sum(1, 2, 3))`

In this example, you’re no longer passing a list to `my_sum()`. Instead, you’re passing three different positional arguments. `my_sum()` takes all the parameters that are provided in the input and packs them all into a single iterable object named `args`.

Note that **`args` is just a name.** You’re not required to use the name `args`. You can choose any name that you prefer, such as `integers`:

Python`sum_integers_args_2.py`

`def my_sum(*integers):     result = 0     for x in integers:         result += x     return result  print(my_sum(1, 2, 3))`

The function still works, even if you pass the iterable object as `integers` instead of `args`. All that matters here is that you use the **unpacking operator** (`*`).

Bear in mind that the iterable object you’ll get using the unpacking operator `*` is not a [`list`](https://realpython.com/python-list/) but a [`tuple`](https://realpython.com/python-lists-tuples/). A `tuple` is similar to a `list` in that they both support slicing and iteration. However, tuples are very different in at least one aspect: lists are [mutable](https://realpython.com/python-mutable-vs-immutable-types/), while tuples are not. To test this, run the following code. This script tries to change a value of a list:

Python`change_list.py`

`my_list = [1, 2, 3] my_list[0] = 9 print(my_list)`

The value located at the very first index of the list should be updated to `9`. If you execute this script, you will see that the list indeed gets modified:

Shell

`$ python change_list.py [9, 2, 3]`

The first value is no longer `0`, but the updated value `9`. Now, try to do the same with a tuple:

Python`change_tuple.py`

`my_tuple = (1, 2, 3) my_tuple[0] = 9 print(my_tuple)`

Here, you see the same values, except they’re held together as a tuple. If you try to execute this script, you will see that the Python interpreter returns an [error](https://realpython.com/python-exceptions/):

Shell

`$ python change_tuple.py Traceback (most recent call last):   File "change_tuple.py", line 3, in <module>     my_tuple[0] = 9 TypeError: 'tuple' object does not support item assignment`

This is because a tuple is an immutable object, and its values cannot be changed after assignment. Keep this in mind when you’re working with tuples and `*args`.

## Using the Python kwargs Variable in Function Definitions[](https://realpython.com/python-kwargs-and-args/#using-the-python-kwargs-variable-in-function-definitions "Permanent link")

Okay, now you’ve understood what `*args` is for, but what about `**kwargs`? `**kwargs` works just like `*args`, but instead of accepting positional arguments it accepts keyword (or **named**) arguments. Take the following example:

Python`concatenate.py`

`def concatenate(**kwargs):     result = ""     # Iterating over the Python kwargs dictionary     for arg in kwargs.values():         result += arg     return result  print(concatenate(a="Real", b="Python", c="Is", d="Great", e="!"))`

When you execute the script above, `concatenate()` will iterate through the Python kwargs [dictionary](https://realpython.com/python-dicts/) and concatenate all the values it finds:

Shell

`$ python concatenate.py RealPythonIsGreat!`

Like `args`, `kwargs` is just a name that can be changed to whatever you want. Again, what is important here is the use of the **unpacking operator** (`**`).

So, the previous example could be written like this:

Python`concatenate_2.py`

`def concatenate(**words):     result = ""     for arg in words.values():         result += arg     return result  print(concatenate(a="Real", b="Python", c="Is", d="Great", e="!"))`

Note that in the example above the iterable object is a standard `dict`. If you [iterate over the dictionary](https://realpython.com/iterate-through-dictionary-python/) and want to return its values, like in the example shown, then you must use `.values()`.

In fact, if you forget to use this method, you will find yourself iterating through the **keys** of your Python kwargs dictionary instead, like in the following example:

Python`concatenate_keys.py`

`def concatenate(**kwargs):     result = ""     # Iterating over the keys of the Python kwargs dictionary     for arg in kwargs:         result += arg     return result  print(concatenate(a="Real", b="Python", c="Is", d="Great", e="!"))`

Now, if you try to execute this example, you’ll notice the following output:

Shell

`$ python concatenate_keys.py abcde`

As you can see, if you don’t specify `.values()`, your function will iterate over the keys of your Python kwargs dictionary, returning the wrong result.

[Remove ads](https://realpython.com/account/join/)

## Ordering Arguments in a Function[](https://realpython.com/python-kwargs-and-args/#ordering-arguments-in-a-function "Permanent link")

Now that you have learned what `*args` and `**kwargs` are for, you are ready to start writing functions that take a varying number of input arguments. But what if you want to create a function that takes a changeable number of both positional _and_ named arguments?

In this case, you have to bear in mind that **order counts**. Just as non-default arguments have to precede default arguments, so `*args` must come before `**kwargs`.

To recap, the correct order for your parameters is:

1. Standard arguments
2. `*args` arguments
3. `**kwargs` arguments

For example, this function definition is correct:

Python`correct_function_definition.py`

`def my_function(a, b, *args, **kwargs):     pass`

The `*args` variable is appropriately listed before `**kwargs`. But what if you try to modify the order of the arguments? For example, consider the following function:

Python`wrong_function_definition.py`

`def my_function(a, b, **kwargs, *args):     pass`

Now, `**kwargs` comes before `*args` in the function definition. If you try to run this example, you’ll receive an error from the interpreter:

Shell

`$ python wrong_function_definition.py   File "wrong_function_definition.py", line 2     def my_function(a, b, **kwargs, *args):                                     ^ SyntaxError: invalid syntax`

In this case, since `*args` comes after `**kwargs`, the Python interpreter throws a [`SyntaxError`](https://realpython.com/invalid-syntax-python/).

## Unpacking With the Asterisk Operators: `*` & `**`[](https://realpython.com/python-kwargs-and-args/#unpacking-with-the-asterisk-operators "Permanent link")

You are now able to use `*args` and `**kwargs` to define Python functions that take a varying number of input arguments. Let’s go a little deeper to understand something more about the **unpacking operators**.

The single and double asterisk unpacking operators were introduced in Python 2. As of the 3.5 release, they have become even more powerful, thanks to [PEP 448](https://www.python.org/dev/peps/pep-0448/). In short, the unpacking operators are operators that unpack the values from iterable objects in Python. The single asterisk operator `*` can be used on any iterable that Python provides, while the double asterisk operator `**` can only be used on dictionaries.

Let’s start with an example:

Python`print_list.py`

`my_list = [1, 2, 3] print(my_list)`

This code defines a list and then prints it to the standard output:

Shell

`$ python print_list.py [1, 2, 3]`

Note how the list is printed, along with the corresponding brackets and commas.

Now, try to prepend the unpacking operator `*` to the name of your list:

Python`print_unpacked_list.py`

`my_list = [1, 2, 3] print(*my_list)`

Here, the `*` operator tells `print()` to unpack the list first.

In this case, the output is no longer the list itself, but rather _the content_ of the list:

Shell

`$ python print_unpacked_list.py 1 2 3`

Can you see the difference between this execution and the one from `print_list.py`? Instead of a list, `print()` has taken three separate arguments as the input.

Another thing you’ll notice is that in `print_unpacked_list.py`, you used the unpacking operator `*` to call a function, instead of in a function definition. In this case, `print()` takes all the items of a list as though they were single arguments.

You can also use this method to call your own functions, but if your function requires a specific number of arguments, then the iterable you unpack must have the same number of arguments.

To test this behavior, consider this script:

Python`unpacking_call.py`

`def my_sum(a, b, c):     print(a + b + c)  my_list = [1, 2, 3] my_sum(*my_list)`

Here, `my_sum()` explicitly states that `a`, `b`, and `c` are required arguments.

If you run this script, you’ll get the sum of the three numbers in `my_list`:

Shell

`$ python unpacking_call.py 6`

The 3 elements in `my_list` match up perfectly with the required arguments in `my_sum()`.

Now look at the following script, where `my_list` has 4 arguments instead of 3:

Python`wrong_unpacking_call.py`

`def my_sum(a, b, c):     print(a + b + c)  my_list = [1, 2, 3, 4] my_sum(*my_list)`

In this example, `my_sum()` still expects just three arguments, but the `*` operator gets 4 items from the list. If you try to execute this script, you’ll see that the Python interpreter is unable to run it:

Shell

`$ python wrong_unpacking_call.py Traceback (most recent call last):   File "wrong_unpacking_call.py", line 6, in <module>     my_sum(*my_list) TypeError: my_sum() takes 3 positional arguments but 4 were given`

When you use the `*` operator to unpack a list and pass arguments to a function, it’s exactly as though you’re passing every single argument alone. This means that you can use multiple unpacking operators to get values from several lists and pass them all to a single function.

To test this behavior, consider the following example:

Python`sum_integers_args_3.py`

`def my_sum(*args):     result = 0     for x in args:         result += x     return result  list1 = [1, 2, 3] list2 = [4, 5] list3 = [6, 7, 8, 9]  print(my_sum(*list1, *list2, *list3))`

If you run this example, all three lists are unpacked. Each individual item is passed to `my_sum()`, resulting in the following output:

Shell

`$ python sum_integers_args_3.py 45`

There are other convenient uses of the unpacking operator. For example, say you need to split a list into three different parts. The output should show the first value, the last value, and all the values in between. With the unpacking operator, you can do this in just one line of code:

Python`extract_list_body.py`

`my_list = [1, 2, 3, 4, 5, 6]  a, *b, c = my_list  print(a) print(b) print(c)`

In this example, `my_list` contains 6 items. The first variable is assigned to `a`, the last to `c`, and all other values are packed into a new list `b`. If you run the [script](https://realpython.com/run-python-scripts/), `print()` will show you that your three variables have the values you would expect:

Shell

`$ python extract_list_body.py 1 [2, 3, 4, 5] 6`

Another interesting thing you can do with the unpacking operator `*` is to split the items of any iterable object. This could be very useful if you need to merge two lists, for instance:

Python`merging_lists.py`

`my_first_list = [1, 2, 3] my_second_list = [4, 5, 6] my_merged_list = [*my_first_list, *my_second_list]  print(my_merged_list)`

The unpacking operator `*` is prepended to both `my_first_list` and `my_second_list`.

If you run this script, you’ll see that the result is a merged list:

Shell

`$ python merging_lists.py [1, 2, 3, 4, 5, 6]`

You can even merge two different dictionaries by using the unpacking operator `**`:

Python`merging_dicts.py`

`my_first_dict = {"A": 1, "B": 2} my_second_dict = {"C": 3, "D": 4} my_merged_dict = {**my_first_dict, **my_second_dict}  print(my_merged_dict)`

Here, the iterables to merge are `my_first_dict` and `my_second_dict`.

Executing this code outputs a merged dictionary:

Shell

`$ python merging_dicts.py {'A': 1, 'B': 2, 'C': 3, 'D': 4}`

Remember that the `*` operator works on _any_ iterable object. It can also be used to unpack a [string](https://realpython.com/python-strings/):

Python`string_to_list.py`

`a = [*"RealPython"] print(a)`

In Python, strings are iterable objects, so `*` will unpack it and place all individual values in a list `a`:

Shell

`$ python string_to_list.py ['R', 'e', 'a', 'l', 'P', 'y', 't', 'h', 'o', 'n']`

The previous example seems great, but when you work with these operators it’s important to keep in mind the seventh rule of [_The Zen of Python_](https://realpython.com/zen-of-python/) by Tim Peters: _Readability counts_.

To see why, consider the following example:

Python`mysterious_statement.py`

`*a, = "RealPython" print(a)`

There’s the unpacking operator `*`, followed by a variable, a comma, and an assignment. That’s a lot packed into one line! In fact, this code is no different from the previous example. It just takes the string `RealPython` and assigns all the items to the new list `a`, thanks to the unpacking operator `*`.

The comma after the `a` does the trick. When you use the unpacking operator with variable assignment, Python requires that your resulting variable is either a list or a tuple. With the trailing comma, you have defined a tuple with only one named variable, `a`, which is the list `['R', 'e', 'a', 'l', 'P', 'y', 't', 'h', 'o', 'n']`.

Where's the tuple?Show/Hide

While this is a neat trick, many Pythonistas would not consider this code to be very readable. As such, it’s best to use these kinds of constructions sparingly.

[Remove ads](https://realpython.com/account/join/)

## Python Mutable Data structures

A data structure is a way of organizing data in computer memory. Built-in data structures in Python can be divided into: **mutable** and **immutable**. Mutable can change their elements **lists**, **dictionaries**, and **sets**. Immutable cannot be modified after their creation ie. **tuple**.

Different Python third-party packages implement their own data structures, like [DataFrames](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html) and [Series](https://pandas.pydata.org/docs/reference/api/pandas.Series.html?highlight=series#pandas.Series) in `pandas` or [arrays](https://numpy.org/doc/stable/reference/generated/numpy.array.html) in `NumPy`. 

### Lists
**dynamic mutable arrays** which hold an **ordered** collection of items of any type. The order of insertion is retained.

Pros
- easy to make tables (list of lists)
- easy collection of data or nested ds

Cons
- slow for math operations (use NumPy instead)
- more disk space

```python
l2 = [1, 2, "3", 4] 
l3 = list(any_iter)

# slice list from ith index
l5 = l2[2:]

# change
l1.append(5)
l1.remove(2) #remove 2 if present
```


### Dictionaries
**mutable** data structures that contain a collection of unique **keys** and their mapped **values**.
Later values override previous for a key. Remember insertion order for 3.6.0+

```python
d1 = {"a": 1, "b": 2}
print(d1["a"])

d4 = dict([["one", 1], ["two", 2]])
```

Pros : O(1) time access
Cons : Large space

### Sets
mutable dynamic collections of **immutable unique** elements.
Pros
- fast membership check
Cons : no indexing, no order maintained

```python
s1 = {1,2,3}
s2 = set([1, 2, 3, 4])
```

#### Examples

To create a set, we can use either curly brackets (`{}`) or the `set()` constructor. Do not confuse sets with dictionaries (which also use curly brackets), as sets do not contain `key:value` pairs. Note, though, that like with dictionary keys, only immutable data structures or types are allowed as set elements. This time, let's directly create populated sets:

```python
# Create a set using curly brackets
s1 = {1, 2, 3}

# Create a set using the set() constructor


# Print out sets
print(f"Set s1: {s1}")
print(f"Set s2: {s2}")
```

```text
Set s1: {1, 2, 3}
Set s2: {1, 2, 3, 4}
```

In the second example, we used an **iterable** (such as a list) to create a set. However, if we used lists as set elements, Python would throw an error. Why do you think it happens? **Tip**: read the definition of sets.

To practice, you can try using other data structures to create a set.

As with their math counterparts, we can perform certain operations on our sets. For example, we can create a **union** of sets, which basically means merging two sets together. However, if two sets have two or more identical values, the resulting set will contain only one of these values. There are two ways to create a union: either with the `union()` method or with the vertical bar (`|`) operator. Let's make an example:

```python
# Create two new sets
names1 = set(["Glory", "Tony", "Joel", "Dennis"])
names2 = set(["Morgan", "Joel", "Tony", "Emmanuel", "Diego"])

# Create a union of two sets using the union() method
names_union = names1.union(names2)

# Create a union of two sets using the | operator
names_union = names1 | names2

# Print out the resulting union
print(names_union)
```

```text
{'Glory', 'Dennis', 'Diego', 'Joel', 'Emmanuel', 'Tony', 'Morgan'}
```

In the above union, we can see that `Tony` and `Joel` appear only once, even though we merged two sets.

Next, we may also want to find out which names appear in both sets. This can be done with the `intersection()` method or the ampersand (`&`) operator.

```python
# Intersection of two sets using the intersection() method
names_intersection = names1.intersection(names2)

# Intersection of two sets using the & operator
names_intersection = names1 & names2

# Print out the resulting intersection
print(names_intersection)
```

```text
{'Joel', 'Tony'}
```

`Joel` and `Tony` appear in both sets; thus, they are returned by the set intersection.

The last example of set operations is the difference between two sets. In other words, this operation will return all the elements that are present in the first set, but not in the second one. We can use either the `difference()` method or the minus sign (`-`):

```python
# Create a set of all the names present in names1 but absent in names2 with the difference() method
names_difference = names1.difference(names2)

# Create a set of all the names present in names1 but absent in names2 with the - operator
names_difference = names1 - names2

# Print out the resulting difference
print(names_difference)
```

```text
{'Dennis', 'Glory'}
```

What would happen if you swapped the positions of the sets? Try to predict the result before the attempt.

There are other operations that can be used in sets. For more information, refer to [this tutorial](https://www.dataquest.io/blog/tutorial-everything-you-need-to-know-about-python-sets/), or [Python documentation](https://docs.python.org/3/library/stdtypes.html#set).

Finally, as a bonus, let's compare how fast using sets is, when compared to lists, for checking the existence of an element within them.

```python
import time

def find_element(iterable):
    """Find an element in range 0-4999 (included) in an iterable and pass."""
    for i in range(5000):
        if i in iterable:
            pass

# Create a list and a set
s = set(range(10000000))

l = list(range(10000000))

start_time = time.time()
find_element(s) # Find elements in a set
print(f"Finding an element in a set took {time.time() - start_time} seconds.")

start_time = time.time()
find_element(l) # Find elements in a list
print(f"Finding an element in a list took {time.time() - start_time} seconds.")
```

```text
Finding an element in a set took 0.00016832351684570312 seconds.
Finding an element in a list took 0.04723954200744629 seconds.
```

It is obvious that using sets is considerably faster than using lists. This difference will increase for larger sets and lists.

### Tuples

Tuples are almost identical to lists, so they contain an ordered collection of elements, except for one property: they are **immutable**. We would use tuples if we needed a data structure that, once created, cannot be modified anymore. Furthermore, tuples can be used as dictionary keys if all the elements are immutable.

Other than that, tuples have the same properties as lists. To create a tuple, we can either use round brackets (`()`) or the `tuple()` constructor. We can easily transform lists into tuples and vice versa (recall that we created the list `l4` from a tuple).

The **pros of tuples** are:

- They are immutable, so once created, we can be sure that we won't change their contents by mistake.
- They can be used as dictionary keys if all their elements are immutable.

The **cons of tuples** are:

- We cannot use them when we have to work with modifiable objects; we have to resort to lists instead.
- Tuples cannot be copied.
- They occupy more memory than lists.

#### Examples

Let's take a look at some examples:

```python
# Create a tuple using round brackets
t1 = (1, 2, 3, 4)

# Create a tuple from a list the tuple() constructor
t2 = tuple([1, 2, 3, 4, 5])

# Create a tuple using the tuple() constructor
t3 = tuple([1, 2, 3, 4, 5, 6])

# Print out tuples
print(f"Tuple t1: {t1}")
print(f"Tuple t2: {t2}")
print(f"Tuple t3: {t3}")
```

```text
Tuple t1: (1, 2, 3, 4)
Tuple t2: (1, 2, 3, 4, 5)
Tuple t3: (1, 2, 3, 4, 5, 6)
```

Is it possible to create tuples from other data structures (i.e., sets or dictionaries)? Try it for practice.

Tuples are immutable; thus, we cannot change their elements once they are created. Let's see what happens if we try to do so:

```python
# Try to change the value at index 0 in tuple t1
t1[0] = 1
```

```text
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

It is a `TypeError`! Tuples do not support item assignments because they are immutable. To solve this problem, we can convert this tuple into a list.

However, we can access elements in a tuple by their indices, like in lists:

```python
# Print out the value at index 1 in the tuple t2
print(f"The value at index 1 in t2 is {t2[1]}.")
```

```text
The value at index 1 in t2 is 2.
```

Tuples can also be used as dictionary keys. For example, we may store certain elements and their consecutive indices in a tuple and assign values to them:

```python
# Use tuples as dictionary keys
working_hours = {("Rebecca", 1): 38, ("Thomas", 2): 40}
```

If you use a tuple as a dictionary key, then the tuple must contain immutable objects:

```python
# Use tuples containing mutable objects as dictionary keys
working_hours = {(["Rebecca", 1]): 38, (["Thomas", 2]): 40}
```

```
---------------------------------------------------------------------------

TypeError                                 Traceback (most recent call last)

Input In [20], in ()
      1 # Use tuples containing mutable objects as dictionary keys
----> 2 working_hours = {(["Rebecca", 1]): 38, (["Thomas", 2]): 40}

TypeError: unhashable type: 'list'
```

We get a `TypeError` if our tuples/keys contain mutable objects (lists in this case).

## Conclusions

Let's wrap up what we have learned from this tutorial:

- Data structure is a fundamental concept in programming, which is required for easily storing and retrieving data.
- Python has four main data structures split between mutable (lists, dictionaries, and sets) and immutable (tuples) types.
- Lists are useful to hold a heterogeneous collection of related objects.
- We need dictionaries whenever we need to link a key to a value and to quickly access some data by a key, like in a real-world dictionary.
- Sets allow us to perform operations, such as intersection or difference, on them; thus, they are useful for comparing two sets of data.
- Tuples are similar to lists, but are immutable; they can be used as data containers that we do not want to modify by mistake.