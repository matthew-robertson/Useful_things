# System Design
## Patterns
1. I don't want to talk about every pattern I ever see, so here's the official online version of [Game Programming Patterns](https://gameprogrammingpatterns.com/contents.html).
1. I'm not a huge fan of their site and find their content harder to grok, but [Refactoring Guru](https://refactoring.guru/design-patterns/catalog) also has a decent collection.

## Data Structures & Algorithms
1. [CLRS](https://en.wikipedia.org/wiki/Introduction_to_Algorithms) is the bible here. You own it, take a look sometime.
1. Hashmaps, Dicts, Maps. Whatever the language calls them, they're incredibly powerful tools.
1. BigO runtimes of all the common algorithms can be found [here](https://www.bigocheatsheet.com/). They're not crazy important most of the time, but it's worth keeping the most common ones in your head.

## Building APIs
1. This is more for general APIs than for HTTP ones (Since things like Get/Put/Patch are pretty explicit about what they allow), but "any function that returns a value should not have observable side effects" - Martin Fowler, Refactoring 307.
1. (Idempotent methods](http://restcookbook.com/HTTP%20Methods/idempotency/) (Like GET, PUT, and DELETE), should have the same effect everytime for the same inputs.
1. Safe (Like GET) methods shouldn't change data, just get it.

## Devops

# Software Engineering
1. According to Kent Beck, software is simple if, in order of importance, it: runs all the tests, is DRY, expresses the writer's intent, and minimizes the amount of classes/functions.
1. A cheat sheet for Clean Code's advice can be found [here](https://www.planetgeek.ch/wp-content/uploads/2014/11/Clean-Code-V2.4.pdf).
1. The [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) in particular is something you should keep in mind while writing code.
1. Code is read way more frequently than it's written. Keep that in mind when coming up with names and laying out code.
1. One of the overarching pieces of advice from Clean Code (R. Martin) and Refactoring (M. Fowler and K.Beck) is that refactoring should be done in more discrete steps than you probably think. In combination with good tests, this means that the code is never broken for too long, and you always know what broke it.
1. Comments are often a sign that something should be renamed/refactored. Using the actual code to document the code is less prone to rot.
1. Tests give confidence that errors will be caught. Such confidence means you spend less time deliberating changes, and more time doing them, since mistakes will be caught.
1. The `Migration` method of refactoring functions is especially handy: instead of trying to fix all references to the old function at once, make the old function call your new one so you can change over one at a time. This is especially useful for API changes, and is a good way to deprecate things without losing DRYness.
1. "I will, in fact, claim that the difference between a bad programmer and a good one is whether he considers his code or his data structures more important. Bad programmers worry about the code. Good programmers worry about data structures and their relationships.", [Linus Torvalds](https://lwn.net/Articles/193245/)
1. "... \[Data processing\] is what most programs do most of the time. Sure, there is a computational aspect to programs. There is quality of implementation issues to this, but there is nothing wrong with saying: programs process data. Because data is information. Information systems ... this should be what we are doing, right? ...", [Rich Hickey, Creator of Clojure](https://github.com/matthiasn/talk-transcripts/blob/master/Hickey_Rich/ClojureMadeSimple.md).
1. "Data dominates.  If you've chosen the right data structures and organized things well, the algorithms will almost always be self-evident.  Data structures, not algorithms, are central to programming.", [Rob Pike, creator of Go](https://www.lysator.liu.se/c/pikestyle.html)
1. "I find it wise to be moderately paranoid about collections and I'd rather copy them unncessarily than debug errors due to unexpected modifications.", Martin Fowler, Refactoring P.173
1. Replacing primitives with objects can be really powerful. You've already encountered this with Banned-word-bot development, and you should look out for it more.
1. Prefer immutable data when possible. It'll save you tons of headaches. The Functional Programming people have the right idea. Doing things by value instead of reference is one way to do this. (Martin Fowler, Refactoring, P.252)
1. You'll still want references if you need shared data though.
1. If you're swapping a reference to a value, don't forget to make sure you have a value-based equality method for it.
1. If you're using a switch statement, it's worth considering if you could be doing it polymorphically instead. Especially if the logic is complicated or repeated elsewhere.
1. Consider replacing special case code by making a class/object for the case. I.E.: an `UnkownCustomer` class, that returns the default values, instead of checking everywhere if the customer is unknown then doing special cases.
1. Consider using assertions to enforce assumptions about your data/inputs. Only do this for things that would be programmer error.
1. Like Clean Code and Refactoring both suggest, avoid flag arguments if you can. Better to have explicit functions for both cases. Makes things more clear.
1. Sometimes, like for personal throwaway scripts, clean code isn't worth it. To quote [Larry Hastings](https://www.youtube.com/watch?v=Jd8ulMb6_ls) "Write Sloppy Code, Solve Your Problem".
1. At least in personal automation stuff: fail fast, and fail loudly.
1. Stop writing shell scripts. Let's do more Python or whatever.

# Unix
1. If you want to mass-rename stuff, don't copy the files to somewhere else, just make hardlinks to the backup folder.

# General Language Stuff
1. Languages can be [dynamically](https://en.wikipedia.org/wiki/Scope_(computer_science)#Dynamic_scoping) (Like most shells, a few LISPS or lexically (I.E. Pretty much every language you've used) scoped. Some examples are [here](https://stackoverflow.com/questions/1473111/besides-logo-and-emacs-lisp-what-are-other-pure-dynamically-scoped-languages). If dynamically scoped, this code will print "Heck off": 
```
const greeting = "Hello!";

function printGreeting() {
    console.log(greeting);
}

function greet() {
    const greeting = "Heck off!";
    printGreeting();
}
```

# Language-specific
## Python
1. `defaultdict` is an amazing utility class, that stops dicts from erroring if you access something that doesn't exist.
1. Lambda bodies are executed every run, even if they are one line, like `a = lambda: datetime.now()` will always give you a new time.
1. Python does not optimise tail calls.
1. The [Zen of Python](https://www.python.org/dev/peps/pep-0020/) tells you what's pythonic. Notably: "There should be one—and preferably only one—obvious way to do it.\ Although that way may not be obvious at first unless you're Dutch."
1. Python has a "Walrus operator" `:=` as of 3.8. It's used to assign a variable and return the assigned value. E.G.: `print(a:="Whoopsie")` will print "Whoopsie" and also assign that to `a`. This is similar to assignment in other languages, but more explicit.
1. [PEP 572](https://www.python.org/dev/peps/pep-0572/), which introduced the Walrus Operator, was the one that got Guido to step down as BDFL. Pushback was strong. Maybe this is useful for tricky elifs, or list comprehensions?
1. Python doesn't have a conception of block scope. Most of the obvious things (like classes, functions/lambdas, comprehensions, etc) will create new scope, but [blocks wont](https://stackoverflow.com/a/6167952)
1. `os.system()` won't throw an exception on errors. You need to use `subprocess.run(... check=True)` if you want to deal with errors from it.

## C++

## Javascript
1. `debugger;` functions exactly like putting a breakpoint in the code.
1. There is no difference between `npm install` and `npm i`. It is there because JS people type `npm install` so much.
1. `NaN` always compares to false. Only `isNaN` can compare `NaN`.
1. `const` can't be used in normal for loops, because it can't be reassigned. It *can* however be used in a `for (const foo in bar)`
1. es2019 adds a "pipeline" operator `|>`, which lets you turn `b = bar(foo(a))` into `b = a |> foo |> bar`.
1. `0 === -0` will return true. If you explicitly want to check for negative 0, you should use `Object.is(x, -0)` instead. [Source](https://eslint.org/docs/rules/no-compare-neg-zero)

    ### Typescript
    1. In interface definitions, `?` can be used to mark optional parameters, like 
    ```
    interface Person {
        name: string;
        age: number;
        phone?: string;
    }
    ```
    ### Chai
    1. `expect(y).to.be.closeTo(x, delta)` can be used to test the equality of floats.
    1. `expect([1,2,3]).to.eql([1,2,3])` can be used to test deep equality between arrays
