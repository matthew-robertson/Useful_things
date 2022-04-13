1. One of the overarching pieces of advice from Clean Code (R. Martin) and Refactoring (M. Fowler and K.Beck) is that refactoring should be done in more discrete steps than you probably think. In combination with good tests, this means that the code is never broken for too long, and you always know what broke it.
1. Comments are often a sign that something should be renamed/refactored. Using the actual code to document the code is less prone to rot.
1. The `Migration` method of refactoring functions is especially handy: instead of trying to fix all references to the old function at once, make the old function call your new one so you can change over one at a time. This is especially useful for API changes, and is a good way to deprecate things without losing DRYness.
1. Replacing primitives with objects can be really powerful. You've already encountered this with Banned-word-bot development, and you should look out for it more.
1. If you're swapping a reference to a value, don't forget to make sure you have a value-based equality method for it.
1. If you're using a switch statement, it's worth considering if you could be doing it polymorphically instead. Especially if the logic is complicated or repeated elsewhere.
1. Consider replacing special case code by making a class/object for the case. I.E.: an `UnkownCustomer` class, that returns the default values, instead of checking everywhere if the customer is unknown then doing special cases.

## [Using GIT to support SOLID refactoring](https://www.youtube.com/watch?v=3OgbQOsW61Y)
1. It can be hard to figure how to make classes single-purpose, but analyzing commit messages can help you figure out why classes have reason to change, and potentially break them out.
	1. For this to work, you need to have good commit messages, so they're actually helpful.
1. Git can help you recognize violations of the Open/closed principle. If you're adding features, you shouldn't need to change existing code (no red in the diff).
	1. For this to work, you need to have a consistent style so that diffs aren't muddied.
1. The Interface Segregation Principle says you shouldn't depend on things you don't use. You can use git for this by checking the logs to see if you're adding things to mimic other interfaces.
1. The Dependency Inversion Principle can be easily be tracked by Git pre-commit hooks, since you can statically detect references to static classes and things.
1. You can use git logs as well to let you find out which files are changing the most, to see what your worst offenders are for all these things.
