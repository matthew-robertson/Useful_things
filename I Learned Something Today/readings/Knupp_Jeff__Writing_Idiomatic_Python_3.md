Impression:
My first time reading about Python kind of generally, as opposed to PyCon talks or language-agnostic software development books. Having written Python for quite a while, this was shorter and more basic than I was hoping for, but I did learn a handful of things from it. Not bad for an easy afternoon's read.

Quotes/Annotations:
1. "Avoid repeating variable name in compound if statement. ...`name = "Tom" \n is_generic_name = name in ("Tom", "Dick", "Harry")`"
1. "The rules regarding 'truthiness' are reasonably straightforward. All of the following are considered False: `None`, `False`, 0 (for numeric types), empty sequences, empty dictionaries, a value of `0` or `False` when `__len__` or `__nonzero__` is called. Everything else is considered True. The last condition... allows you to define how 'truthiness' should work for any class you create."
1. "if statements in Python make use of 'truthiness' implicitly, and you should too."
1. "Use if and else as a short ternary operator replacement.... `value = 1 if foo else 0`"
1. "Use the enumerate function in loops instead of creating a 'index' variable... `for index, element in enumerate(my_container):`"
1. "One of the lesser known facts about Python's for loop is that it can include an else clause. The else clause is executed after the iterator is exhausted, unless the loop was ended prematurely due to a break statement.... This obviates the need for conditional flags in a loop solely used to determine if some condition held."
1. "Avoid using a mutable object as the default value for a function argument. When the Python Interpreter encounters a function definition, default arguments are evaluated to determine their value. This evaluation, however, occurs only once. *Calling* the function does not trigger another evaluation of the arguments. Since the computed value is used in all subsequent calls to the function, using a mutable object as a default value often yields unintended results."
1. "In Python, everything is an object, and this includes functions. Functions can be assigned to variables, passed to other functions, and returned as results from function calls."
1. "In many languages, exceptions are reserved for truly exceptional cases. For example, a function that takes a file name as an argument and performs some calculations on the file's contents probably shouldn't throw an exception if the file is not found. That's not too 'exceptional'; it's probably a reasonably common occurrence. If, however, the file system itself were unavailable, raising an exception makes sense."
1. "Python takes a different view [toward exceptions]. Exceptions can be found in almost every popular third-party package, and the Python standard library makes liberal use of them. In face, exceptions are built into fundamental parts of the language itself. For example, did you know that any time you use a for loop in Python, you're using exceptions? ...Have you ever wondered how for lops know when to stop? For things like lists that have an easily determined length the question seems trivial. But what about generators, which could produce values ad infinitum?... The answer may surprise you: the [iterator] raises a `StopIteration` exception."
1. "Code written [to avoid exceptions by pre-checking all conditions] is said to be written in a 'Look Before You Leap (YBYL)' style.... It's rarely possible to anticipate everything that could go wrong. What's more, as the Python documentation astutely points out, code written in this style can fail badly in a multi-threaded environment. A condition that was true in an if statement may be false by the next line. Alternatively, code written according to the principle, '[It's] Easier to Ask for Forgiveness than Permission' assumes things will go well and catches exceptions if they don't. It puts the code's true purpose front-and-center, increasing clarity.... The error handling code that follows is easier to read you already know the operation that could have failed."
1. "Exceptions have tracebacks and messages for a reason: to aid in debugging when something goes wrong. If you 'swallow' an exception with a bare except clause, you suppress genuinely useful debugging information. If you need to know whenever an exception occurs but don't intend to deal with it (say for logging purposes), add a bare raise to the end of your except."
1. "Almost none of the idioms described in this book are meant to be mechanically followed: use your head."
1. "Use multiple assignment to condense variables all set to the same value... `x=y=z='foo'`"
1. "Avoid using a temporary variable when performing a swap of two values"
```
foo = 'bar'
bar = 'bar'
(foo, bar) = (bar, foo)
```
1. "When applying a simple sequence of transformations on some datum, chaining the calls in a single expression is often more clear than creating a temporary variable for each step of the transformation. Too much chaining, however, can make your code harder to follow. 'No more than three chained functions' is a good rule of thumb."
1. "It's often useful to be able to make use of the ASCII value of a character. It is likewise useful to be able to translate in the 'other direction', from ASCII code to character. Python has two oft-overlooked build-in functions, `chr` and `ord`, that are used to perform these translations."
1. "List comprehensions, when used judiciously, increase clarity in code that builds a list from existing data. This is especially true when elements are both checked for some condition and transformed in some way. There are also (usually) performance benefits to using a list comprehension (or alternately, a generator expression) due to optimization in the cPython interpreter."
1. "Prefer list comprehensions to the built-in `map()` and `filter()` functions."
1. "Use the built-in function `sum` to calculate the sum of a list of values."
1. "Use `all` to determine if all elements of an iterable are `True`."
1. "Use the `*` operator to represent the rest of a list"
```
(first, second, *rest) = some_list
(first, *middle, last) = some_list
(*head, penultimate, last) = some_list
```
1. "Use a dict as a substitute for a `switch...case` statement."
1. "The list comprehension is a well-known Python construct. Less well known is the dict comprehension. Its purpose is identical..."
1. "The set comprehension syntax is relatively new in Python and, therefore, often overlooked. Just as a list can be generated using a list comprehension, a set can be generated using a set comprehension. In fact, the syntax is nearly identical (modulo the enclosing characters)."
1. "Use sets to eliminate duplicate entries from `Iterable` containers."
1. "Use `collections.namedtuple` to make tuple-heavy code more clear... A `namedtuple` is a normal tuple with a few extra capabilities. Most importantly, `namedtuples` give you the ability to access fields by names rather than by index."
1. "In Python, it is possible to 'unpack' data for multiple assignment. Those familiar with LISP may know this as destructuring bind.,, `(animal, name, age) = list_from_csv`"
1. "Use a tuple to return multiple values from a function."
1. "Always use `self` or a `@classmethod` when referring to a class's attributes."
1. "Use `_` as a placeholder for data in a tuple that should be ignored."
1. "`isinstance(object, class-or-object-or-tuple)` is a built-in function that returns `True` if `object` is of the same type of subtype as the second argument. If the second argument is a tuple, the function returns `True` is the object is of the same type of subtype as any of the elements of the tuple.... remember that any class can be used as the second argument"
1. "Before, I hinted that the single and double underscore prefix were more than mere convention. Few developers are aware of the fact that prepending attribute names in a class does actually do something. Prepending a single underscore means that the symbol won't be imported if the 'all' idiom is used. Prepending two underscores to an attribute invokes Python's name mangling... If `Foo` is a class, the definition `def __bar()` will be 'mangled' to `_classname__attributename`."
1. "Use properties to 'future-proof' your class implementation... there's a reason 'getters' and 'setters' exist (in addition to further frustrating Java programmers): you never know when what once was pure data will require calculation instead." 
```
@property
def price(self):
  return self._price * TAX_RATE

@price.setter
def price(self, value):
  self._price = value
```
1. "While `__str__` is used for printing a class in a way that a human can read, `__repr__` is used for *machines* to read. Python's default implementation for `__repr__` is useless, but it would be very difficult to come up with a globally useful (and correct) version. `__repr__` should contain all the information necessary to reconstruct the object, and it should be possible to distinguish between two different instances using `__repr__`. A good rule of thumb is, if possible, `eval(repr(instance))==instance`."
1. "Define `__str__` in a class to show a human-readable representation."
1. "There are a number of classes in the standard library that support or use a context manager. In addition, user defined classes can be easily made to work with a context manager by defining `__enter__` and `__exit__` methods. Functions may be wrapped with context managers through the `contextlib` module."
1. "A list comprehension generates a list object and fills in all of the elements immediately. For large lists, this can be prohibitively expensive. The generator returned by a generator expression, on the other hand, generates each element 'on-demand'.... A logical extension of the way generator expressions work is that you can use them on infinite sequences."
```
for uppercase_name in (name.upper() for nam in get_all_usernames()):
 ...
```
1. "Use all capital letters when declaring global constant values."
1. "Avoid placing multiple statements on a single line."
1. "Writing a 'good' docstring, like writing good documentation in general, takes practice. Following the rules in PEP-257 will help a good deal. Two in particular will get you 90% of the way there: Everything public or exported should have a docstring, and how to properly format them."
1. "The only thing worse than too much documentation is wrong documentation."
1. "While the actual ordering [of import statements] is not as important [as having an ordering], the following is the order recommended by Python's Programming FAQ: standard library modules,third-paty library modules, modules local to the current project. Many choose to arrange the imports in (roughly) alphabetical order."
1. "The Python programming FAQ goes so far as to say, 'Never use relative package imports.'"
1. "Do not use `from foo import *` to import the contents of a module.... But what if you have to import a number of names from the `foo` package? Simple. Make use of the fact that parenthesis can be used to group `import` statements."
1. "When writing libraries, one often needs to determine if a certain package is available on a user's machine. If it is, it is important and used. If not, a fallback package must be used. The idiomatic way to perform this chick is through the use of a try/except block. If an exception is raised, the package does not exist and the fallback package should be imported."
1. "If you have a package with dozens of modules, only one or two of which are meant to be used by client code, then `__init__.py` is your friend. You can import modules from all over you package and make them available via the `init` module. This lets client code refer to classes that may be nested 4 levels deep as if they were in the main module."
1. "Use the `if __name__ == '__main__'` pattern to allow a file to be both imported and run directly."
1. "Often, a Python file is always going to be used as a script, and typing 'python ' is somewhat verbose. It would be nice if we could just run '' which would somehow know to invoke itself with the Python interpreter. On Windows, this happens automatically as `.py` files are associated with the interpreter. On BSD-like systems, by making the line `#! /usr/bin/env python` the first line, the script can be executed directly."
1. "Python scrips should be good shell citizens. It's tempting to jam a bunch of code after the `if __name__ == '__main__'` statement and not return anything. Avoid this temptation. Create a main function that contains the code to be run as a script. Use `sys.exit` in `main` to return error codes if something goes wrong or 0 if everything runs to completion. The only code under the `if __name__ == '__main__ '` statement should call `sys.exit` with the return value of your main function as the parameter."
1. "While there are libraries, both in the Python standard library and third-party, that handle programs with complicated command line options, they're overkill for simple scripts. Instead, simply inspect `sys.argv`."
1. "The functional programming stalwarts that `itertools` provides should be seen as fundamental building blocks. What's more, the documentation for `itertools` has a 'Recipes' section that provides idiomatic implementations of common functional programming constructs, all created using the `itertools` module."
1. "Use functions in the `os.path` module when working with directory paths."
