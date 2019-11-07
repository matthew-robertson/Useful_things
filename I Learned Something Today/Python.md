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