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


##### Unpacking operator
```python
def fn(*args) # args is tuple, for postional args
	return args/args.length

def fn(**kwargs) # is dict, for named args

# can unpack values from any iterable / dict too
# *args must come before **kwargs

#using as spread
print(*'POL') # 'P', 'O', 'L'
```

## Functions

### Decorators
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

## Data structures

A data structure is a way of organizing data in computer memory. Mutable can change their elements **lists**, **dictionaries**, and **sets**. Immutable cannot be modified after their creation ie. **tuple**.

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
l3 = list(any_tuple)

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

# math operations
s3 = s1.union(s2) or s1 | s2
s4 = s1.intersection(s2) or s1 & s2
s5 = s1.difference(s2) or s1 - s2
```

```python
#benchmarking
import time
start = time.time()
# do something
end = time.time() - start
```

### Tuples
Tuples are like lists but denote ordered collection of **immutables**. 
Can be used as dictionary keys if all their elements are immutable.
Cons
- can't be copied; huge space

```python
t1 = (1, 2, 3, 4)
t2 = tuple([1, 2, 3, 4, 5])

# Use tuples as dictionary keys
working_hours = {("Rebecca", 1): 38, ("Thomas", 2): 40}
```