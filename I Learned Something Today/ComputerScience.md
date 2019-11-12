# System Design
## Patterns
1. I don't want to talk about every pattern I ever see, so here's the official online version of [Game Programming Patterns](https://gameprogrammingpatterns.com/contents.html).
1. I'm not a huge fan of their site and find their content harder to grok, but [Refactoring Guru](https://refactoring.guru/design-patterns/catalog) also has a decent collection.
1. The [Command pattern](https://gameprogrammingpatterns.com/command.html) is super useful for letting you manage more complex operations compared to a regular function, by turning a function into an object. One such usage is including an "Undo", another is in games allowing easily rebound controls or NPC actors.

## Data Structures & Algorithms
1. [CLRS](https://en.wikipedia.org/wiki/Introduction_to_Algorithms) is the bible here. You own it, take a look sometime.
1. Hashmaps, Dicts, Maps. Whatever the language calls them, they're incredibly powerful tools.
1. BigO runtimes of all the common algorithms can be found [here](https://www.bigocheatsheet.com/). They're not crazy important most of the time, but it's worth keeping the most common ones in your head.

## Building APIs
1. This is more for general APIs than for HTTP ones (Since things like Get/Put/Patch are pretty explicit about what they allow), but "any function that returns a value should not have observable side effects" - Martin Fowler, Refactoring 307.
1. (Idempotent methods](http://restcookbook.com/HTTP%20Methods/idempotency/) (Like GET, PUT, and DELETE), should have the same effect everytime for the same inputs.
1. Safe (Like GET) methods shouldn't change data, just fetch it.

## Devops

# Software Engineering
1. According to Kent Beck, software is simple if, in order of importance, it: runs all the tests, is DRY, expresses the writer's intent, and minimizes the amount of classes/functions.
1. A cheat sheet for Clean Code's advice can be found [here](https://www.planetgeek.ch/wp-content/uploads/2014/11/Clean-Code-V2.4.pdf).
1. The [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) in particular is something you should keep in mind while writing code.
1. Code is read way more frequently than it's written. Keep that in mind when coming up with names and laying out code.
1. "I will, in fact, claim that the difference between a bad programmer and a good one is whether he considers his code or his data structures more important. Bad programmers worry about the code. Good programmers worry about data structures and their relationships.", [Linus Torvalds](https://lwn.net/Articles/193245/)
1. "... \[Data processing\] is what most programs do most of the time. Sure, there is a computational aspect to programs. There is quality of implementation issues to this, but there is nothing wrong with saying: programs process data. Because data is information. Information systems ... this should be what we are doing, right? ...", [Rich Hickey, Creator of Clojure](https://github.com/matthiasn/talk-transcripts/blob/master/Hickey_Rich/ClojureMadeSimple.md).
1. "Data dominates.  If you've chosen the right data structures and organized things well, the algorithms will almost always be self-evident.  Data structures, not algorithms, are central to programming.", [Rob Pike, creator of Go](https://www.lysator.liu.se/c/pikestyle.html)
1. "I find it wise to be moderately paranoid about collections and I'd rather copy them unncessarily than debug errors due to unexpected modifications.", Martin Fowler, Refactoring P.173
1. Prefer immutable data when possible. It'll save you tons of headaches. The Functional Programming people have the right idea. Doing things by value instead of reference is one way to do this. (Martin Fowler, Refactoring, P.252)
1. You'll still want references if you need shared data though.
1. Like Clean Code and Refactoring both suggest, avoid flag arguments if you can. Better to have explicit functions for both cases. Makes things more clear.
1. Sometimes, like for personal throwaway scripts, clean code isn't worth it. To quote [Larry Hastings](https://www.youtube.com/watch?v=Jd8ulMb6_ls) "Write Sloppy Code, Solve Your Problem".
1. At least in personal automation stuff: fail fast, and fail loudly.
1. Stop writing shell scripts. Let's do more Python or whatever.
1. Referential Transparency (Similar to idempotency, I think), means you'll get the same results for the same inputs. It makes things way easier to work with if you can pull it off.
1. Taking cues from Unity, React, and a number of other things, "Favour object composition over class inheritance". Inheritance isn't bad, but think about if composition will work better in any given case. Especially if you need to vary in more than one way. You can't play the inheritance card twice, to paraphrase Martin Fowler.
1. "For each desired change, make the change easy (warning: this may be hard), then make the easy change." - Kent Beck.
1. Software is more than just code. Its history is important, and keeping it nicely maintained will pay dividends over time.
1. Some (wrong) people will say that they do not need type safety because they use unit tests. These people are wrong. Type checks are way safer than tests can ever hope to be, and eliminate an entire category of tests which no longer need to be written.
1. If you're dealing with data that doesn't fit into memory, you have a few different techniques available to you.
  1. Get more RAM. This is easy, and often times cheaper than your time. Do this first.
  1. Compress the data's memory footprint: change ints to chars, to bools, etc...
  1. Chunk the data: process books one page at a time.
  1. Index the data: Build an easy way to determine what subset to load. The simplest way is to build a well-named file-structure.

# Unix
1. If you want to mass-rename stuff, don't copy the files to somewhere else, just make hardlinks to the backup folder.
1. Running services that use a port below 1024 requires `sudo` (or something like nginx to redirect from one port to another).
1. `grep [options] [pattern] [file]`. Use it. `-r` does recursive, `-i` does case-insensitive, `-n` gives line numbers too.
1. To use `scp` to copy a file to a remote server, use something like `scp -i Identity.pem localFile.ext username@domain:/path/to/copy/to`. To copy from, swap the order.

# Programming Language Theory
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
1. Static typing can do wonders for preventing us from writing impossible or partial functions. consider `head :: [a] -> a` which seems simple enough. The typing doesn't work once you pass in an empty array though, and languages like Haskell can tell us that.
1. Functional Programming style asks you to "Avoid mutation and side effects", which is rad as hell. Why isn't it common? Possibly it just hasn't had enough time.
