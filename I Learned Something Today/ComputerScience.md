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
1. (Idempotent methods](http://restcookbook.com/HTTP%20Methods/idempotency/) (Like GET, PUT, and DELETE), should have the same effect every time for the same inputs.
1. Safe (Like GET) methods shouldn't change data, just fetch it.

## Microservices
1. Microservices should be independant. That means you *can* choose the "best" tech for each service. Don't though. You should use the same handfuls of things to prevent re-invention and enable knowledge sharing.
1. Prefer separate databases/schemas/whatever for each micro-service to maintain clear ownership lines. 
1. Watch out for dependency versions, especially for in-house frameworks. It's easy to couple micro-services more tightly than you'd expect.
1. Design with the assumption that things will fail. Otherwise you'll "combine the complexity of a microservice architecture with the rigidity of a monolith"
1. Having well-defined tools is good, but you *should* let people upgrade or use clearly better tools.
1. "The key to failure is the hidden monolith." - [David Schmitz](https://youtu.be/X0tjziAQfNQ?t=2396)

## Devops
1. Don't be afraid to revert. You should not be leaving broken code in production if you don't have to.
1. Automate as much as possible, and be comfortable and confident in your CI/CD. It'll eliminate easy mistakes from configuring and deploying things. Here's looking at you, Banned-word-services.
1. Dev machines should look like staging, should look like prod. Otherwise, you'll break things moving from one environment to another. Can't do CD if you don't have something set up well.
1. When launching, newly deploying something to prod, or otherwise expecting a big uptick in users, [make sure your SAAS plans and environment test are set correctly](https://lunchbag.ca/lunch-money-mistakes/).
1. Remember YAGNI: You ain't gonna need it. Don't overexpand and build things you think you "might" want later.

## Scalability
1. Vertical scaling is the easiest. If you're bounded by RAM: throw more money at the machine and pack more ram in it. This is a problem though, because you'll eventually run out of money or hit the state of the art.
1. Horizontal scaling is a little nicer, but involves parallelizing the problem in some way so that many machines can do your work. Some issues with this:
  1. You need to figure out how to load balance, i.e.: distribute the traffic among the backend servers. A decent way is to have a public server that just determines what backend server to use. This means the backend servers can use exclusively private IPs, which is nice.
  1. You need to decide if you have a bunch of services, or a bunch of clones of a monolith. The latter ends up eating up a lot more diskspace, but is more redundant.
1. load balancing strategies:
  1. Round robin: Like tetris piece generation. Use each ip once, then repeat. This can cause a problem since loaded servers will continue to get more hits. If you do it DNS side, there's also the issue of caching.
  1. Metric based: If you can tell how much load a server is under, you could go for the lightest server each time, for example. However, you have to send the user to the same server each time, or you'll break sessions. 

## Infrastructure
1. [Bastion Hosts](https://en.wikipedia.org/wiki/Bastion_host) are a public interface to a private network, allowing a network's defenses to be concentrated in fewer places.
1. Bastion Hosts should have public IPs, not because you need them to reach them (VPNs and things can be used to avoid that), but to decouple the internal network and prod so that if something breaks you can switch to using the public IPs.
1. Set up as much as you cna through automate-able processes. You *will* have to do it again later, and this will prevent both making random mistakes/typos, and forgetting what exactly you did.

## Things to look for in a hosting company
1. SFTP over FTP. Gotta encrypt usernames and passwords.
1. Check that you won't run into geo-blocking issues.
1. Watch out for shared hosts, versus VPS (virtual private server), if you think you'll actually need a decent amount of resources. A major difference is that you get control over the OS with a VPS, where no one else has access to your chunk of hardware.

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
1. "Show me your flowcharts and conceal your tables, and I shall continue to be mystified. Show me your tables, and I usually won't need your flowcharts." - Mythical Man-Month (102).
1. Conway's Law, restated by Yourdon and Constantine: "The structure of any system designed by an organization is isomorphic to the structure of the organization."
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
1. If you need to locate a file you know the name of, use `find .` (replacing with whatever directory you want), with the argument `-name 'fileName'`. `-iname` will make it case-insensitive, `-maxdepth n` lets you set how deep to recurse.
1. "Those that don't remember history are doomed to press Up repeatedly."
1. `alt+b` and `alt+f` move backwards and forwards by a word.
1. `ctrl+a` and `ctrl+e` move to the start/end of the line.
1. `ctrl+r` and `ctrl+s` will search through the history, which is wild.

# General Language Stuff
1. Floating point math, amirite? It means `0.1 + 0.2 != 0.3`, thanks to issues with representing necesarry rounding to store floats/doubles in memory.
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
1. If you find yourself in the unenviable position of writing a lexer/parser for a language. [Don't  use a ReGeX](https://commandcenter.blogspot.com/2011/08/regular-expressions-in-lexing-and.html).

## Functional Programming
1. Functional Programming style asks you to "Avoid mutation and side effects", which is rad as hell. Why isn't it more common? Possibly it just hasn't had enough time.
1. [Persistent Data Structures](https://en.wikipedia.org/wiki/Persistent_data_structure) are versions of arrays, lists, trees, etc... that are 1) immutable, and 2) support a "copy with small change" operation that it does extremely efficiently to avoid slowdown from editing large structures. [Russ Olsen, FP in 40 minutes](https://youtu.be/0if71HOyVjY?t=1355)
1. Atoms in clojure help bridge between functions and state, and function by applying a function using their current value, then updating their value to reflect the result of the function. They get around timing trickiness by detecting if the value changed out from under it, and if so, re-run it.
1. There are a lot of things FP won't help you with, but it *will* prevent threads changing values out from under other threads. It might have the wrong version, but it won't be garbage.


