1. Tests give confidence that errors will be caught. Such confidence means you spend less time deliberating changes, and more time doing them, since mistakes will be caught.
1. Consider using assertions to enforce assumptions about your data/inputs. Only do this for things that would be programmer error. These are like runtime typechecks.
1. [Hillel Wayne's talk about generative testing/contracts and accompanying Q&A](hillelwayne.com/talks/beyond-unit-tests)
1. Write unit tests. They're the basis of all your testing.
1. Part of the problem with things like unit, integration, or acceptance testing is that they're "automanual": you have to manually write every test and think of all the edge-cases
1. Property-based, "Invariant", or contract-based testing should get around this in our testing.
1. Integration tests are way harder,
1. As cool as static typing is, it can only guarantee so much (hard to guarantee `tail([a,...])` returns `[...]` for example. Use asserts as well.
1. When using asserts for testing, you generally want to test a pre-condition (something about the inputs), and something about the output (post-condition). This is a contract.
1. If you've achieved a full specification using contracts, your property test just has to call the function.
1. You can't always do that though, usually because of side-effects like mutation.
1. [Contracts + property tests = integration tests](https://youtu.be/MYucYon2-lk?t=1335). 
1. A short list of wild test topics:
	1. Test Generators (from Clojure)
	1. Contract Inheritance (from Eiffel)
	1. Global Contracts (from Ada)
	1. Static Verifiication (from Dafny)
	1. Abstract Base Classes Metatyping (from Python potentially)
1. Check out [a11y-automation](https://a11y-automation.dev) for a list of possible accessibility violations and how they might be tested.
