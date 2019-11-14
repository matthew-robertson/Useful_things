1. `defaultdict` is an amazing utility class, that stops dicts from erroring if you access something that doesn't exist.
1. Lambda bodies are executed every run, even if they are one line, like `a = lambda: datetime.now()` will always give you a new time.
1. Python does not optimise tail calls.
1. The [Zen of Python](https://www.python.org/dev/peps/pep-0020/) tells you what's pythonic. Notably: "There should be one—and preferably only one—obvious way to do it.\ Although that way may not be obvious at first unless you're Dutch."
1. Python has a "Walrus operator" `:=` as of 3.8. It's used to assign a variable and return the assigned value. E.G.: `print(a:="Whoopsie")` will print "Whoopsie" and also assign that to `a`. This is similar to assignment in other languages, but more explicit.
1. [PEP 572](https://www.python.org/dev/peps/pep-0572/), which introduced the Walrus Operator, was the one that got Guido to step down as BDFL. Pushback was strong. Maybe this is useful for tricky elifs, or list comprehensions?
1. Python doesn't have a conception of block scope. Most of the obvious things (like classes, functions/lambdas, comprehensions, etc) will create new scope, but [blocks wont](https://stackoverflow.com/a/6167952)
1. `os.system()` won't throw an exception on errors. You need to use `subprocess.run(... check=True)` if you want to deal with errors from it.
1. You can declare regular functions inside of other functions, not just lambdas.
1. The format for making lambdas is `x = lambda a, b : a * b`
1. Decorators are a pretty nifty way to wrap functions and have them execute the same functionality. They look roughly like this:
```
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator
def say_whee():
    print("Whee!")
```
1. the `requests` library makes things easy as hecke for dealing with HTTP requests. `request.headers` will get you the dict of headers, `request.json` will get the body of the request.
1. Use `pip install -r fileName` to install every requirement in the file.
1. Build your `requirements.py` using `pip freeze > requirements.py`.
1. `Mypy` is a separate, official, tool, that will do static analysis of your code for type errors. It does this using the type hints.
1. There are a few contract libraries in Python, but since contract testing is so uncommon, the libraries aren't that great. [Contracts](https://github.com/deadpixi/contracts) is the one [Hillel Wayne](https://www.youtube.com/watch?v=MYucYon2-lk) recommends.
1. `functools.partial` lets you build partial functions, which is rad as hell. Eg.:
```
from functools import partial
basetwo = partial(int, base=2)
basetwo('10010') #18
```
1. Magic methods start and end with `__` "dunder". You can implement a few of them to make your objects act like builtins (numbers, lists, dicts, etc..).
 	1. `__str__` tells Python how to print out the class when in a string, instead of using `toString()` everywhere.
	1. `__add__`, and also subtractoin, multiplication, etc... lets you overload the mathematical symbols, which is handy.
	1. `__getitem__` lets you use `d['asdf']` notation, which is rad as hell.
	1. `__len__` lets you overload the built-in `len` to take your class.
1. You can build custom iterables. To do that, you need to do two things:
	1. Implement `__iter__()`, which must return an iterator
	1. Have an iterator, which must implement `__next__()`, which must `raise StopIteration` when there are no more items left.
	1. Your `Iterator` and your `Iterable` can be the same class, in which `__iter__` can just return self.
1. You can use getattr in a cool way to make commandline tools, since `getattr` lets you get a function from a class using a string.


## Typing
1. Python is strongly, dynamically typed. Every object has a definitive type, but variables can hold anything. Note that it's not completely strongly typed: lots of stuff equates to boolean false.
1. Type hints look like `def fun(foo: int) -> int`. To do type checking you need `Mypy`, so `pip install mypy`, then call `mypy file.py` to typecheck.
1. A lot of types can be inferred, which is pretty rad. If it can't, annotating the variable will appease the typechecker. Only do this if needed.
1. Always annotate the function signatures though, because you're good at your job.
1. You *can* use `Union` and `Optional`, but you should avoid it if possible to avoid redundant checks to 
1. importing `Optional` from `typing` lets you do stuff like `get_foo(foo_id: Optional[int])`, which requires either an integer or None.
1. A better option is to use  the `@overload` decorator which does stuff kinda Haskelly:
```
from typing import Optional, overload

@overload
def get_foo(foo_id: None) -> None:
  pass

def get_foo(foo_id: int) -> Foo:
  pass

def get_foo(foo_id: Optional[int]) -> Optional[Foo]:
  if foo_id is None:
    return None
  return Foo(foo_id)
```
1. You can do generic functions using `TypeVar`, like so: `AnyStr = TypeVar('AnyStr', str, bytes)`. This is like unioning `str` and `bytes`, but it [ensures that the typevar binds to the same variable throughout any call to the function](https://youtu.be/pMgmKJyWKn8?t=861s).
1. `AnyStr` itself is actually useful enough that it's built into the `typing` module.
1. The `Any` type exists as an escape hatch. Don't use it if possible, it'll make the typechecker miss things it should catch.
1. `Protocols` let you ducktype, and let you build structural subtypes, but might still be part of `typing_extensions` instead of `typing`. Usage looks like:
```
from typing_extensions import Protocol

class Renderable(Protocol):
  def render(self) -> str: ...
```
1. There are a few ways to tell the typechecker to take a hike:
	1. Using the `Any` type. Not great since you loss all benefits of typecasting, but you know
	1. The `cast` function. `cast(Dict[str, int], get_config_var('my_config'))` lets you lie to the type checker and tell it your object is really of a different type.
	1. `# type: ignore` will have it completely ignore the line. Kinda messed up. Only use this for type checker bugs
	1. `.pyi` files let you "lie to the type checker on industrial scale", they just define interfactes to let you type things, without actually checking the types. It is a bit much, so check out the [talk example](https://youtu.be/pMgmKJyWKn8?t=1299).
1. You can incrementally add typechecking since the typechecker only looks at functions with annotated signatures. [Type-checked Python in the real world](https://youtu.be/pMgmKJyWKn8?t=1480). 
1. `mypy` lets you specify strictness levels, and you should use it as part of your CI process to not regress.
1. Adding types to legacy code can be a nightmare. `monkeytype` was added to help figure out what types things actually are, and create stub files for modules and aplpy them.

## venv
1. Set it up using `pip install virtualenv`.
1. Create a new venv by using `virtualenv envName` where you want it to live.
1. Activate a venv using `source envName/bin/activate`.
1. Deactivate it using `deactivate`

## Code Generators
1. Code generators, like the name implies, will autogenerate code for you. This is good if the code they generate is the code you want.
1. These kinds of things are handy, because they'll take a bit of the pressure off of you to remember to write all the little things, and should be pretty well tested.
1. [dataclasses](https://docs.python.org/3/library/dataclasses.html) are a pretty cool code generator Python 3.8 added to provide a bit more power than `namedTuple`. Notably it'll autogenerate an `__init__` and a `__repr__`, plus equality stuff.
1. Some docs that go into more advanced use cases are [here](https://www.dropbox.com/s/m8pwkkz43qz5pgt/HettingerPycon2018.pdf)

## [Hypothesis](hypothesis.works)
1. Writing a test in hypothesis is similar to regular ones, but includes a `@given` decorator to let you try and sweep the state space.
1. Hypothesis will remember any inputs that previously caused a test to fail, and always try those going forward, which seems wild.
1. You can add parameters to the given as well. For example, this will test lists of integers that contain at least 2 elements:
```
@given(lists(integers(), min_size=2))
def test_fuzz(li):
	bar(li)
```

## Contracts
1. `@require` describes the 
1. You can write a contract in Contracts like so:
```
@require("l must not be empty", lambda args: len(args.l) > 0)
@ensure("result is tail of list", labda args, result: [args.l[0]] + result == args.l)
def tail(l: List[Any]) -> List[Any]:
	output = l[1:]
	return output
```
1. With that tail example, we've actually defined a full spec, so our property test is just:
```
@given(lists(integers(), min_size=1))
def test_tail(li):
	tail(li)
```
