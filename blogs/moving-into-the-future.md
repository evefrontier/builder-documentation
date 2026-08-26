# Moving into the Future: Upgrading to Python 3

The meat of this development blog is covered in the [main news article](https://evefrontier.com/en/news/moving-into-the-future-upgrading-to-python-3). But if you’re still here you may be one who appreciates reading changelogs and delving deep into codebases. If so then oh boy do we have some bonus content for you.

Python has changed quite a bit since 2.7, we thought it would be good to compile a quick overview of the changes that are worth highlighting:

<Callout emoji="💡">
First of all, we’d just like to point you to [the official Python documentation](https://docs.python.org/3/). It’s actually a really good resource for all things related to the language syntax, the standard library, release notes, etc.
Once this is complete the World Deployer container will go offline.
</Callout>

## Core Additions
### Print is a function
The cowboy-debugger's best friend. The print statement has bit the dust and been replaced by the ```print()``` function. The print function works mostly the same as before, with some new options such as separator and suffix customization. The function signature looks like this:
```python copy
def print(*args, sep=' ', end='\n', file=None):
```

### Iterators and views
A handful of things that used to return a list now return an iterator. For example ```map()```, ```filter()```, ```range()``` and ```zip()``` all return an iterator. The ```iterkeys()```, ```itervalues()``` and ```iteritems()``` methods on dict have been removed and the corresponding ```keys()```, ```values()``` and ```items()``` methods now return a view instead (practically the same as an iterator).

This means that you don't have to use utilities such as ```xrange()``` or ```itertools.izip``` anymore. However, this also means that if you need the returned values to be in a list then you have to create it yourself, i.e. ```list(d.values())```.

### Text vs. Data
The Python documentation has a rather dramatic quote regarding this change:

> *Everything you thought you knew about binary data and Unicode has changed.*

<br/ >

However, we can be a bit less dramatic and just say:

> *Don't worry, in practice this doesn't really change anything for you, you're good.*

<br/ >

In short we now have the concepts of text and data. All text, represented with the ```str``` type, is Unicode. Data, represented with the ```bytes``` type, is just a sequence of bytes. You can not directly mix these two together. If you need to represent some text as data (i.e. for serialization) then it must be encoded as bytes with the ```str.encode()``` method. If you need to interpret some data as text, then it must first be decoded from bytes to text with the ```bytes.decode()``` method. That's pretty much it.

Basic string literals are now treated as Unicode text by default (no need to use ```u""``` anymore), and ```b""``` is used for bytes literals.

 You can read a much more detailed breakdown here: [What’s New In Python 3.0](https://docs.python.org/3/whatsnew/3.0.html#text-vs-data-instead-of-unicode-vs-8-bit)

## New Toys

### Type Hinting
You can now annotate functions and variables with type information.

```python copy
age: int = 1

def stringify(num: int) -> str:
    return str(num)
```

These annotations are not enforced at runtime, but they are used by tools such as PyCharm and [mypy](https://github.com/python/mypy) to give you insights and warnings about your code.

This is a huge feature that we highly encourage developers to explore and use. Coding against a well type-annotated API makes everything so much easier for everyone.

There’s no need to go into more detail about this vast topic here, but a good starting point would be to read about it in the mypy documentation: [Type hints cheat sheet - mypy 1.15.0 documentation](https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html)

### Data Classes

Sometimes we create classes that exist mostly to group together a handful of data. It's fairly easy to slap together such a class by just defining an ```__init__()``` method that assigns its arguments to ```self```, or you could use a ```collections.namedtuple```<sup>[1](#footnote1)</sup>. However, sometimes we also need to be able to compare instances of the class, and hash them so they can be used as dictionary keys, and print them nicely for debugging, and order them in a priority queue, etc. In that case you need to implement magic-methods for your class.

Imagine for example that you have a ```User``` class:

```python copy
class User:
    firstname: str
    lastname: str

    def __init__(self, firstname: str, lastname: str) -> None:
        self.firstname = firstname
        self.lastname = lastname

    def __repr__(self) -> str:
        clsname = self.__class__.__name__
        firstname = self.firstname
        lastname = self.lastname
        return f'{clsname}({firstname=}, {lastname=})'

    def __eq__(self, other) -> bool:
        return (
            self.__class__ is other.__class__ and
            self.firstname == other.firstname and
            self.lastname == other.lastname
        )

    def __hash__(self) -> int:
        return hash((self.firstname, self.lastname))
```

If you wanted to add a ```age``` field to this class you'd have to make at least seven changes in the above code. However, the same class written using the new ```@dataclass``` decorator would only require that you make a single change:

```python copy
from dataclasses import dataclass

@dataclass
class User:
    firstname: str
    lastname: str
    age: int = 0  # just add this line and you're done
```

The ```@dataclass``` decorator automatically generates ```__init__```, ```__hash__```, ```__repr__```, etc.  You can somewhat control which methods are generated via arguments to the decorator, which you can read more about here: [dataclasses — Data Classes](https://docs.python.org/3/library/dataclasses.html)

<br/ >
<a id="footnote1"></a>
**[1]**: *Please, don’t use namedtuple for data classes. It’s a tool that’s misunderstood and misused for problems that @dataclass handles far better.*

### Enum
An ```Enum``` is a set of symbolic names bound to unique values. They are similar to global variables, but they offer a more useful ```repr()```, grouping, type-safety, and a few other features.

```python copy
from enum import Enum

class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3
```

We already had a back-ported version of the ```enum``` module in our codebase, but now it's in the standard library as well.

More info here: [enum - Support for enumerations](https://docs.python.org/3/library/enum.html) 

### F-strings
You're probably used to using either %-formatting or the ```str.format()``` method to format strings. However, there's a new kid on the block that allows you to embed expressions inside string literals, using a minimal syntax. Say hello to the f-string:

```python copy
>>> name = "Bob"
>>> f"His name is {name}."
"His name is Bob."
```

For the cowboy-debuggers out there you can add '=' to quickly format the given variable name along with its value:

```python copy
>>> cowboy = True
>>> guns = 2
>>> print(f"{cowboy=}, {guns=}")
cowboy=True, guns=2
```

See also [Lexical analysis](https://docs.python.org/3/reference/lexical_analysis.html#f-strings)

## Niche but Notable Changes

### Pattern matching

If you're familiar with C / Java / JavaScript's ```switch``` or even Rust's ```match``` then this should be pretty familiar.

```python copy
def http_error(status):
    match status:
        case 400:
            return "Bad request"
        case 404:
            return "Not found"
        case 418:
            return "I'm a teapot"
        case _:
            return "Something's wrong with the internet"
```

"But isn't this just a glorified if-else statement?" you might ask. Yes, in the case above it certainly is. However, the rabbit hole does go quite a bit deeper:

```python copy
class Point:
    __match_args__ = ('x', 'y')
    def __init__(self, x, y):
        self.x = x
        self.y = y

match point:
	case Point(x=0, y=0):
		print("Origin")
	case Point(x=0, y=y):
		print(f"Y={y}")
	case Point(x=x, y=0):
		print(f"X={x}")
	case Point():
		print("Somewhere else")
	case _:
		print("Not a point")

match points:
    case []:
        print("No points")
    case [Point(0, 0)]:
        print("The origin")
    case [Point(x, y)]:
        print(f"Single point {x}, {y}")
    case [Point(0, y1), Point(0, y2)]:
        print(f"Two on the Y axis at {y1}, {y2}")
    case _:
        print("Something else")
```

Be careful with the match statement. It’s a powerful tool that comes with some tradeoffs. It increases the indentation one more level compared to an if-else statement, and it's easy to craft some complex code with it, so only use it when it markedly improves code readability vs. if-else statements.

You can read more about match statements here: [More Control Flow Tools](https://docs.python.org/3/tutorial/controlflow.html#tut-match)

### Division

In Python 2 the result type of a division depended on the input types, so f.ex. if you divided a float by an integer the result would be an integer.

In Python 3 division is always performed with floats, regardless of the input types. If you want a truncated result you can use the ```//``` operator instead, or use ```round()```.

### Ordering comparisons are more strict

Python 3.0 has simplified the rules for ordering comparisons. The ordering comparison operators (```<```, ```<=```, ```>=```, ```>```) now raise a TypeError exception when the operands don’t have a meaningful natural ordering. I.e. ```1 < ''```, ```0 > None``` or ```len <= len``` are no longer valid. Additionally, the ```cmp``` argument of ```sorted()``` and ```list.sort()``` has been removed along with the ```cmp()``` function and the associated ```__cmp__()``` method.

### Dictionaries retain the insertion order

Do you remember ```collections.OrderedDict```? Well, that's effectively the default dictionary type in Python 3.

```dict()``` now remembers the insertion order of its keys, and iterating over the keys/values follows that same insertion order. Likewise, ```**kwargs``` and class member order is preserved since both are essentially ```dict()s``` under the hood.

### Nested ```with``` statements

You can now use multiple context managers in a single ```with``` statement, like this:

```python copy
with open("a.txt"), open("b.txt") as a, b:
    ...
```

### The Walrus Operator ```:=```

Honestly, this is a strange one that some people wish hadn’t been added to the language, but it's here to stay, so we need to get familiar with it. Here's an example sighting of this wild beast:

```python copy
while node := node.parent is not None:
    process(node)
```

The walrus operator allows you to use variable assignments as expressions, and thus nest them within other expressions. There are niche cases where this can increase readability given that you know about this operator. However, as is stated in the official documentation (a good indicator to some that this was a bad decision in the first place):

> Try to limit use of the walrus operator to clean cases that reduce complexity and improve readability.

<br/ >

Read more about it here: [What's New In Python 3.8](https://docs.python.org/3/whatsnew/3.8.html#assignment-expressions)

### Underscores as Thousand Separators

This one is just a nice little code-readability feature that can be summarized with this example:

```python copy
amount = 10_000_000.0
addr = 0xCAFE_F00D
flags = 0b_0011_1111_0100_1110
```

See also [PEP 515 - Underscores in Numeric Literals](https://peps.python.org/pep-0515/)

## Advanced Topics

### Metaclasses

The syntax for declaring metaclasses and the semantics for how classes with metaclasses are constructed has been changed drastically. You can read the specifics here: [PEP 3115 - Metaclasses in Python 3000](https://peps.python.org/pep-3115/)

### Positional-only parameters

There is a new function parameter syntax ```/``` to indicate that some function parameters must be specified positionally and cannot be used as keyword arguments. Additionally, any parameters defined after the ```*``` variable parameter must be specified as keyword arguments.

In the following example, parameters *a* and *b* are positional-only, while *c* or *d* can be positional or keyword, and *e* or *f* are required to be keywords:

```python copy
def f(a, b, /, c, d, *, e, f):
    print(a, b, c, d, e, f)
```

More details here: [What's New In Python 3.8](https://docs.python.org/3/whatsnew/3.8.html#positional-only-parameters)

### Simpler customization of class creation

It is now possible to customize subclass creation without using a metaclass. The new ```__init_subclass__``` classmethod is called on the base class whenever a new subclass is created.

```python copy
class PluginBase:
    subclasses = []

    def __init_subclass__(cls, **kwargs):
        super().__init_subclass__(**kwargs)
        cls.subclasses.append(cls)

class Plugin1(PluginBase):
    pass

class Plugin2(PluginBase):
    pass
```

More details here: [What's New In Python 3.6](https://docs.python.org/3/whatsnew/3.6.html#pep-487-simpler-customization-of-class-creation)

### Extended iterable unpacking

The iterable unpacking syntax has been slightly changed to allow specifying a “catch-all” name which will be assigned a list of all items not assigned to a “regular” name:

```python copy
>>> a, *b, c = range(5)
>>> a
0
>>> c
4
>>> b
[1, 2, 3]
```

More details here: [PEP 3132 - Extended Iterable Unpacking](https://peps.python.org/pep-3132/) 

### No more implicit relative imports

In Python 2 you were allowed to [implicitly import modules with a path relative to the importing module](https://docs.python.org/2.7/tutorial/modules.html?highlight=implicit%20relative#intra-package-references). In Python 3 all import paths must be explicit, meaning that a path starting with a ```.``` is relative and anything else is absolute. If you don't understand what any of that means, don't worry, it was just an old foot-gun that's now been removed.

### Exception context

The inspection and reporting capabilities of exception objects has been significantly improved. Exception instances support new ```__context__```, ```__cause__``` and ```__traceback__``` properties that help give more information about exceptions that occur during the handling of other exceptions.

During the handling of one exception (exception A), it is possible that another exception (exception B) may occur. In Python 2, if this happened, exception B was propagated outward and exception A was lost. In order to debug the problem, it would be useful to know about both exceptions. The exception’s ```__context__``` attribute retains this information automatically.

Additionally, it can sometimes be useful for an exception handler to intentionally re-raise an exception, either to provide extra information or to translate an exception to another type. The ```__cause__``` attribute provides an explicit way to record the direct cause of an exception.

We’ve got some fancy new syntax to set the ```__cause__``` property:

```python copy
raise EXCEPTION from CAUSE
```

which is equivalent to:

```python copy
exc = EXCEPTION
exc.__cause__ = CAUSE
raise exc
```

And finally the ```__traceback__``` property simply stores the traceback object whereas in Python 2 you had to retrieve the traceback from ```sys.exc_traceback```.

You can read more about exceptions here: [Built-in Exceptions](https://docs.python.org/3/library/exceptions.html)