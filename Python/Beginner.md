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

|Function|Description|
|---|---|
|`bytearray()`|Creates and returns an object of the [`bytearray`](https://realpython.com/python-strings/#bytearray-objects) class|
|`bytes()`|Creates and returns a [`bytes`](https://realpython.com/python-strings/#bytes-objects) object (similar to `bytearray`, but immutable)|
|`dict()`|Creates a [`dict`](https://realpython.com/python-dicts/) object|
|`frozenset()`|Creates a [`frozenset`](https://realpython.com/python-sets/#frozen-sets) object|
|`list()`|Creates a [`list`](https://realpython.com/python-lists-tuples/) object|
|`object()`|Creates a new featureless object|
|`set()`|Creates a [`set`](https://realpython.com/python-sets/) object|
|`tuple()`|Creates a [`tuple`](https://realpython.com/python-lists-tuples/) object|

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

|                                                                  |
| ---------------------------------------------------------------- |
| `()`                                                             |
| `**`                                                             |
| `+x`  `-x`  `~x`                                                 |
| `*`  `/`  `//`  `%`                                              |
| `+`  `-`                                                         |
| `<<`  `>>`                                                       |
| `&`                                                              |
| `^`                                                              |
| `\|`                                                             |
| `==`  `!=`  `>`  `>=`  `<`  `<=`  `is`  `is not`  `in`  `not in` |
| `not`                                                            |
| `and`                                                            |
| `or`                                                             |

In Python, a decorator is a design pattern that allows you to modify the functionality of a [function](https://www.programiz.com/python-programming/function) by wrapping it in another function.

The outer function is called the decorator, which takes the original function as an argument and returns a modified version of it.

---

## Prerequisites for learning decorators

Before we learn about decorators, we need to understand a few important concepts related to Python functions. Also, remember that everything in Python is an [object](https://www.programiz.com/python-programming/class#objects), even functions are objects.

### Nested Function

We can include one function inside another, known as a nested function. For example,

```
def outer(x):
    def inner(y):
        return x + y
    return inner

add_five = outer(5)
result = add_five(6)
print(result)  # prints 11

# Output: 11
```

[Run Code](https://www.programiz.com/python-programming/online-compiler)

Here, we have created the `inner()` function inside the `outer()` function.

### Pass Function as Argument

We can pass a function as an argument to another function in Python. For Example,

```
def add(x, y):
    return x + y

def calculate(func, x, y):
    return func(x, y)

result = calculate(add, 4, 6)
print(result)  # prints 10
```

[Run Code](https://www.programiz.com/python-programming/online-compiler)

**Output**

10

In the above example, the `calculate()` function takes a function as its argument. While calling `calculate()`, we are passing the `add()` function as the argument.

In the `calculate()` function, arguments: `func`, `x`, `y` become `add`, `4`, and `6` respectively.

And hence, `func(x, y)` becomes `add(4, 6)` which returns **10**.

### Return a Function as a Value

In Python, we can also return a function as a return value. For example,

```
def greeting(name):
    def hello():
        return "Hello, " + name + "!"
    return hello

greet = greeting("Atlantis")
print(greet())  # prints "Hello, Atlantis!"

# Output: Hello, Atlantis!
```

[Run Code](https://www.programiz.com/python-programming/online-compiler)

In the above example, the `return hello` statement returns the inner `hello()` function. This function is now assigned to the greet variable.

That's why, when we call `greet()` as a function, we get the output.

## Python Decorators

As mentioned earlier, A Python decorator is a function that takes in a function and returns it by adding some functionality.

In fact, any object which implements the special `__call__()` method is termed callable. So, in the most basic sense, a decorator is a callable that returns a callable.

Basically, a decorator takes in a function, adds some functionality and returns it.

```
def make_pretty(func):
    def inner():
        print("I got decorated")
        func()
    return inner


def ordinary():
    print("I am ordinary")

# Output: I am ordinary
```

[Run Code](https://www.programiz.com/python-programming/online-compiler)

Here, we have created two functions:

- `ordinary()` that prints `"I am ordinary"`
- `make_pretty()` that takes a function as its argument and has a nested function named `inner()`, and returns the inner function.

We are calling the `ordinary()` function normally, so we get the output `"I am ordinary"`. Now, let's call it using the decorator function.

```
def make_pretty(func):
    # define the inner function 
    def inner():
        # add some additional behavior to decorated function
        print("I got decorated")

        # call original function
        func()
    # return the inner function
    return inner

# define ordinary function
def ordinary():
    print("I am ordinary")
    
# decorate the ordinary function
decorated_func = make_pretty(ordinary)

# call the decorated function
decorated_func()
```

[Run Code](https://www.programiz.com/python-programming/online-compiler)

**Output**

I got decorated
I am ordinary

In the example shown above, `make_pretty()` is a decorator. Notice the code,

```
decorated_func = make_pretty(ordinary)
```

- We are now passing the `ordinary()` function as the argument to the `make_pretty()`.
- The `make_pretty()` function returns the inner function, and it is now assigned to the decorated_func variable.

```
decorated_func()
```

Here, we are actually calling the `inner()` function, where we are printing

### @ Symbol With Decorator

Instead of assigning the function call to a [variable](https://www.programiz.com/python-programming/variables-constants-literals), Python provides a much more elegant way to achieve this functionality using the `@` symbol. For example,

```
def make_pretty(func):

    def inner():
        print("I got decorated")
        func()
    return inner

@make_pretty
def ordinary():
    print("I am ordinary")

ordinary()  
```

[Run Code](https://www.programiz.com/python-programming/online-compiler)

**Output**

I got decorated
I am ordinary

Here, the `ordinary()` function is decorated with the `make_pretty()` decorator using the `@make_pretty` syntax, which is equivalent to calling `ordinary = make_pretty(ordinary)`.

---

## Decorating Functions with Parameters

The above decorator was simple and it only worked with functions that did not have any parameters. What if we had functions that took in parameters like:

```
def divide(a, b):
    return a/b
```

This function has two parameters, `a` and `b`. We know it will give an error if we pass in b as **0**.

Now let's make a decorator to check for this case that will cause the error.

```
def smart_divide(func):
    def inner(a, b):
        print("I am going to divide", a, "and", b)
        if b == 0:
            print("Whoops! cannot divide")
            return

        return func(a, b)
    return inner

@smart_divide
def divide(a, b):
    print(a/b)

divide(2,5)

divide(2,0)
```

[Run Code](https://www.programiz.com/python-programming/online-compiler)

**Output**

I am going to divide 2 and 5
0.4
I am going to divide 2 and 0
Whoops! cannot divide

Here, when we call the `divide()` function with the arguments **(2,5)**, the `inner()` function defined in the `smart_divide()` decorator is called instead.

This `inner()` function calls the original `divide()` function with the arguments **2** and **5** and returns the result, which is **0.4**.

Similarly, When we call the `divide()` function with the arguments (**2,0)**, the `inner()` function checks that `b` is equal to **0** and prints an error message before returning `None`.

---

## Chaining Decorators in Python

Multiple decorators can be chained in Python.

To chain decorators in Python, we can apply multiple decorators to a single function by placing them one after the other, with the most inner decorator being applied first.

```
def star(func):
    def inner(*args, **kwargs):
        print("*" * 15)
        func(*args, **kwargs)
        print("*" * 15)
    return inner


def percent(func):
    def inner(*args, **kwargs):
        print("%" * 15)
        func(*args, **kwargs)
        print("%" * 15)
    return inner


@star
@percent
def printer(msg):
    print(msg)

printer("Hello")
```

[Run Code](https://www.programiz.com/python-programming/online-compiler)

**Output**

***************
%%%%%%%%%%%%%%%
Hello
%%%%%%%%%%%%%%%
***************

The above syntax of,

```
@star
@percent
def printer(msg):
    print(msg)
```

is equivalent to

```
def printer(msg):
    print(msg)
printer = star(percent(printer))
```

The order in which we chain decorators matter. If we had reversed the order as,

```
@percent
@star
def printer(msg):
    print(msg)
```
