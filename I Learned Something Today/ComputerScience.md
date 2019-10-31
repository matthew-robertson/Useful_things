# System Design
## Patterns
1. I don't want to talk about every pattern I ever see, so here's the official online version of [Game Programming Patterns](https://gameprogrammingpatterns.com/contents.html).
1. I'm not a huge fan of their site and find their content harder to grok, but [Refactoring Guru](https://refactoring.guru/design-patterns/catalog) also has a decent collection.

## Data Structures & Algorithms
1. CLRS is the bible here. You own it, take a look sometime.
1. Hashmaps, Dicts, Maps, whatever the language calls them, they're incredibly powerful tools.
1. BigO runtimes of all the common algorithms can be found [here](https://www.bigocheatsheet.com/). They're not crazy important most of the time, but it's worth keeping the most common ones in your head.

# Software Engineering
1. According to Kent Beck, software is simple if, in order of importance, it: runs all the tests, is DRY, expresses the writer's intent, and minimizes the amount of classes/functions.
1. A cheat sheet for Clean Code's advice can be found [here](https://www.planetgeek.ch/wp-content/uploads/2014/11/Clean-Code-V2.4.pdf).
1. The [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) in particular is something you should keep in mind while writing code.
1. Code is read way more frequently than it's written. Keep that in mind when coming up with names and laying out code.
1. One of the overarching pieces of advice from Clean Code (R. Martin) and Refactoring (M. Fowler and K.Beck) is that refactoring should be done in more discrete steps than you probably think. In combination with good tests, this means that the code is never broken for too long, and you always know what broke it.
1. Comments are often a sign that something should be renamed/refactored. Using the actual code to document the code is less prone to rot.
1. Tests give confidence that errors will be caught. Such confidence means you spend less time deliberating changes, and more time doing them, since mistakes will be caught.
1. The `Migration` method of refactoring functions is especially handy: instead of trying to fix all references to the old function at once, make the old function call your new one so you can change over one at a time. This is especially useful for API changes, and is a good way to deprecate things without losing DRYness.

# Language-specific
## Python
1. `defaultdict` is an amazing utility class, that stops dicts from erroring if you access something that doesn't exist.
1. Lambda bodies are executed every run, even if they are one line, like `a = lambda: datetime.now()` will always give you a new time.
1. Python does not optimise tail calls.

## C++

## Javascript
1. `debugger;` functions exactly like putting a breakpoint in the code.
1. There is no difference between `npm install` and `npm i`. It is there because JS people type `npm install` so much.
1. `NaN` always compares to false. Only `isNaN` can compare `NaN`.
1. `const` can't be used in normal for loops, because it can't be reassigned. It *can* however be used in a `for (const foo in bar)`
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
