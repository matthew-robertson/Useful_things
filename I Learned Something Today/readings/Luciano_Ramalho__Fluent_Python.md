Fluent Python

Magic and Dunder The term magic method is slang for special method, but when talking about a specific method like __getitem__, some Python developers take the shortcut of saying “under-under-getitem” which is ambiguous, because the syntax __x has another special meaning.[3] Being precise and pronouncing “under-under-getitem-under-under” is tiresome, so I follow the lead of author and teacher Steve Holden and say “dunder-getitem.” All experienced Pythonistas understand that shortcut. As a result, the special methods are also known as dunder methods.[

We’ve just seen two advantages of using special methods to leverage the Python data model: 
The users of your classes don’t have to memorize arbitrary method names for standard operations (“How to get the number of items? Is it .size(), .length(), or what?”).

It’s easier to benefit from the rich Python standard library and avoid reinventing the wheel, like the random.choice function.
 But

Iteration is often implicit. If a collection has no __contains__ method, the in operator does a sequential scan. Case in point: in works with our FrenchDeck class because it is iterable.

Although FrenchDeck implicitly inherits from object,[5] its functionality is not inherited, but comes from leveraging the data model and composition. By implementing the special methods __len__ and __getitem__, our FrenchDeck behaves like a standard Python sequence, allowing it to benefit from core language features (e.g., iteration and slicing) and from the standard library, as shown by the examples using random.choice, reversed, and sorted. Thanks to composition, the __len__ and __getitem__ implementations can hand off all the work to a list object, self._cards. How

Avoid creating arbitrary, custom attributes with the __foo__ syntax because such names may acquire special meanings in the future, even if they are unused today. Emulating

The first thing to know about special methods is that they are meant to be called by the Python interpreter, and not by you. You don’t write my_object.__len__(). You write len(my_object) and, if my_object is an instance of a user-defined class, then Python calls the __len__ instance method you implemented. But for built-in types like list, str, bytearray, and so on, the interpreter takes a shortcut: the CPython implementation of len() actually returns the value of the ob_size field in the PyVarObject C struct that represents any variable-sized built-in object in memory. This is much faster than calling a method. More often than not, the special method call is implicit. For example, the statement for i in x: actually causes the invocation of iter(x), which in turn may call x.__iter__() if that is available. Normally, your code should not have many direct calls to special methods. Unless you are doing a lot of metaprogramming, you should be implementing special methods more often than invoking them explicitly. The only special method that is frequently called by user code directly is __init__, to invoke the initializer of the superclass in your own __init__ implementation.

you need to invoke a special method, it is usually better to call the related built-in function (e.g., len, iter, str, etc). These built-ins call the corresponding special method, but often provide other services and—for built-in types—are faster than method calls. See, for example, A Closer Look at the iter Function in Chapter 

Note that in our __repr__ implementation, we used %r to obtain the standard representation of the attributes to be displayed. This is good practice, because it shows the crucial difference between Vector(1, 2) and Vector('1', '2')—the latter would not work in the context of this example, because the constructor’s arguments must be numbers, not str. The string returned by __repr__ should be unambiguous and, if possible, match the source code necessary to re-create the object being represented. That is why our chosen representation looks like calling the constructor of the class (e.g., Vector(3, 4)). Contrast __repr__ with __str__, which is called by the str() constructor and implicitly used by the print function. __str__ should return a string suitable for display to end users. If you only implement one of these special methods, choose __repr__, because when no custom __str__ is available, Python will call __repr__ as a fallback.

Type Although Python has a bool type, it accepts any object in a boolean context, such as the expression controlling an if or while statement, or as operands to and, or, and not. To determine whether a value x is truthy or falsy, Python applies bool(x), which always returns True or False. By default, instances of user-defined classes are considered truthy, unless either __bool__ or  __len__ is implemented. Basically, bool(x)
calls x.__bool__() and uses the result. If __bool__ is not implemented, Python tries to invoke x.__len__(), and if that returns  zero, bool returns False. Otherwise bool returns True. Our

 The “Data Model” chapter of The Python Language Reference lists 83 special method names, 47 of which are used to implement arithmetic, bitwise, and comparison operators.

detail. Why len Is Not a Method I asked this question to core developer Raymond Hettinger in 2013 and the key to his answer was a quote from The Zen of Python: “practicality beats purity.” In How Special Methods Are Used, I described how len(x) runs very fast when x is an instance of a built-in type. No method is called for the built-in objects of CPython: the length is simply read from a field in a C struct. Getting the number of items in a collection is a common operation and must work efficiently for such basic and diverse types as str, list, memoryview, and so on. In other words, len is not called as a method because it gets special treatment as part of the Python data model, just like abs. But thanks to the special method __len__, you can also make len work with your own custom objects. This is a fair compromise between the need for efficient built-in objects and the consistency of the language. Also from The Zen of Python: “Special cases aren’t special enough to break the rules.

 you think of abs and len as unary operators, you may be more inclined to forgive their functional look-and-feel, as opposed to the method call syntax one might expect in an OO language. In fact, the ABC language—a direct ancestor of Python that pioneered many of its features—had an # operator that was the equivalent of len (you’d write #s). When used as an infix operator, written x#s, it counted the occurrences of x in s, which in Python you get as s.count(x), for any sequence s. Chapter

Reading The “Data Model” chapter of The Python Language Reference is the canonical source for the subject of this chapter and much of this book. Python in a Nutshell, 2nd Edition (O’Reilly) by Alex Martelli has excellent coverage of the data model. As I write this, the most recent edition of the Nutshell book is from 2006 and focuses on Python 2.5, but there have been very few changes in the data model since then, and Martelli’s description of the mechanics of attribute access is the most authoritative I’ve seen apart from the actual C source code of CPython.

Overflow. David Beazley has two books covering the data model in detail in the context of Python 3: Python Essential Reference, 4th Edition (Addison-Wesley Professional), and Python Cookbook, 3rd Edition (O’Reilly), coauthored with Brian K. Jones. The Art of the Metaobject Protocol (AMOP, MIT Press) by Gregor Kiczales, Jim des Rivieres, and Daniel G. Bobrow explains the concept of a metaobject protocol (MOP), of which the Python data model is one example.

Data Model or Object Model? What the Python documentation calls the “Python data model,” most authors would say is the “Python object model.” Alex Martelli’s Python in a Nutshell 2E, and David Beazley’s Python Essential Reference 4E are the best books covering the “Python data model,” but they always refer to it as the “object model.” On Wikipedia, the first definition of object model is “The properties of objects in general in a specific computer programming language.” This is what the “Python data model” is about. In this book, I will use “data model” because the documentation favors that term when referring to the Python object model, and because it is the title of the chapter of The Python Language Reference most relevant to our discussions. Magic

The Ruby community calls their equivalent of the special methods magic methods. Many in the Python community adopt that term as well. I believe the special methods are actually the opposite of magic. Python and Ruby are the same in this regard: both empower their users with a rich metaobject protocol that is not magic, but enables users to leverage the same tools available to core developers. In

Metaobjects The Art of the Metaobject Protocol (AMOP) is my favorite computer book title. Less subjectively, the term metaobject protocol is useful to think about the Python data model and similar features in other languages. The metaobject part refers to the objects that are the building blocks of the language itself. In this context, protocol is a synonym of interface. So a metaobject protocol is a fancy synonym for object model: an API for core language constructs. A rich metaobject protocol enables extending

The standard library offers a rich selection of sequence types implemented in C: 
Container sequences
 
  list, tuple, and collections.deque can hold items of different types.
 
Flat sequences
 
  str, bytes, bytearray, memoryview, and array.array hold items of one type.
 Container sequences hold references to the objects they contain, which may be of any type, while flat sequences physically store the value of each item within its own memory space, and not as distinct objects. Thus, flat sequences are more compact, but they are limited to holding primitive values like characters, bytes, and numbers. Another way of grouping sequence types is by mutability: 
Mutable sequences
 
  list, bytearray, array.array, collections.deque, and memoryview
 
Immutable sequences
 
  tuple, str, and bytes


Note that the built-in concrete sequence types do not actually subclass the Sequence and MutableSequence abstract base classes (ABCs) depicted, but the ABCs are still useful as a formalization of what functionality to expect from a full-featured sequence type.

For brevity, many Python programmers refer to list comprehensions as listcomps, and generator expressions as genexps. I will use these words as well.

Of course, it is possible to abuse list comprehensions to write truly incomprehensible code. I’ve seen Python code with listcomps used just to repeat a block of code for its side effects. If you are not doing something with the produced list, you should not use that syntax. Also, try to keep it short. If the list comprehension spans more than two lines, it is probably best to break it apart or rewrite as a plain old for loop. Use your best judgment: for Python as for English, there are no hard-and-fast rules for clear writing. Syntax

scope, sometimes with tragic consequences. See the following Python 2.7 console session: Python 2.7.6 (default, Mar 22 2014, 22:59:38)
[GCC 4.8.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> x = 'my precious'
>>> dummy = [x for x in 'ABC']
>>> x
'C' As you can see, the initial value of x was clobbered. This no longer happens in Python 3. List comprehensions, generator expressions, and their siblings set and dict comprehensions now have their own local scope, like functions. Variables assigned within the expression are local, but variables in the surrounding scope can still be referenced. Even better, the local variables do not mask the variables from the surrounding scope. This is Python 3: >>> x = 'ABC'
>>> dummy = [ord(x) for x in x]
>>> x  
'ABC'
>>> dummy  
[65, 66, 67]
>>>   
The value of x is preserved.
   
The list comprehension produces the expected list.


filter Listcomps do everything the map and filter functions do, without the contortions of the functionally challenged Python lambda. Consider Example 2-3. Example 2-3. The same list built by a listcomp and a map/filter composition >>> symbols = '$¢£¥€¤'
>>> beyond_ascii = [ord(s) for s in symbols if ord(s) > 127]
>>> beyond_ascii
[162, 163, 165, 8364, 164]
>>> beyond_ascii = list(filter(lambda c: c > 127, map(ord, symbols)))
>>> beyond_ascii
[162, 163, 165, 8364, 164] I used to believe that map and filter were faster than the equivalent listcomps, but Alex Martelli pointed out that’s not the case—at least not in the preceding examples.

In Example 1-1 (Chapter 1), the following expression was used to initialize a card deck with a list made of 52 cards from all 13 ranks of each of the 4 suits, grouped by suit:         self._cards = [Card(rank, suit) for suit in self.suits
                                        for rank in self.ranks]

Products Listcomps can generate lists from the Cartesian product of two or more iterables. The items that make up the cartesian product are tuples made from items from every input iterable. The resulting list has a length equal to the lengths of the input iterables multiplied.

Expressions To initialize tuples, arrays, and other types of sequences, you could also start from a listcomp, but a genexp saves memory because it yields items one by one using the iterator protocol instead of building a whole list just to feed another constructor. Genexps use the same syntax as listcomps, but are enclosed in parentheses rather than brackets.

If the generator expression is the single argument in a function call, there is no need to duplicate the enclosing parentheses.
 

Tuple unpacking works with any iterable object. The only requirement is that the iterable yields exactly one item per variable in the receiving tuple, unless you use a star (*) to capture excess items as explained in Using * to grab excess items. The term tuple unpacking is widely used by Pythonistas, but iterable unpacking is gaining traction, as in the title of PEP 3132 — Extended Iterable Unpacking. The

If you write internationalized software, _ is not a good dummy variable because it is traditionally used as an alias to the gettext.gettext function, as recommended in the gettext module documentation. Otherwise, it’s a nice name for placeholder variable. Another way of focusing on just some of the items when unpacking 

Defining function parameters with *args to grab arbitrary excess arguments is a classic Python feature. In Python 3, this idea was extended to apply to parallel assignment as well: >>> a, b, *rest = range(5)
>>> a, b, rest
(0, 1, [2, 3, 4])


Unpacking The tuple to receive an expression to unpack can have nested tuples, like (a, b, (c, d)), and Python will do the right thing if the expression matches the nesting structure.

Before Python 3, it was possible to define functions with nested tuples in the formal parameters (e.g., def fn(a, (b, c), d):). This is no longer supported in Python 3 function definitions, for practical reasons explained in PEP 3113 — Removal of Tuple Parameter Unpacking. To be clear: nothing changed from the perspective of users calling a function. The restriction applies only to the definition of  functions.

Instances of a class that you build with namedtuple take exactly the same amount of memory as tuples because the field names are stored in the class. They use less memory than a regular object because they don’t store attributes in a per-instance __dict__.

.
 A named tuple type has a few attributes in addition to those inherited from tuple. Example 2-10 shows the most useful: the _fields class attribute, the class method _make(iterable), and the _asdict() instance method. Example 2-10. Named tuple attributes

_fields is a tuple with the field names of the class.
   
_make() allow you to instantiate a named tuple from an iterable; City(*delhi_data) would do the same.
   
_asdict() returns a collections.OrderedDict built from the named tuple instance. That can be used to produce a nice display of city data.

When using a tuple as an immutable variation of list, it helps to know how similar they actually are. As you can see in Table 2-1, tuple supports all list methods that do not involve adding or removing items, with one exception—tuple lacks the __reversed__ method. However, that is just for optimization; reversed(my_tuple) works without it. Table

The Pythonic convention of excluding the last item in slices and ranges works well with the zero-based indexing used in Python, C, and many other languages. Some convenient features of the convention are: 
It’s easy to see the length of a slice or range when only the stop position is given: range(3) and my_list[:3] both produce three items.

It’s easy to compute the length of a slice or range when start and stop are given: just subtract stop - start.

It’s easy to split a sequence in two parts at any index x, without overlapping: simply get my_list[:x] and my_list[x:]. For example:
 >>> l = [10, 20, 30, 40, 50, 60]
>>> l[:2]  # split at 2
[10, 20]
>>> l[2:]
[30, 40, 50, 60]
>>> l[:3]  # split at 3
[10, 20, 30]
>>> l[3:]
[40, 50, 60] But the best arguments for this convention were written by the Dutch computer scientist Edsger W. Dijkstra (see the last reference in Further Reading). Now

This is no secret, but worth repeating just in case: s[a:b:c] can be used to specify a stride or step c, causing the resulting slice to skip items. The stride can also be negative, returning items in reverse.

The notation a:b:c is only valid within [] when used as the indexing or subscript operator, and it produces a slice object: slice(a, b, c). As we will see in How Slicing Works, to evaluate the expression seq[start:stop:step], Python calls seq.__getitem__(slice(start, stop, step)). Even if you are not implementing your own sequence types, knowing about slice objects is useful because it lets you assign names to slices, just like spreadsheets allow naming of cell ranges.

The [] operator can also take multiple indexes or slices separated by commas. This is used, for instance, in the external NumPy package, where items of a two-dimensional numpy.ndarray can be fetched using the syntax a[i, j] and a two-dimensional slice obtained with an expression like a[m:n, k:l]. Example 2-22 later in this chapter shows the use of this notation. The __getitem__ and __setitem__ special methods that handle the [] operator simply receive the indices in a[i, j] as a tuple. In other words, to evaluate a[i, j], Python calls a.__getitem__((i, j)). The built-in sequence types in Python are one-dimensional, so they support only one index or slice, and not a tuple of them. The ellipsis—written with three full stops (...) and not … (Unicode U+2026)—is recognized as a token by the Python parser. It is an alias to the Ellipsis object, the single instance of the ellipsis class.[7] As such, it can be passed as an argument to functions and as part of a slice specification, as in f(a, ..., z) or a[i:...]. NumPy uses ... as a shortcut when slicing arrays of many dimensions; for example, if x is a four-dimensional array, x[i, ...] is a shortcut for x[i, :, :, :,]. See the Tentative NumPy Tutorial to learn more about this. At the time of this writing, I am unaware of uses of Ellipsis or multidimensional indexes and slices in the Python standard library. If you spot one, let me know. These syntactic features exist to support user-defined types and extensions such as NumPy. Slices

Mutable sequences can be grafted, excised, and otherwise modified in place using slice notation on the left side of an assignment statement or as the target of a del statement.


>>> l
[0, 1, 20, 30, 5, 6, 7, 8, 9]
>>> del l[5:7]
>>> l
[0, 1, 20, 30, 5, 8, 9]
>>> l[3::2] = [11, 22]
>>> l
[0, 1, 20, 11, 5, 22, 9]
>>> l[2:5] = 100  
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only assign an iterable
>>> l[2:5] = [100]
>>> l
[0, 1, 100, 22, 9]   
When the target of the assignment is a slice, the right side must be an iterable object, even if it has just one item.


Beware of expressions like a * n when a is a sequence containing mutable items because the result may surprise you. For example, trying to initialize a list of lists as my_list = [[]] * 3 will result in a list with three references to the same inner list, which is probably not what you want.

The special method that makes += work is __iadd__ (for “in-place addition”). However, if __iadd__ is not implemented, Python falls back to calling __add__. Consider this simple expression: >>> a += b If a implements __iadd__, that will be called. In the case of mutable sequences (e.g., list, bytearray, array.array), a will be changed in place (i.e., the effect will be similar to a.extend(b)). However, when a does not implement __iadd__, the expression a += b has the same effect as a = a + b: the expression a + b is evaluated first, producing a new object, which is then bound to a. In other words, the identity of the object bound to a may or may not change, depending on the availability of __iadd__. In general, for mutable sequences, it is a good bet that __iadd__ is implemented and that += happens in place. For immutable sequences, clearly there is no way for that to happen. What I just wrote about += also applies to *=, which is implemented via __imul__. The __iadd__ and __imul__ special methods are discussed in Chapter 13. Here

Repeated concatenation of immutable sequences is inefficient, because instead of just appending new items, the interpreter has to copy the whole target sequence to create a new one with the new items concatenated.[

Try to answer without using the console: what is the result of evaluating the two expressions in Example 2-14?[9] Example 2-14. A riddle >>> t = (1, 2, [30, 40])
>>> t[2] += [50, 60] What happens next? Choose the best answer: 
t becomes (1, 2, [30, 40, 50, 60]).

TypeError is raised with the message 'tuple' object does not support item assignment.

Neither.

Both a and b.
 When I saw this, I was pretty sure the answer was b, but it’s actually d, “Both a and b.”! Example 2-15 is the actual output from a Python 3.4 console (actually the result is the same in a Python 2.7 console).[10] Example

If you look at the bytecode Python generates for the expression s[a] += b (Example 2-16), it becomes clear how that happens. Example 2-16. Bytecode for the expression s[a] += b >>> dis.dis('s[a] += b')
  1           0 LOAD_NAME                0 (s)


Put the value of s[a] on TOS (Top Of Stack).
   
Perform TOS += b. This succeeds if TOS refers to a mutable object (it’s a list, in Example 2-15).
   
Assign s[a] = TOS. This fails if s is immutable (the t tuple in Example 2-15).
 This example is quite a corner case—in 15 years of using Python, I have never seen this strange behavior actually bite somebody. I take three lessons from this: 
Putting mutable items in tuples is not a good idea.

Augmented assignment is not an atomic operation—we just saw it throwing an exception after doing part of its job.

Inspecting Python bytecode is not too difficult, and is often helpful to see what is going on under the hood.


The list.sort method sorts a list in place—that is, without making a copy. It returns None to remind us that it changes the target object, and does not create a new list. This is an important Python API convention: functions or methods that change an object in place should return None to make it clear to the caller that the object itself was changed, and no new object was created. The same behavior can be seen, for example, in the random.shuffle function.

The convention of returning None to signal in-place changes has a drawback: you cannot cascade calls to those methods. In contrast, methods that return new objects (e.g., all str methods) can be cascaded in the fluent interface style. See Wikipedia’s Wikipedia’s “Fluent interface” entry for further description of this topic.

bisect(haystack, needle) does a binary search for needle in haystack—which must be a sorted sequence—to locate the position where needle can be inserted while maintaining haystack in ascending order. In other words, all items appearing up to that position are less than or equal to needle. You could use the result of bisect(haystack, needle) as the index argument to haystack.insert(index, needle)—however, using insort does both steps, and is faster. Tip

haystack The behavior of bisect can be fine-tuned in two ways. First, a pair of optional arguments, lo and hi, allow narrowing the region in the sequence to be searched when inserting. lo defaults to 0 and hi to the len() of the sequence. Second, bisect is actually an alias for bisect_right, and there is a sister function called bisect_left. Their difference is apparent only when the needle compares equal to an item in the list: bisect_right returns an insertion point after the existing item, and bisect_left returns the position of the existing item, so insertion would occur before it. With simple types like int this makes no difference, but if the sequence contains objects that are distinct yet compare equal, then it may be relevant. For example, 1 and 1.0 are distinct, but 1 == 1.0 is True. Figure 2-5 shows the result of using bisect_left. Figure

An interesting application of bisect is to perform table lookups by numeric values—for example, to convert test scores to letter grades, as in Example 2-18. Example 2-18. Given a test score, grade returns the corresponding letter grade >>> def grade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
...     i = bisect.bisect(breakpoints, score)
...     return grades[i]
...
>>> [grade(score) for score in [33, 99, 77, 70, 89, 90, 100]]
['F', 'A', 'C', 'C', 'B', 'A', 'A'] The code in Example 2-18 is from the bisect module documentation, which also lists functions to use bisect as a faster replacement for the index method when searching through long ordered sequences of numbers. These

Sorting is expensive, so once you have a sorted sequence, it’s good to keep it that way. That is why bisect.insort was created. insort(seq, item) inserts item into seq so as to keep seq in ascending order. See Example 2-19 and its output in Figure 2-6. Example

 the list will only contain numbers, an array.array is more efficient than a list: it supports all mutable sequence operations (including  .pop, .insert, and .extend), and additional methods for fast loading and saving such as .frombytes and .tofile. A Python array is as lean as a C array. When creating an array, you  provide a typecode, a letter to determine the underlying C type used to store each item in the array. For example, b is the typecode for signed char. If you create an array('b'), then each item will be stored in a single byte and interpreted as an integer from –128 to 127. For large sequences of numbers, this saves a lot of memory. And Python will not let you put any number that does not match the type for the array. Example

Another fast and more flexible way of saving numeric data is the pickle module for object serialization. Saving an array of floats with pickle.dump is almost as fast as with array.tofile—however, pickle handles almost all built-in types, including complex numbers, nested collections, and even instances of user-defined classes automatically (if they are not too tricky in their implementation

As of Python 3.4, the array type does not have an in-place sort method like list.sort(). If you need to sort an array, use the sorted function to rebuild it sorted: a = array.array(a.typecode, sorted(a)) To keep a sorted array sorted while adding items to it, use the bisect.insort function (as seen in Inserting with bisect.insort). If

The built-in memorview class is a shared-memory sequence type that lets you handle slices of arrays without copying bytes. It was inspired by the NumPy library (which we’ll discuss shortly in NumPy and SciPy). Travis Oliphant, lead author of NumPy, answers When should a memoryview be used? like this: A memoryview is essentially a generalized NumPy array structure in Python itself (without the math). It allows you to share memory between data-structures (things like PIL images, SQLlite databases, NumPy arrays, etc.) without first copying. This is very important for large data sets. Using

warranted. For advanced array and matrix operations, NumPy and SciPy are the reason why Python became mainstream in scientific computing applications. NumPy implements multi-dimensional, homogeneous arrays and matrix types that hold not only numbers but also user-defined records, and provides efficient elementwise operations. SciPy is a library, written on top of NumPy, offering many scientific computing algorithms from linear algebra, numerical calculus, and statistics. SciPy is fast and reliable because it leverages the widely used C and Fortran code base from the Netlib Repository. In other words, SciPy gives scientists the best of both worlds: an interactive prompt and high-level Python APIs, together with industrial-strength number-crunching functions optimized in C and Fortran.

The class collections.deque is a thread-safe double-ended queue designed for fast inserting and removing from both ends. It is also the way to go if you need to keep a list of “last seen items” or something like that, because a deque can be bounded—i.e., created with a maximum length—and then, when it is full, it discards items from the opposite end when you append new ones. Example 2-23 shows

Note that deque implements most of the list methods, and adds a few specific to its design, like popleft and rotate. But there is a hidden cost: removing items from the middle of a deque is not as fast. It is really optimized for appending and popping from the ends. The append and popleft operations are atomic, so deque is safe to use as a LIFO queue in multithreaded applications without the need for using locks. Table

queue
 
    This provides the synchronized (i.e., thread-safe) classes Queue, LifoQueue, and PriorityQueue. These are used for safe communication between threads. All three classes can be bounded by providing a maxsize argument greater than 0 to the constructor. However, they don’t discard items to make room as deque does. Instead, when the queue is full the insertion of a new item blocks—i.e., it waits until some other thread makes room by taking an item from the queue, which is useful to throttle the number of live threads.


asyncio
 
    Newly added to Python 3.4, asyncio provides Queue, LifoQueue, PriorityQueue, and JoinableQueue with APIs inspired by the classes contained in the queue and multiprocessing modules, but adapted for managing tasks in asynchronous programming.


heapq
 
    In contrast to the previous three modules, heapq does not implement a queue class, but provides functions like heappush and heappop that let you use a mutable sequence as a heap queue or priority queue.


Chapter 1, “Data Structures” of Python Cookbook, 3rd Edition (O’Reilly) by David Beazley and Brian K. Jones has many recipes focusing on sequences, including “Recipe 1.11. Naming a Slice,” from which I learned the trick of assigning slices to variables to improve readability, illustrated in our Example 2-11. The second edition of Python Cookbook was written for Python 2.4, but much of its code works with Python 3, and a lot of the recipes in Chapters 5 and 6 deal with sequences. The book was edited by Alex Martelli, Anna Martelli Ravenscroft, and David Ascher, and it includes contributions by dozens of Pythonistas. The third edition was rewritten from scratch, and focuses more on the semantics of the language—particularly what has changed in Python 3—while the older volume emphasizes pragmatics (i.e., how to apply the language to real-world problems). Even though some of the second edition solutions are no longer the best approach, I honestly think it is worthwhile to have both editions of Python Cookbook on hand.

Eli Bendersky’s blog post “Less Copies in Python with the Buffer Protocol and memoryviews includes a short tutorial on memoryview. There are numerous books covering NumPy in the market, even some that don’t mention “NumPy” in the title. Wes McKinney’s Python for Data Analysis (O’Reilly) is one such title. Scientists

The best defense of the Python convention of excluding the last item in ranges and slices was written by Edsger W. Dijkstra himself, in a short memo titled “Why Numbering Should Start at Zero”. The subject of the memo is mathematical notation, but it’s relevant to Python because Prof. Dijkstra explains with rigor and humor why the sequence 2, 3, …, 12 should always be expressed as 2 ≤ i < 13. All other reasonable conventions are refuted, as is the idea of letting each user choose a convention. The title refers to zero-based indexing, but the memo is really about why it is desirable that 'ABCDE'[1:3] means 'BC' and not 'BCD' and why it makes perfect sense to write 2, 3, …, 12 as range(2, 13). (By the way, the memo is a handwritten note, but it’s beautiful and totally readable. Somebody should create a Dijkstra font—I’d buy it.) Soapbox

feature. With each of these changes, the language became more flexible, more consistent, and simpler at the same time. “Elegance begets simplicity” is the motto on my favorite PyCon T-shirt from Chicago, 2009.

Lists Introductory Python texts emphasize that lists can contain objects of mixed types, but in practice that feature is not very useful: we put items in a list to process them later, which implies that all items should support at least some operation in common (i.e., they should all “quack” whether or not they are genetically 100% ducks). For example, you can’t sort a list in Python 3 unless the items in it are comparable: >>> l = [28, 14, '28', 5, '9', '1', 0, 6, '23', 19]
>>> sorted(l)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unorderable types: str() < int() Unlike lists, tuples often hold items of different types. That is natural, considering that each item in a tuple is really a field, and each field type is independent of the others.

to provide a two-argument comparison function like the deprecated cmp(a, b) function in Python 2. Using key is both simpler and more efficient. It’s simpler because you just define a one-argument function that retrieves or calculates whatever criterion you want to use to sort your objects; this is easier than writing a two-argument function to return –1, 0, 1. It is also more efficient because the key function is invoked only once per item, while the two-argument comparison is called every time the sorting algorithm needs to compare two items. Of course, Python also has to compare the keys while sorting, but that comparison is done in optimized C code and not in a Python function that you wrote. By the way, using key actually lets us sort a mixed bag of numbers and number-like strings. You just need to decide whether you want to treat all items as integers or strings: >

The dict type is not only widely used in our programs but also a fundamental part of the Python implementation. Module namespaces, class and instance attributes, and function keyword arguments are some of the fundamental constructs where dictionaries are deployed. The built-in functions live in __builtins__.__dict__. Because of their crucial role, Python dicts are highly optimized. Hash tables are the engines behind Python’s high-performance dicts. We


 Generic Mapping Types The collections.abc module provides the Mapping and MutableMapping ABCs to formalize the interfaces of dict and similar types (in Python 2.6 to 3.2, these classes are imported from the collections module, and not from collections.abc). See Figure

Implementations of specialized mappings often extend dict or collections.UserDict, instead of these ABCs. The main value of the ABCs is documenting and formalizing the minimal interfaces for mappings, and serving as criteria for isinstance tests in code that needs to support mappings in a broad sense: >>> my_dict = {}
>>> isinstance(my_dict, abc.Mapping)
True Using isinstance is better than checking whether a function argument is of dict type, because then alternative mapping types can be used. All mapping types in the standard library use the basic dict in their implementation, so they share the limitation that the keys must be hashable (the values need not be hashable, only the keys

). What Is Hashable? Here is part of the definition of hashable from the Python Glossary: An object is hashable if it has a hash value which never changes during its lifetime (it needs a __hash__() method), and can be compared to other objects (it needs an __eq__() method). Hashable objects which compare equal must have the same hash value. […] The atomic immutable types (str, bytes, numeric types) are all hashable. A frozenset is always hashable, because its elements must be hashable by definition. A tuple is hashable only if all its items are hashable. See tuples

Warning At the time of this writing, the Python Glossary states: “All of Python’s immutable built-in objects are hashable” but that is inaccurate because a tuple is immutable, yet it may contain references to unhashable objects.

User-defined types are hashable by default because their hash value is their id() and they all compare not equal. If an object implements a custom __eq__ that takes into account its internal state, it may be hashable only if all its attributes are immutable.

The default_factory of a defaultdict is only invoked to provide default values for __getitem__ calls, and not for the other methods. For example, if dd is a defaultdict, and k is a missing key, dd[k] will call the default_factory to create a default value, but dd.get(k) still returns None. The

Underlying the way mappings deal with missing keys is the aptly named __missing__ method. This method is not defined in the base dict class, but dict is aware of it: if you subclass dict and provide a __missing__ method, the standard dict.__getitem__ will call it whenever a key is not found, instead of raising KeyError. Warning The __missing__ method is just called by __getitem__ (i.e., for the d[k] operator). The presence of a __missing__ method has no effect on the behavior of other methods that look up keys, such as get or __contains__ (which implements the in operator). This is why the default_factory of defaultdict works only with __getitem__, as noted in the warning at the end of the previous section. Suppose

Maintains keys in insertion order, allowing iteration over items in a predictable order. The popitem method of an OrderedDict pops the first item by default, but if called as my_odict.popitem(last=True), it pops the last item added.

collections.Counter
 
    A mapping that holds an integer count for each key. Updating an existing key adds to its count. This can be used to count instances of hashable objects (the keys) or as a multiset—a set that can hold several occurrences of each element. Counter implements the + and - operators to combine tallies, and other useful methods such as most_common([n]), which returns an ordered list of tuples with the n most common items and their counts; see the documentation.

collections.ChainMap
 
    Holds a list of mappings that can be searched as one. The lookup is performed on each mapping in order, and succeeds if the key is found in any of them. This is useful to interpreters for languages with nested scopes, where each mapping represents a scope context. The “ChainMap objects” section of the collections docs has several examples of ChainMap usage, including this snippet inspired by the basic rules of variable lookup in Python:
 import builtins
pylookup = ChainMap(locals(), globals(), vars(builtins)) 


It’s almost always easier to create a new mapping type by extending UserDict rather than dict. Its value can be appreciated as we extend our StrKeyDict0 from Example 3-7 to make sure that any keys added to the mapping are stored as str. The main reason why it’s preferable to subclass from UserDict rather than from dict is that the built-in has some implementation shortcuts that end up forcing us to override methods that we can just inherit from UserDict with no problems.[14] Note that UserDict does not inherit from dict, but has an internal dict instance, called data, which holds the actual items. This avoids undesired recursion when coding special methods like __setitem__, and simplifies the coding of __contains__, compared to Example 3-7.

.
 Because UserDict subclasses MutableMapping, the remaining methods that make StrKeyDict a full-fledged mapping are inherited from UserDict, MutableMapping, or Mapping. The latter have several useful concrete methods, in spite of being abstract base classes (ABCs). The following methods are worth noting: 
MutableMapping.update
 
This powerful method can be called directly but is also used by __init__ to load the instance from other mappings, from iterables of (key, value) pairs, and keyword arguments. Because it uses self[key] = value to add items, it ends up calling our implementation of __setitem__.
 
Mapping.get
 
In StrKeyDict0 (Example 3-7), we had to code our own get to obtain results consistent with __getitem__, but in Example 3-8 we inherited Mapping.get, which is implemented exactly like StrKeyDict0.get (see Python source code).


After I wrote StrKeyDict, I discovered that Antoine Pitrou authored PEP 455 — Adding a key-transforming dictionary to collections and a patch to enhance the collections module with a TransformDict.

The mapping types provided by the standard library are all mutable, but you may need to guarantee that a user cannot change a mapping by mistake. A concrete use case can be found, again, in the Pingo.io project I described in The __missing__ Method: the board.pins mapping represents the physical GPIO pins on the device. As such, it’s nice to prevent inadvertent updates to board.pins because the hardware can’t possibly be changed via software, so any change in the mapping would make it inconsistent with the physical reality of the device. Since Python 3.3, the types module provides a wrapper class called MappingProxyType, which, given a mapping, returns a mappingproxy instance that is a read-only but dynamic view of the original mapping. This means that updates to the original mapping can be seen in the mappingproxy, but changes cannot be made through it.

Here is how this could be used in practice in the Pingo.io scenario: the constructor in a concrete Board subclass would fill a private mapping with the pin objects, and expose it to clients of the API via a public .pins attribute implemented as a mappingproxy. That way the clients would not be able to add, remove, or change pins by accident.

Set elements must be hashable. The set type is not hashable, but frozenset is, so you can have frozenset elements inside a set. In

The syntax of set literals—{1}, {1, 2}, etc.—looks exactly like the math notation, with one important exception: there’s no literal notation for the empty set, so we must remember to write set(). Syntax

Literal set syntax like {1, 2, 3} is both faster and more readable than calling the constructor (e.g., set([1, 2, 3])). The latter form is slower because, to evaluate it, Python has to look up the set name to fetch the constructor, then build a list, and finally pass it to the constructor. In contrast, to process a literal like {1, 2, 3}, Python runs a specialized BUILD_SET bytecode.

Set comprehensions (setcomps) were added in Python 2.7, together with the dictcomps that we saw in dict Comprehensions.

The infix operators in Table 3-2 require that both operands be sets, but all other methods take one or more iterable arguments. For example, to produce the union of four collections, a, b, c, and d, you can call a.union(b, c, d), where a must be a set, but b, c, and d can be iterables of any type.

 your program does any kind of I/O, the lookup time for keys in dicts or sets is negligible, regardless of the dict or set size (as long as it does fit in RAM). See the code used to generate Table 3-6 and accompanying discussion in Appendix A, Example A-1. Now that we have concrete evidence of the speed of dicts and sets, let’

Starting with Python 3.3, a random salt value is added to the hashes of str, bytes, and datetime objects. The salt value is constant within a Python process but varies between interpreter runs. The random salt is a security measure to prevent a DOS attack. Details are in a note in the documentation for the __hash__ special method.

If you implement a class with a custom __eq__ method, you must also implement a suitable __hash__, because you must always make sure that if a == b is True then hash(a) == hash(b) is also True. Otherwise you are breaking an invariant of the hash table algorithm, with the grave consequence that dicts and sets will not deal reliably with your objects. If a custom __eq__ depends on mutable state, then __hash__ must raise TypeError with a message like unhashable type: 'MyClass'. dicts

In Python 3, the .keys(), .items(), and .values() methods return dictionary views, which behave more like sets than the lists returned by these methods in Python 2. Such views are also dynamic: they do not replicate the contents of the dict, and they immediately reflect any changes to the dict. We

Whenever you add a new item to a dict, the Python interpreter may decide that the hash table of that dictionary needs to grow. This entails building a new, bigger hash table, and adding all current items to the new table. During this process, new (but different) hash collisions may happen, with the result that the keys are likely to be ordered differently in the new hash table. All of this is implementation-dependent, so you cannot reliably predict when it will happen. If you are iterating over the dictionary keys and changing them at the same time, your loop may not scan all the items as expected—not even the items that were already in the dictionary before you added to it. This

Two powerful methods available in most mappings are setdefault and update. The setdefault method is used to update items holding mutable values, for example, in a dict of list values, to avoid redundant searches for the same key. The update method allows bulk insertion or overwriting of items from any other mapping, from iterables providing (key, value) pairs and from keyword arguments. Mapping constructors also use update internally, allowing instances to be initialized from mappings, iterables, or keyword arguments.

The Unicode standard explicitly separates the identity of characters from specific byte representations:

Views The struct module provides functions to parse packed bytes into a tuple of fields of different types and to perform the opposite conversion, from a tuple into packed bytes. struct is used with bytes, bytearray, and memoryview objects.

Note that slicing a memoryview returns a new memoryview, without copying bytes (Leonardo Rochael—one of the technical reviewers—pointed out that even less byte copying would happen if I used the mmap module to open the image as a memory-mapped file. I will not cover mmap in this book, but if you read and change binary files frequently, learning more about mmap — Memory-mapped file support will be very fruitful

We will not go deeper into memoryview or the struct module in this book, but if you work with binary data, you’ll find it worthwhile to study their docs: Built-in Types » Memory Views and struct — Interpret bytes as packed binary data.

) All those asterisks in Figure 4-1 make clear that some encodings, like ASCII and even the multibyte GB2312, cannot represent every Unicode character. The UTF encodings, however, are designed to handle every Unicode code point. The encodings shown in Figure 4-1 were chosen as a representative sample: 
latin1 a.k.a. iso8859_1
 
Important because it is the basis for other encodings, such as cp1252 and Unicode itself (note how the latin1 byte values appear in the cp1252 bytes and even in the code points

UTF-16 superseded the original 16-bit Unicode 1.0 encoding—UCS-2—way back in 1996. UCS-2 is still deployed in many systems, but it only supports code points up to U+FFFF. As of Unicode 6.3, more than 50% of the allocated code points are above U+10000, including the increasingly popular emoji pictographs. With

Most non-UTF codecs handle only a small subset of the Unicode characters. When converting text to bytes, if a character is not defined in the target encoding, UnicodeEncodeError will be raised, unless special handling is provided by passing an errors argument to the encoding method or function. The

The error='ignore' handler silently skips characters that cannot be encoded; this is usually a very bad idea.
   
When encoding, error='replace' substitutes unencodable characters with '?'; data is lost, but users will know something is amiss.
 

The codecs error handling is extensible. You may register extra strings for the errors argument by passing a name and an error handling function to the codecs.register_error function. See the codecs.register_error documentation. Coping

>> city.encode('cp437', errors='ignore')

Garbled characters are known as gremlins or mojibake (文字化け—Japanese for “transformed text”). Example

Not every byte holds a valid ASCII character, and not every byte sequence is valid UTF-8 or UTF-16; therefore, when you assume one of these encodings while converting a binary sequence to text, you will get a UnicodeDecodeError if unexpected bytes are found. On the other hand, many legacy 8-bit encodings like 'cp1252', 'iso8859_1', and 'koi8_r' are able to decode any stream of bytes, including random noise, without generating errors. Therefore, if your program assumes the wrong 8-bit encoding, it will silently decode 

SyntaxError When Loading Modules with Unexpected Encoding UTF-8 is the default source encoding for Python 3, just as ASCII was the default for Python 2 (starting with 2.5). If you load a .py module containing non-UTF-8 data and no encoding declaration, you get a message like this: 

Now that Python 3 source code is no longer limited to ASCII and defaults to the excellent UTF-8 encoding, the best “fix” for source code in legacy encodings like 'cp1252' is to convert them to UTF-8 already, and not bother with the coding comments. If your editor does not support UTF-8, it’s time to switch. Non

Python 3 allows non-ASCII identifiers in source code: >>> ação = 'PBR'  # ação = stock
>>> ε = 10**-6    # ε = epsilon Some people dislike the idea. The most common argument to stick with ASCII identifiers is to make it easy for everyone to read and edit code. That argument misses the point: you want your source code to be readable and editable by its intended audience, and that may not be “everyone.” If the code belongs to a multinational corporation or is open source and you want contributors from around the world, the identifiers should be in English, and then all you need is ASCII. But if you are a teacher in Brazil, your students will find it easier to read code that uses Portuguese variable and function names, correctly spelled. And they will have no difficulty typing the cedillas and accented vowels on their localized keyboards. Now that Python can parse Unicode names and UTF-8 is the default source encoding, I see no point in coding identifiers in Portuguese without accents, as we used to do in Python 2 out of necessity—unless you need the code to run on Python 2 also. If the names are in Portuguese, leaving out the accents won’t make the code more readable to anyone. This is my point of view as a Portuguese-speaking Brazilian, but I believe it applies across borders and cultures: choose the human language that makes the code easier to read by the team, then use the characters needed for correct spelling. Suppose

How do you find the encoding of a byte sequence? Short answer: you can’t. You must be told. Some communication protocols and file formats, like HTTP and XML, contain headers that explicitly tell us how the content is encoded. You can be sure that some byte streams are not ASCII because they contain byte values over 127, and the way UTF-8 and UTF-16 are built also limits the possible byte sequences. But even then, you can never be 100% positive that a binary file is ASCII 

However, considering that human languages also have their rules and restrictions, once you assume that a stream of bytes is human plain text it may be possible to sniff out its encoding using heuristics and statistics. For example, if b'\x00' bytes are common, it is probably a 16- or 32-bit encoding, and not an 8-bit scheme, because null characters in plain text are bugs; when the byte sequence b'\x20\x00' appears often, it is likely to be the space character (U+0020) in a UTF-16LE encoding, rather than the obscure U+2000 EN QUAD character—whatever that is. That is how the package Chardet — The Universal Character Encoding Detector works to identify one of 30 supported encodings. Chardet is a Python library that you can use in your programs, but also includes a command-line utility, chardetect.

The bytes are b'\xff\xfe'. That is a BOM—byte-order mark—denoting the “little-endian” byte ordering of the Intel CPU where the encoding was performed. On a little-endian machine, for each code point the least significant byte comes first: the letter 'E', code point U+0045 (decimal 69), is encoded in byte offsets 2 and 3 as 69 and 0: >

On a big-endian CPU, the encoding would be reversed; 'E' would be encoded as 0 and 69. To avoid confusion, the UTF-16 encoding prepends the text to be encoded with the special character ZERO WIDTH NO-BREAK SPACE (U+FEFF), which is invisible. On a little-endian system, that is encoded as b'\xff\xfe' (decimal 255, 254). Because, by design, there is no U+FFFE character, the byte sequence b'\xff\xfe' must mean the ZERO WIDTH NO-BREAK SPACE on a little-endian encoding, so the codec knows which byte ordering to use. There is a variant of UTF-16—UTF-16LE—that is explicitly little-endian, and another one explicitly big-endian, UTF-16BE. If you use them, a BOM is not generated: >

present, the BOM is supposed to be filtered by the UTF-16 codec, so that you only get the actual text contents of the file without the leading  ZERO WIDTH NO-BREAK SPACE. The standard says that if a file is UTF-16 and has no BOM, it should be assumed to be UTF-16BE (big-endian). However, the Intel x86 architecture is little-endian, so there is plenty of little-endian UTF-16 with no BOM in the wild. This

The best practice for handling text is the “Unicode sandwich” (Figure 4-2).[22] This means that bytes should be decoded to str as early as possible on input (e.g., when opening a file for reading). The “meat” of the sandwich is the business logic of your program, where text handling is done exclusively on str objects. You should never be encoding or decoding in the middle of other processing. On output, the str are encoded to bytes as late as possible. Most web frameworks work like that, and we rarely touch bytes when using them. In Django, for example, your views should output Unicode str; Django itself takes care of encoding the response to bytes, using UTF-8 by 

problems. Tip Code that has to run on multiple machines or on multiple occasions should never depend on encoding defaults. Always pass an explicit encoding= argument when opening text files, because the default may change from one machine to the next, or from one day to the next.

 you omit the encoding argument when opening a file, the default is given by locale.getpreferredencoding() ('cp1252' in Example 4-12).

The encoding of sys.stdout/stdin/stderr is given by the PYTHONIOENCODING environment variable, if present, otherwise it is either inherited from the console or defined by locale.getpreferredencoding() if the output/input is redirected to/from a file.

sys.getdefaultencoding() is used internally by Python to convert binary data to/from str; this happens less often in Python 3, but still happens.[24] Changing this setting is not supported.[25]

sys.getfilesystemencoding() is used to encode/decode filenames (not file contents). It is used when open() gets a str argument for the filename; if the filename is given as a bytes argument, it is passed unchanged to the OS API. The Python Unicode HOWTO says: “on Windows, Python uses the name mbcs to refer to whatever the currently configured encoding is.” The acronym MBCS stands for Multi Byte Character Set, which for Microsoft are the legacy variable-width encodings like gb2312 or Shift_JIS, but not UTF-8. (On this topic, a useful answer on StackOverflow is “Difference between MBCS and UTF-8 on Windows”.)


Note On GNU/Linux and OSX all of these encodings are set to UTF-8 by default, and have been for several years, so I/O handles all Unicode characters. On Windows, not only are different encodings used in the same system, but they are usually codepages like 'cp850' or 'cp1252' that support only ASCII with 127 additional characters that are not the same from one encoding to the other. Therefore, Windows users are far more likely to face encoding errors unless they are extra careful.

String comparisons are complicated by the fact that Unicode has combining characters: diacritics and other marks that attach to the preceding character, appearing as one when printed. For example, the word “café” may be composed in two ways, using four or five code points, but the result looks exactly the same: >>> s1 = 'café'
>>> s2 = 'cafe\u0301'
>>> s1, s2
('café', 'café')
>>> len(s1), len(s2)
(4, 5)
>>> s1 == s2
False The

The solution is to use Unicode normalization, provided by the unicodedata.normalize function. The first argument to that function is one of four strings: 'NFC', 'NFD', 'NFKC', and 'NFKD'. Let’s start with the first two. Normalization Form C (NFC) composes the code points to produce the shortest equivalent string, while NFD decomposes, expanding composed characters into base characters and separate combining characters. Both of these normalizations make comparisons work as expected: 

Western keyboards usually generate composed characters, so text typed by users will be in NFC by default. However, to be safe, it may be good to sanitize strings with normalize('NFC', user_text) before saving. NFC is also the normalization form recommended by the W3C in Character Model for the World Wide Web: String Matching and Searching. Some single characters are normalized by NFC into another single character. The symbol for the ohm (Ω) unit of electrical resistance is normalized to the Greek uppercase omega. They are visually identical, but they compare unequal so it is essential to normalize to avoid surprises: 

In the acronyms for the other two normalization forms—NFKC and NFKD—the letter K stands for “compatibility.” These are stronger forms of normalization, affecting the so-called “compatibility characters.” Although one goal of Unicode is to have a single “canonical” code point for each character, some characters appear more than once for compatibility with preexisting standards. For example, the micro sign, 'µ' (U+00B5), was added to Unicode to support round-trip conversion to latin1, even though the same character is part of the Greek alphabet with code point U+03BC (GREEK SMALL LETTER MU). So, the micro sign is considered a “compatibility character.” In the NFKC and NFKD forms, each compatibility character is replaced by a “compatibility decomposition” of one or more characters that are considered a “preferred” representation, even if there is some formatting loss—ideally, the formatting should be the responsibility of external markup, not part of Unicode. To exemplify, the compatibility decomposition of the one half fraction '½' (U+00BD) is the sequence of three characters '1/2', and the compatibility decomposition of the micro sign 'µ' (U+00B5) is the lowercase mu 'μ' (U+03BC).[

Warning NFKC and NFKD normalization should be applied with care and only in special cases—e.g., search and indexing—and not for permanent storage, because these transformations cause data loss.

Case folding is essentially converting all text to lowercase, with some additional transformations. It is supported by the str.casefold() method (new in Python 3.3). For any string s containing only latin1 characters, s.casefold() produces the same result as s.lower(), with only two exceptions—the micro sign 'µ' is changed to the Greek lowercase mu (which looks the same in most fonts) and the German Eszett or “sharp s” (ß) becomes “ss”: >

As of Python 3.4, there are 116 code points for which str.casefold() and str.lower() return different results. That’s 0.11% of a total of 110,122 named characters in Unicode 6.3. As

As we’ve seen, NFC and NFD are safe to use and allow sensible comparisons between Unicode strings. NFC is the best normalized form for most applications. str.casefold() is the way to go for case-insensitive comparisons. If you work with text in many languages, a pair of functions like nfc_equal and fold_equal in Example 4-13 are useful additions to your toolbox. Example

The standard way to sort non-ASCII text in Python is to use the locale.strxfrm function which, according to the locale module docs, “transforms a string to one that can be used in locale-aware comparisons.” To enable locale.strxfrm, you must first set a suitable locale for your application, and pray that the OS supports it. On GNU/Linux (Ubuntu 14.04) with the pt_BR locale, the sequence of commands in Example 4-19 works.

So you need to call setlocale(LC_COLLATE, «your_locale») before using locale.strxfrm as the key when sorting. There are a few caveats, though: 
Because locale settings are global, calling setlocale in a library is not recommended. Your application or framework should set the locale when the process starts, and should not change it afterwards.

The locale must be installed on the OS, otherwise setlocale raises a locale.Error: unsupported locale setting exception.

You must know how to spell the locale name. They are pretty much standardized in the Unix derivatives as 'language_code.encoding', but on Windows the syntax is more complicated: Language Name-Language Variant_Region Name.codepage>. Note that the Language Name, Language Variant, and Region Name parts can have spaces inside them, but the parts after the

The locale must be correctly implemented by the makers of the OS. I was successful on Ubuntu 14.04, but not on OSX (Mavericks 10.9). On two different Macs, the call setlocale(LC_COLLATE, 'pt_BR.UTF-8') returns the string 'pt_BR.UTF-8' with no complaints. But sorted(fruits, key=locale.strxfrm) produced the same incorrect result as sorted(fruits) did. I also tried the fr_FR, es_ES, and de_DE locales on OSX, but locale.strxfrm never did its job.

James Tauber, prolific Django contributor, must have felt the pain and created PyUCA, a pure-Python implementation of the Unicode Collation Algorithm (UCA). Example 4-20 shows how easy it is to use. Example 4-20. Using the pyuca.Collator.sort_key method >>> import pyuca
>>> coll = pyuca.Collator()
>>> fruits = ['caju', 'atemoia', 'cajá', 'açaí', 'acerola']
>>> sorted_fruits = sorted(fruits, key=coll.sort_key)
>>> sorted_fruits
['açaí', 'acerola', 'atemoia', 'cajá', 'caju'] This is friendly and just works. I tested it on GNU/Linux, OSX, and Windows. Only Python 3.X is supported at this time. PyUCA does not take the locale into account. If you need to customize the sorting, you can provide the path to a custom collation table to the Collator() constructor. Out of the box, it uses allkeys.txt, which is bundled with the project. That’s just a copy of the Default Unicode Collation Element Table from Unicode 6.3.0. By the way, that table is one of the many that comprise the Unicode database, our next subject. The

The Unicode standard provides an entire database—in the form of numerous structured text files—that includes not only the table mapping code points to character names, but also metadata about the individual characters and how they are related. For example, the Unicode database records whether a character is printable, is a letter, is a decimal digit, or is some other numeric symbol. That’s how the str methods isidentifier, isprintable, isdecimal, and isnumeric work. str.casefold also uses information from a Unicode table. The unicodedata module has functions that return character metadata; for instance, its official name in the standard, whether it is a combining character (e.g., diacritic like a combining tilde), and the numeric value of the symbol for humans (not its code point). Example 4-21 shows the use of unicodedata.name() and unicodedata.numeric() along with the .isdecimal() and .isnumeric() methods of str. Example

Figure 4-3 shows that the regular expression r'\d' matches the digit “1” and the Devanagari digit 3, but not some other characters that are considered digits by the isdigit function. The re module is not as savvy about Unicode as it could be. The new regex module available in PyPI was designed to eventually replace re and provides better Unicode support.[

The GNU/Linux kernel is not Unicode savvy, so in the real world you may find filenames made of byte sequences that are not valid in any sensible encoding scheme, and cannot be decoded to str. File servers with clients using a variety of OSes are particularly prone to this problem. In order to work around this issue, all os module functions that accept filenames or pathnames take arguments as str or bytes. If one such function is called with a str argument, the argument will be automatically converted using the codec named by sys.getfilesystemencoding(), and the OS response will be decoded with the same codec. This is almost always what you want, in keeping with the Unicode sandwich best practice.

 trick to deal with unexpected bytes or unknown encodings is the surrogateescape codec error handler described in PEP 383 — Non-decodable Bytes in System Character Interfaces introduced in Python 3.1. The idea of this error handler is to replace each nondecodable byte with a code point in the Unicode range from U+DC00 to U+DCFF that lies in the so-called “Low Surrogate Area” of the standard—a code space with no characters assigned, reserved for internal use in applications. On encoding, such code points are converted back to the byte values they replaced.

Summary We started the chapter by dismissing the notion that 1 character == 1 byte. As the world adopts Unicode (80% of websites already use UTF-8), we need to keep the concept of text strings separated from the binary sequences that represent them in files, and Python 3 enforces this separation. After

Ned Batchelder’s 2012 PyCon US talk “Pragmatic Unicode — or — How Do I Stop the Pain?” was outstanding. Ned is so professional that he provides a full transcript of the talk along with the slides and video. Esther Nam and Travis Fischer gave an excellent PyCon 2014 talk “Character encoding and Unicode in Python: How to (╯°□°)╯︵ ┻━┻ with dignity” (slides, video), from which I quoted this chapter’s short and sweet epigraph: “Humans use text. Computers speak bytes.” Lennart Regebro—one of this book’s technical reviewers—presents his “Useful Mental Model of Unicode (UMMU)” in the short post “Unconfusing Unicode: What Is Unicode?”. Unicode is a complex standard, so Lennart’s UMMU is a really useful starting point.

The W3C pages Case Folding: An Introduction and Character Model for the World Wide Web: String Matching and Searching cover normalization concepts, with the former being a gentle introduction and the latter a working draft written in dry standard-speak—the same tone of the Unicode Standard Annex #15 — Unicode Normalization Forms.

Martijn Faassen’s “Changing the Python Default Encoding Considered Harmful” and Tarek Ziadé’s “sys.setdefaultencoding Is Evil” explain why the default encoding you get from sys.getdefaultencoding() should never be changed, even if you discover how.

books Unicode Explained by Jukka K. Korpela (O’Reilly) and Unicode Demystified by Richard Gillam (Addison-Wesley) are not Python-specific but were very helpful as I studied Unicode 

Unicode Riddles Imprecise qualifiers such as “often,” “most,” and “usually” seem to pop up whenever I write about Unicode normalization. I regret the lack of more definitive advice, but there are so many exceptions to the rules in Unicode that it is hard to be absolutely positive. For example, the µ (micro sign) is considered a “compatibility character” but the Ω (ohm) and Å (Ångström) symbols are not. The difference has practical consequences: NFC normalization—recommended for text matching—replaces the Ω (ohm) by Ω (uppercase Grek omega) and the Å (Ångström) by Å (uppercase A with ring above). But as a “compatibility character” the µ (micro sign) is not replaced by the visually identical μ (lowercase Greek mu), except when the stronger NFKC or NFKD normalizations are applied, and these transformations are lossy. I understand the µ (micro sign) is in Unicode because it appears in the latin1 encoding and replacing it with the Greek mu would break round-trip conversion. After all, that’s why the micro sign is a “compatibility character.” But if the ohm and Ångström symbols are not in Unicode for compatibility reasons, then why have them at all? There are already code points for the GREEK CAPITAL LETTER OMEGA and the LATIN CAPITAL LETTER A WITH RING ABOVE, which look the same and replace them on NFC normalization. Go figure. My take after many hours studying Unicode: it is hugely complex and full of special cases, reflecting the wonderful variety of human languages and the politics of industry 

Functions in Python are first-class objects. Programming language theorists define a “first-class object” as a program entity that can be: 
Created at runtime

Assigned to a variable or element in a data structure

Passed as an argument to a function

Returned as the result of a function
 Integers, strings, and dictionaries are other examples of first-class objects in Python—nothing fancy here. But if

Functional languages commonly offer the map, filter, and reduce higher-order functions (sometimes with different names). The map and filter functions are still built-ins in Python 3, but since the introduction of list comprehensions and generator expressions, they are not as important. A listcomp or a genexp does the job of map and filter combined, but is more readable. Consider

.
 In Python 3, map and filter return generators—a form of iterator—so their direct substitute is now a generator expression (in Python 2, these functions returned lists, therefore their closest alternative is a listcomp). The reduce function was demoted from a built-in in Python 2 to the functools module in Python 3. Its most common use case, summation, is better served by the sum built-in available since Python 2.3 was released in

Lundh’s lambda Refactoring Recipe If you find a piece of code hard to understand because of a lambda, Fredrik Lundh suggests this refactoring procedure: 
Write a comment explaining what the heck that lambda does.

Study the comment for a while, and think of a name that captures the essence of the comment.

Convert the lambda to a def statement, using that name.

Remove the comment.
 These steps are quoted from the Functional Programming HOWTO, a must read.

 lambda syntax is just syntactic sugar: a lambda expression creates a function object just like the def statement. That is just one of several kinds of callable objects in Python.

The call operator (i.e., ()) may be applied to other objects beyond user-defined functions. To determine whether an object is callable, use the callable() built-in function. The Python Data Model documentation lists seven callable types: 
User-defined functions
 
Created with def statements or lambda expressions.
 
Built-in functions
 
A function implemented in C (for CPython), like len or time.strftime.
 
Built-in methods
 
Methods implemented in C, like dict.get.
 
Methods
 
Functions defined in the body of a class.
 
Classes
 
When invoked, a class runs its __new__ method to create an instance, then __init__ to initialize it, and finally the instance is returned to the caller. Because there is no new operator in Python, calling a class is like calling a function. (Usually calling a class creates an instance of the same class, but other behaviors are possible by overriding __new__. We’ll see an example of this in Flexible Object Creation with __new__.)
 
Class instances
 
If a class defines a __call__ method, then its instances may be invoked as functions. See User-Defined Callable Types.
 
Generator functions
 
Functions or methods that use the yield keyword. When called, generator functions return a generator object.


Given the variety of existing callable types in Python, the safest way to determine whether an object is callable is to use the callable() built-in: >

Not only are Python functions real objects, but arbitrary Python objects may also be made to behave like functions. Implementing a __call__ instance method is all it takes. Example

True A class implementing __call__ is an easy way to create function-like objects that have some internal state that must be kept across invocations, like the remaining items in the BingoCage. An example is a decorator. Decorators must be functions, but it is sometimes convenient to be able to “remember” something between calls of the decorator (e.g., for memoization—caching the results of expensive computations for later use).

Like the instances of a plain user-defined class, a function uses the __dict__ attribute to store user attributes assigned to it. This is useful as a primitive form of annotation. Assigning arbitrary attributes to functions is not a very common practice in general, but Django is one framework that uses it. See, for example, the short_description, boolean, and allow_tags attributes described in The Django admin site documentation.

One of the best features of Python functions is the extremely flexible parameter handling mechanism, enhanced with keyword-only arguments in Python 3. Closely related are the use of * and ** to “explode” iterables and mappings into separate arguments when we call a function.

 bound to the named parameters, with the remaining caught by **attrs.
 Keyword-only arguments are a new feature in Python 3. In Example 5-10, the cls parameter can only be given as a keyword argument—it will never capture unnamed positional arguments. To specify keyword-only arguments when defining a function, name them after the argument prefixed with *. If you don’t want to support variable positional arguments but still want keyword-only arguments, put a * by itself in the signature,

How does Bobo know which parameter names are required by the function, and whether they have default values or not? Within a function object, the __defaults__ attribute holds a tuple with the default values of positional and keyword arguments. The defaults for keyword-only arguments appear in __kwdefaults__. The names of the arguments, however, are found within the __code__ attribute, which is a reference to a code object with many attributes of its own. To demonstrate the use of these attributes, we will inspect the function

2 As you can see, this is not the most convenient arrangement of information. The argument names appear in __code__.co_varnames, but that also includes the names of the local variables created in the body of the function. Therefore, the argument names are the first N strings, where N is given by __code__.co_argcount which—by the way—does not include any variable arguments prefixed with * or **. The default values are identified only by their position in the __defaults__ tuple, so to link each with the respective argument, you have to scan from last to first. In the example, we have two arguments, text and max_len, and one default, 80, so it must belong to the last argument, max_len. This is awkward. Fortunately, there is a better way: the inspect module.

better. inspect.signature returns an inspect.Signature object, which has a parameters attribute that lets you read an ordered mapping of names to inspect.Parameter objects. Each Parameter instance has attributes such as name, default, and kind. The special value inspect._empty denotes parameters with no default, which makes sense considering that None is a valid—and popular—default value. The kind attribute holds one of five possible values from the _ParameterKind class: 


An inspect.Signature object has a bind method that takes any number of arguments and binds them to the parameters in the signature, applying the usual rules for matching actual arguments to formal parameters. This can be used by a framework to validate arguments prior to the actual function invocation.

Each argument in the function declaration may have an annotation expression preceded by :. If there is a default value, the annotation goes between the argument name and the = sign. To annotate the return value, add -> and another expression between the ) and the : at the tail of the function declaration. The expressions may be of any type. The most common types used in annotations are classes, like str or int, or strings, like 'int > 0', as seen in the annotation for max_len in Example 5-19. No processing is done with the annotations. They are merely stored in the __annotations__ attribute of the function, a dict: >

19. The only thing Python does with annotations is to store them in the __annotations__ attribute of the function. Nothing else: no checks, enforcement, validation, or any other action is performed. In other words, annotations have no meaning to the Python interpreter. They are just metadata that may be used by tools, such as IDEs, frameworks, and decorators. At this writing no tools that use this metadata exist in the standard library, except that inspect.signature() knows how to extract the annotations, as Example 5-20 shows.

) To save you the trouble of writing trivial anonymous functions like lambda a, b: a*b, the operator module provides function equivalents for dozens of arithmetic operators.

Another group of one-trick lambdas that operator replaces are functions to pick items from sequences or read attributes from objects: itemgetter and attrgetter actually build custom functions to do that. Example 5-23 shows

> Because itemgetter uses the [] operator, it supports not only sequences but also mappings and any class that implements __getitem__. A sibling of itemgetter is attrgetter, which creates functions to extract object attributes by name. If you pass attrgetter several attribute names as arguments, it also returns a tuple of values. In addition, if any argument name contains a . (dot), attrgetter navigates through nested objects to retrieve the attribute.

Here is a partial list of functions defined in operator (names starting with _ are omitted, because they are mostly implementation details): >>> [name for name in dir(operator) if not name.startswith('_')]
['abs', 'add', 'and_', 'attrgetter', 'concat', 'contains',
'countOf', 'delitem', 'eq', 'floordiv', 'ge', 'getitem', 'gt',
'iadd', 'iand', 'iconcat', 'ifloordiv', 'ilshift', 'imod', 'imul',
'index', 'indexOf', 'inv', 'invert', 'ior', 'ipow', 'irshift',
'is_', 'is_not', 'isub', 'itemgetter', 'itruediv', 'ixor', 'le',
'length_hint', 'lshift', 'lt', 'methodcaller', 'mod', 'mul', 'ne',
'neg', 'not_', 'or_', 'pos', 'pow', 'rshift', 'setitem', 'sub',
'truediv', 'truth', 'xor'] Most of the 52 names listed are self-evident. The group of names prefixed with i and the name of another operator—e.g., iadd, iand, etc.—correspond to the augmented assignment operators—e.g., +=, &=, etc. These change their first argument in place, if it is mutable; if not, the function works like the one without the i prefix: it simply returns the result of the operation.

the remaining operator functions, methodcaller is the last we will cover. It is somewhat similar to attrgetter and itemgetter in that it creates a function on the fly. The function it creates calls a method by name on the object given as argument, as shown

functools.partial is a higher-order function that allows partial application of a function. Given a function, a partial application produces a new callable with some of the arguments of the original function fixed. This is useful to adapt a function that takes one or more arguments to an API that requires a callback with fewer arguments.

A more useful example involves the unicode.normalize function that we saw in Normalizing Unicode for Saner Comparisons. If you work with text from many languages, you may want to apply unicode.normalize('NFC', s) to any string s before comparing or storing it. If you do that often, it’s handy to have an nfc function to do

The functools.partialmethod function (new in Python 3.4) does the same job as partial, but is designed to work with methods. An

feature. A great introduction to functional programming in Python is A. M. Kuchling’s Python Functional Programming HOWTO. The main focus of that text, however, is on the use of iterators and generators, which are the subject of Chapter 14. fn.py is a package to support functional programming in Python 2 and 3. According to its author, Alexey Kachayev, fn.py provides “implementation of missing features to enjoy FP” in Python. It includes a @recur.tco decorator that implements tail-call optimization for unlimited recursion in Python, among many other functions, data structures, and recipes. The StackOverflow question “Python: Why is functools.partial necessary?” has a highly informative (and funny) reply by Alex Martelli, author of the classic Python in a Nutshell. Jim

lambda, map, filter, and reduce first appeared in Lisp, the original functional language. However, Lisp does not limit what can be done inside a lambda, because everything in Lisp is an expression. Python uses a statement-oriented syntax in which expressions cannot contain statements, and many language constructs are statements—including try/catch, which is what I miss most often when writing lambdas.

lambda. Besides the limited anonymous function syntax, the biggest obstacle to wider adoption of functional programming idioms in Python is the lack of tail-recursion elimination, an optimization that allows memory-efficient computation of a function that makes a recursive call at the “tail” of its body. In another blog post, “Tail Recursion Elimination”, Guido gives several reasons why such optimization is not a good fit for Python. That post is a great read for the technical arguments, but even more so because the first three and most important reasons given are usability issues. It is no accident that Python is a pleasure to use, learn, and teach. Guido made it so.

Beyond the Python-specific syntax constraints, anonymous functions have a serious drawback in every language: they have no name. I am only half joking here. Stack traces are easier to read when functions have names. Anonymous functions are a handy shortcut, people have fun coding with them, but sometimes they get carried away—especially if the language and environment encourage deep nesting of anonymous functions, like JavaScript on Node.js. Lots of nested anonymous functions make debugging and error handling hard. Asynchronous programming in Python is more structured, perhaps because the limited lambda demands it. I promise to write more about asynchronous programming in the future, but this subject must be deferred to Chapter 18. By the way, promises, futures, and deferreds are concepts used in modern asynchronous APIs. Along with coroutines, they provide an escape from the so-called “callback hell.” We’ll see how callback-free asynchronous programming works in From Callbacks to Futures and Coroutines. [

Although design patterns are language-independent, that does not mean every pattern applies to every language. In his 1996 presentation, “Design Patterns in Dynamic Languages”, Peter Norvig states that 16 out of the 23 patterns in the original Design Patterns book by Gamma et al. become either “invisible or simpler” in a dynamic language (slide 9). He was talking about Lisp and Dylan, but many of the relevant dynamic features are also present in Python.

In Python 3.4, the simplest way to declare an ABC is to subclass abc.ABC, as I did in Example 6-1. From Python 3.0 to 3.3, you must use the metaclass= keyword in the class statement (e.g., class Promotion(metaclass=ABCMeta):). Example

Each concrete strategy in Example 6-1 is a class with a single method, discount. Furthermore, the strategy instances have no state (no instance attributes). You could say they look a lot like plain functions, and you would be right. Example 6-3 is a refactoring of Example 6-1, replacing the concrete strategies with simple functions and removing the Promo abstract class.

subtle bug: to add a new promotion strategy, we need to code the function and remember to add it to the promos list, or else the new promotion will work when explicitly passed as an argument to Order, but will not be considered by best_promotion. Read on for a couple of solutions to this issue. Finding Strategies in a Module Modules in Python are also first-class objects, and the standard library provides several functions to handle them. The built-in globals is described as follows in the Python docs: 
globals()
 
Return a dictionary representing the current global symbol table. This is always the dictionary of the current module (inside a function or method, this is the module where it is defined, not the module from which it is called

Another way of collecting the available promotions would be to create a module and put all the  strategy functions there, except for best_promo. In Example 6-8, the only significant change is that the list of strategy functions is built by introspection of a separate module called promotions. Note that Example 6-8 depends on importing the promotions module as well as inspect, which provides high-level introspection functions (the imports are not shown for brevity, because they would normally be at the top of the file). Example 6-8. The promos list is built by introspection of a new promotions module promos = [func for name, func in
                inspect.getmembers(promotions, inspect.isfunction)]

def best_promo(order):
    """Select best discount available
    """
    return max(promo(order) for promo in promos) The function inspect.getmembers returns the attributes of an object—in this case, the promotions module—optionally filtered by a predicate (a boolean function). We use inspect.isfunction to get only the functions from the module. Example

introspection. A more explicit alternative for dynamically collecting promotional discount functions would be to use a simple decorator. We’ll show yet another version of our ecommerce Strategy example in Chapter 7, which deals with function decorators.

At a high level, the approach here was similar to the one we applied to Strategy: replacing with callables the instances of a participant class that implemented a single-method interface. After all, every Python callable implements a single-method interface, and that method is named __call__. Chapter

As Peter Norvig pointed out a couple of years after the classic Design Patterns book appeared, “16 of 23 patterns have qualitatively simpler implementation in Lisp or Dylan than in C++ for at least some uses of each pattern” (slide 9 of Norvig’s “Design Patterns in Dynamic Languages” presentation). Python shares some of the dynamic features of the Lisp and Dylan languages, in particular first-class functions, our focus in this part of the book. From

Vlissides. The refactoring of Strategy and the discussion of Command in this chapter are examples of a more general insight: sometimes you may encounter a design pattern or an API that requires that components implement an interface with a single method, and that method has a generic-sounding name such as “execute”, “run”, or “doIt”. Such patterns or APIs often can be implemented with less boilerplate code in Python using first-class functions or other callables.

Expert Python Programming by Tarek Ziadé (Packt) is one of the best intermediate-level Python books in the market, and its final chapter, “Useful Design Patterns,” presents seven of the classic patterns from a Pythonic perspective.

For a fresh look at patterns from the point of view of a dynamic language with duck typing and first-class functions, Design Patterns in Ruby by Russ Olsen (Addison-Wesley) has many insights that are also applicable to Python. In spite of many the syntactic differences, at the semantic level Python and Ruby are closer to each other than to Java or C++. In Design Patterns in Dynamic Languages (slides), Peter Norvig shows how first-class functions (and other dynamic features) make several of the original design patterns either simpler or unnecessary.

 classrooms around the world, design patterns are frequently taught using Java examples. I’ve heard more than one student claim that they were led to believe that the original design patterns are useful in any implementation language. It turns out that the “classic” 23 patterns from the Gamma et al. book apply to “classic” Java very well in spite of being originally presented mostly in the context of C++—a few have Smalltalk examples in the book. But that does not mean every one of those patterns applies equally well in any language. The authors are explicit right at the beginning of their book that “some of our patterns are supported directly by the less common object-oriented languages” (recall full quote on first page of this chapter). The Python bibliography about design patterns is very thin, compared to that of Java, C++, or Ruby. In Further Reading I mentioned Learning Python Design Patterns by Gennadiy Zlobin, which was published as recently as November 2013. In contrast, Russ Olsen’s Design Patterns in Ruby was published in 2007 and has 384 pages—284 more than Zlobin’s work. Now

the talk “Root Cause Analysis of Some Faults in Design Patterns,” presented by Ralph Johnson at IME/CCSL, Universidade de São Paulo, Nov. 15, 2014. [

A decorator is a callable that takes another function as argument (the decorated function).[40] The decorator may perform some processing with the decorated function, and returns it or replaces it with another function or callable object. In

Strictly speaking, decorators are just syntactic sugar. As we just saw, you can always simply call a decorator like any regular callable, passing another function. Sometimes that is actually convenient, especially when doing metaprogramming—changing program behavior at runtime. To summarize: the first crucial fact about decorators is that they have the power to replace the decorated function with a different one. The second crucial fact is that they are executed immediately when a module is loaded. This is explained next. When

] The main point of Example 7-2 is to emphasize that function decorators are executed as soon as the module is imported, but the decorated functions only run when they are explicitly invoked. This highlights the difference between what Pythonistas call import time and runtime. Considering how decorators are commonly employed in real code, Example 7-2 is unusual in two ways: 
The decorator function is defined in the same module as the decorated functions. A real decorator is usually defined in one module and applied to functions in other modules.

The register decorator returns the same function passed as argument. In practice, most decorators define an inner function and return it.
 Even though the register decorator in Example 7-2 returns the decorated function unchanged, that technique is not useless. Similar decorators are used in many Python web frameworks to add functions to some central registry—for example, a registry mapping URL patterns to functions that generate HTTP responses. Such registration decorators may or may not change the decorated function. The next section shows a practical example. Decorator

Strategy. Recall that our main issue with Example 6-6 is the repetition of the function names in their definitions and then in the promos list used by the best_promo function to determine the highest discount applicable. The repetition is problematic because someone may add a new promotional strategy function and forget to manually add it to the promos list—in which case, best_promo will silently ignore the new strategy, introducing a subtle bug in the system. Example 7-3 solves this problem with a registration decorator.

Most decorators do change the decorated function. They usually do it by defining an inner function and returning it to replace the decorated function. Code that uses inner functions almost always depends on closures to operate correctly. To understand closures, we need to take a step back a have a close look at how variable scopes work in Python.

is not a bug, but a design choice: Python does not require you to declare variables, but assumes that a variable assigned in the body of a function is local. This is much better than the behavior of JavaScript, which does not require variable declarations either, but if you do forget to declare that a variable is local (with var), you may clobber a global variable without knowing. If we want the interpreter to treat b as a global variable in spite of the assignment within the function, we use the global declaration: >

The dis module provides an easy way to disassemble the bytecode of Python functions. Read Examples 7-6 and 7-7 to see the bytecodes for f1 and f2 from Examples 7-4 and 7-5. Example 7-6. Disassembly of the f1 function from Example 7-4 >>> from dis import dis
>>> dis(f1)
  2           

closures are sometimes confused with anonymous functions. The reason why many confuse them is historic: defining functions inside functions is not so common, until you start using anonymous functions. And closures only matter when you have nested functions. So a lot of people learn both concepts at the same time. Actually, a closure is a function with an extended scope that encompasses nonglobal variables referenced in the body of the

Note the similarities of the examples: we call Averager() or make_averager() to get a callable object avg that will update the historical series and calculate the current mean. In Example 7-8, avg is an instance of Averager, and in Example 7-9 it is the inner function, averager. Either way, we just call avg(n) to include n in the series and get the updated mean. It’s obvious where the avg of the Averager class keeps the history: the self.series instance attribute. But where does the avg function in the second example find the series? Note that series is a local variable of make_averager because the initialization series = [] happens in the body of that function. But when avg(10) is called, make_averager has already returned, and its local scope is long gone. Within averager, series is a free variable. This is a technical term meaning a variable that is not bound in the local scope. See Figure

series. Inspecting the returned averager object shows how Python keeps the names of local and free variables in the __code__ attribute that represents the compiled body of the function.

The binding for series is kept in the __closure__ attribute of the returned function avg. Each item in avg.__closure__ corresponds to a name in avg.__code__.co_freevars. These items are cells, and they have an attribute called cell_contents where the actual value can be found.

To summarize: a closure is a function that retains the bindings of the free variables that exist when the function is defined, so that they can be used later when the function is invoked and the defining scope is no longer available. Note that the only situation in which a function may need to deal with external variables that are nonglobal is when it is nested in another function.

The problem is that the statement count += 1 actually means the same as count = count + 1, when count is a number or any immutable type. So we are actually assigning to count in the body of averager, and that makes it a local variable. The same problem affects the total variable. We did not have this problem in Example 7-9 because we never assigned to the series name; we only called series.append and invoked sum and len on it. So we took advantage of the fact that lists are mutable. But with immutable types like numbers, strings, tuples, etc., all you can do is read, but never update. If you try to rebind them, as in count = count + 1, then you are implicitly creating a local variable count. It is no longer a free variable, and therefore it is not saved in the closure. To work around this, the nonlocal declaration was introduced in Python 3. It lets you flag a variable as a free variable even when it is assigned a new value within the function. If a new value is assigned to a nonlocal variable, the binding stored in the closure is changed.

The lack of nonlocal in Python 2 requires workarounds, one of which is described in the third code snippet of PEP 3104 — Access to Names in Outer Scopes, which introduced nonlocal. Essentially the idea is to store the variables the inner functions need to change (e.g., count, total) as items or attributes of some mutable object, like a dict or a simple instance, and bind that object to a free variable. Now

This is the typical behavior of a decorator: it replaces the decorated function with a new function that accepts the same arguments and (usually) returns whatever the decorated function was supposed to return, while also doing some extra processing. Tip

The clock decorator implemented in Example 7-15 has a few shortcomings: it does not support keyword arguments, and it masks the __name__ and __doc__ of the decorated function. Example 7-17 uses the functools.wraps decorator to copy the relevant attributes from func to clocked. Also, in this new version, keyword arguments are correctly handled.

Python has three built-in functions that are designed to decorate methods: property, classmethod, and staticmethod. We will discuss property in Using a Property for Attribute Validation and the others in classmethod Versus staticmethod. Another frequently seen decorator is functools.wraps, a helper for building well-behaved decorators. We used it in Example 7-17. Two of the most interesting decorators in the standard library are lru_cache and the brand-new singledispatch (added in Python 3.4). Both are defined in the functools module.

 very practical decorator is functools.lru_cache. It implements memoization: an optimization technique that works by saving the results of previous invocations of an expensive function, avoiding repeat computations on previously used arguments. The letters LRU stand for Least Recently Used, meaning that the growth of the cache is limited by discarding the entries that have not been read for a while. A


@functools.lru_cache() # 
@clock  # 
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-2) + fibonacci(n-1)

if __name__=='__main__':
    print(fibonacci(6))   
Note that lru_cache must be invoked as a regular function—note the parentheses in the line: @functools.lru_cache(). The reason is that it accepts configuration parameters, as we’ll see shortly.
   
This is an example of stacked decorators: @lru_cache() is applied on the function returned by @clock.


 the way, because lru_cache uses a dict to store the results, and the keys are made from the positional and keyword arguments used in the calls, all the arguments taken by the decorated function must be hashable.

Because we don’t have method or function overloading in Python, we can’t create variations of htmlize with different signatures for each data type we want to handle differently. A common solution in Python would be to turn htmlize into a dispatch function, with a chain of if/elif/elif calling specialized functions like htmlize_str, htmlize_int, etc. This is not extensible by users of our module, and is unwieldy: over time, the htmlize dispatcher would become too big, and the coupling between it and the specialized functions would be very tight. The new functools.singledispatch decorator in Python 3.4 allows each module to contribute to the overall solution, and lets you easily provide a specialized function even for classes that you can’t edit. If you decorate a plain function with @singledispatch, it becomes a generic function: a group of functions to perform the same operation in different ways, depending on the type of the first argument.

@singledispatch marks the base function that handles the object type.
   
Each specialized function is decorated with @«base_function».register(«type»).
   
The name of the specialized functions is irrelevant; _ is a good choice to make this clear.
   
For each additional type to receive special treatment, register a new function. numbers.Integral is a virtual superclass of int.
 

When possible, register the specialized functions to handle ABCs (abstract classes) such as numbers.Integral and abc.MutableSequence instead of concrete implementations like int and list. This allows your code to support a greater variety of compatible types. For example, a Python extension can provide alternatives to the int type with fixed bit lengths as subclasses of numbers.Integral.

 notable quality of the singledispatch mechanism is that you can register specialized functions anywhere in the system, in any module. If you later add a module with a new user-defined type, you can easily provide a new custom function to handle that type. And you can write custom functions for classes that you did not write and can’t change.

@singledispatch is not designed to bring Java-style method overloading to Python. A single class with many overloaded variations of a method is better than a single function with a lengthy stretch of if/elif/elif/elif blocks. But both solutions are flawed because they concentrate too much responsibility in a single code unit—the class or the function. The advantage of @singledispath is supporting modular extension: each module can register a specialized function for each type it supports. Decorators

When parsing a decorator in source code, Python takes the decorated function and passes it as the first argument to the decorator function. So how do you make a decorator accept other arguments? The answer is: make a decorator factory that takes those arguments and returns a decorator, which is then applied to the function to be decorated. Confusing? Sure.

Graham Dumpleton and Lennart Regebro—one of this book’s technical reviewers—argue that decorators are best coded as classes implementing __call__, and not as functions like the examples in this chapter. I agree that approach is better for non-trivial decorators, but to explain the basic idea of this language feature, functions are easier to understand. Chapter

cookbook. Graham Dumpleton has a series of in-depth blog posts about techniques for implementing well-behaved decorators, starting with “How You Implemented Your Python Decorator is Wrong”. His deep expertise in this matter is also nicely packaged in the wrapt module he wrote to simplify the 

implementation of decorators and dynamic function wrappers, which support introspection and behave correctly when further decorated, when applied to methods and when used as descriptors. (Descriptors are the subject of chapter Chapter 20.)

PEP 3104 — Access to Names in Outer Scopes describes the introduction of the nonlocal declaration to allow rebinding of names that are neither local nor global. It also includes an excellent overview of how this issue is resolved in other dynamic languages (Perl, Ruby, JavaScript, etc.) and the pros and cons of the design options available to Python. On a more theoretical level, PEP 227 — Statically Nested Scopes documents the introduction of lexical scoping as an option in Python 2.1 and as a standard in Python 2.2, explaining the rationale and design choices for the implementation of closures in Python.

PEP 443 provides the rationale and a detailed description of the single-dispatch generic functions’ facility. An old (March 2005) blog post by Guido van Rossum, “Five-Minute Multimethods in Python”, walks through an implementation of generic functions (a.k.a. multimethods) using decorators. His code supports multiple-dispatch (i.e., dispatch based on more than one positional argument). Guido’s multimethods code is interesting, but it’s a didactic example. For a modern, production-ready implementation of multiple-dispatch generic functions, check out Reg by Martijn Faassen—author of the model-driven and REST-savvy Morepath web framework. “Closures in Python” is a short blog post by Fredrik Lundh that explains the terminology of closures.

Functions should be black boxes, with their implementation hidden from users. But with dynamic scope, if a function uses free variables, the programmer has to know its internals to set up an environment where it works correctly. On the other hand, dynamic scope is easier to implement, which is probably why it was the path taken by John McCarthy when he created Lisp, the first language to have first-class functions. Paul Graham’s article “The Roots of Lisp” is an accessible explanation of John McCarthy’s original paper about the Lisp language: “Recursive Functions of Symbolic Expressions and Their Computation by Machine, Part I”. McCarthy’s paper is a masterpiece as great as Beethoven’s 9th Symphony. Paul Graham translated it for the rest of us, from mathematics to English and running code.

Today, lexical scope is the norm: free variables are evaluated considering the environment where the function is defined. Lexical scope complicates the implementation of languages with first-class functions, because it requires the support of closures. On the other hand, lexical scope makes source code easier to read. Most languages invented since Algol have lexical scope. For many years, Python lambdas did not provide closures, contributing to the bad name of this feature among functional-programming geeks in the blogosphere. This was fixed in Python 2.2 (December 2001), but the

Prof. Stein also spoke about assignment in a very deliberate way. For example, when talking about a seesaw object in a simulation, she would  say: “Variable s is assigned to the seesaw,” but never “The seesaw is assigned to variable s.” With reference variables, it makes much more sense to say that the variable is assigned to an object, and not the other way around. After all, the object is created before the assignment. Example 8-2 proves that the righthand side of an assignment happens first. Example

is The == operator compares the values of objects (the data they hold), while is compares their identities. We often care about values and not identities, so == appears more frequently than is in Python code. However, if you are comparing a variable to a singleton, then it makes sense to use is. By far, the most common case is checking whether a variable is bound to None. This is the recommended way to do it: x is None And the proper way to write its negation is: x is not None The is operator is faster than ==, because it cannot be overloaded, so Python does not have to find and invoke special methods to evaluate it, and computing is as simple as comparing two integer IDs. In contrast, a == b is syntactic sugar for a.__eq__(b). The __eq__ method inherited from object compares object IDs, so it produces the same result as is. But most built-in types override

Tuples, like most Python collections—lists, dicts, sets, etc.—hold references to objects.[42] If the referenced items are mutable, they may change even if the tuple itself does not. In other words, the immutability of tuples really refers to the physical contents of the tuple data structure (i.e., the references it holds), and does not extend to the referenced objects. Example

For lists and other mutable sequences, the shortcut l2 = l1[:] also makes a copy. However, using the constructor or [:] produces a shallow copy (i.e., the outermost container is duplicated, but the copy is filled with references to the same items held by the original container). This saves memory and causes no problems if all the items are immutable. But if there are mutable items, this may lead to unpleasant surprises. In

The copy module provides the deepcopy and copy functions that return deep and shallow copies of arbitrary objects.

Note that making deep copies is not a simple matter in the general case. Objects may have cyclic references that would cause a naïve algorithm to enter an infinite loop. The deepcopy function remembers the objects already copied to handle cyclic references gracefully.

Also, a deep copy may be too deep in some cases. For example, objects may refer to external resources or singletons that should not be copied. You can control the behavior of both copy and deepcopy by implementing the __copy__() and __deepcopy__() special methods as described in the copy module documentation.

The problem is that each default value is evaluated when the function is defined—i.e., usually when the module is loaded—and the default values become attributes of the function object. So if a default value is a mutable object, and you change it, the change will affect every future call of the function. After running

.
 The problem here is that the bus is aliasing the list that is passed to the constructor. Instead, it should keep its own passenger list. The fix is simple: in __init__, when the passengers parameter is provided, self.passengers should be initialized with a copy of it, as we did correctly in Example 8-

Unless a method is explicitly intended to mutate an object received as argument, you should think twice before aliasing the argument object by simply assigning it to an instance variable in your class. If in doubt, make a copy. Your clients will often be happier.

The del statement deletes names, not objects. An object may be garbage collected as result of a del command, but only if the variable deleted holds the last reference to the object, or if the object becomes unreachable.[43] Rebinding a variable may also cause the number of references to an object to reach zero, causing its destruction.

There is a __del__ special method, but it does not cause the disposal of the instance, and should not be called by your code. __del__ is invoked by the Python interpreter when the instance is about to be destroyed to give it a chance to release external resources. You will seldom need to implement __del__ in your own code, yet some Python beginners spend time coding it for no good reason. The proper use of __del__ is rather tricky. See the __del__ special method documentation in the “Data Model” chapter of The Python Language Reference.

CPython, the primary algorithm for garbage collection is reference counting. Essentially, each object keeps count of how many references point to it. As soon as that refcount reaches zero, the object is immediately destroyed: CPython calls the __del__ method on the object (if defined) and then frees the memory allocated to the object.

Other implementations of Python have more sophisticated garbage collectors that do not rely on reference counting, which means the __del__ method may not be called immediately when there are no more references to the object. See “PyPy, Garbage Collection, and a Deadlock” by A. Jesse Jiryu Davis for discussion of improper and proper use of __del__. To

The presence of references is what keeps an object alive in memory. When the reference count of an object reaches zero, the garbage collector disposes of it. But sometimes it is useful to have a reference to an object that does not keep it around longer than necessary. A common use case is a cache. Weak references to an object do not increase its reference count. The object that is the target of a reference is called the referent. Therefore, we say that a weak reference does not prevent the referent from being garbage collected. Weak references are useful in caching applications because you don’t want the cached objects to be kept alive just because they are referenced by the cache.

Tip Example 8-17 is a console session, and the Python console automatically binds the _ variable to the result of expressions that are not None. This interfered with my intended demonstration but also highlights a practical matter: when trying to micro-manage memory we are often surprised by hidden, implicit assignments that create new references to our objects. The _ console variable is one example. Traceback objects are another common source of unexpected references.

The weakref module documentation makes the point that the weakref.ref class is actually a low-level interface intended for advanced uses, and that most programs are better served by the use of the weakref collections and finalize. In other words, consider using WeakKeyDictionary, WeakValueDictionary, WeakSet, and finalize (which use weak references internally) instead of creating and handling your own weakref.ref instances by hand.

The class WeakValueDictionary implements a mutable mapping where the values are weak references to objects. When a referred object is garbage collected elsewhere in the program, the corresponding key is automatically removed from WeakValueDictionary. This is commonly used for caching. Our

Tip A temporary variable may cause an object to last longer than expected by holding a reference to it. This is usually not a problem with local variables: they are destroyed when the function returns. But in Example 8-19, the for loop variable cheese is a global variable and will never go away unless explicitly deleted.

counterpart to the WeakValueDictionary is the WeakKeyDictionary in which the keys are weak references. The weakref.WeakKeyDictionary documentation hints on possible uses: [A WeakKeyDictionary] can be used to associate additional data with an object owned by other parts of an application without adding attributes to those objects. This can be especially useful with objects that override attribute accesses. The weakref module also provides a WeakSet, simply described in the docs as “Set class that keeps weak references to its elements. An element will be discarded when no strong reference to it exists any more.” If you need to build a class that is aware of every one of its instances, a good solution is to create a class attribute with a WeakSet to hold the references to the instances. Otherwise, if a regular set was used, the instances would never be garbage collected, because the class itself would have strong references to them, and classes live as long as the Python process unless you deliberately delete them. These

Not every Python object may be the target, or referent, of a weak reference. Basic list and dict instances may not be referents, but a plain subclass of either can solve this problem easily: class MyList(list):
    """list subclass whose instances may be weakly referenced"""

a_list = MyList(range(10))

# a_list can be the target of a weak reference
wref_to_a_list = weakref.ref(a_list) A set instance can be a referent, and that’s why a set was used in Example 8-17. User-defined types also pose no problem, which explains why the silly Cheese class was needed in Example 8-19. But int and tuple instances cannot be targets of weak references, even if subclasses of those types are created. Most of these limitations are implementation details of CPython that may not apply to other Python iterpreters. They are the result of internal optimizations, some of which are discussed in the following (highly optional) section.

was surprised to learn that, for a tuple t, t[:] does not make a copy, but returns a reference to the same object. You also get a reference to the same tuple if you write tuple(t).[

.
 The same behavior can be observed with instances of str, bytes, and frozenset. Note that a frozenset is not a sequence, so fs[:] does not work if fs is a frozenset. But fs.copy() has the same effect: it cheats and returns a reference to the same object, and not a copy at all, as Example 8-21 shows.

!
 The sharing of string literals is an optimization technique called interning. CPython uses the same technique with small integers to avoid unnecessary duplication of “popular” numbers like 0, –1, and 42. Note that CPython does not intern all strings or integers, and the criteria it uses to do so is an undocumented implementation detail.

Never depend on str or int interning! Always use == and not is to compare them for equality. Interning is a feature for internal use of the Python interpreter.

 Hellmann wrote a long series of excellent blog posts titled Python Module of the Week, which became a book, The Python Standard Library by Example. His posts “copy – Duplicate Objects” and “weakref – Garbage-Collectable References to Objects” cover some of the topics we just discussed.

Wesley Chun, author of the Core Python series of books, made a great presentation about many of the topics covered in this chapter during OSCON 2013. You can download the slides from the “Python 103: Memory Model & Best Practices” talk page. There is also a YouTube video of a longer presentation Wesley gave at EuroPython 2011, covering not only the theme of this chapter but also the use of special methods.

Garbage collection in CPython is done primarily by reference counting, which is easy to implement, but is prone to memory leaking when there are reference cycles, so with version 2.0 (October 2000) a generational garbage collector was implemented, and it is able to dispose of unreachable objects kept alive by reference cycles. But

 you are into the subject of garbage collectors, you may want to read Thomas Perl’s paper “Python Garbage Collector Implementations: CPython, PyPy and GaS”, from which I learned the bit about the safety of the open().write() in CPython.

Every object-oriented language has at least one standard way of getting a string representation from any object. Python has two: 
repr()
 
Return a string representing the object as the developer wants to see it.
 
str()
 
Return a string representing the object as the user wants to see it.
 As you know, we implement the special methods __repr__ and __str__ to support repr() and 

There are two additional special methods to support alternative representations of objects: __bytes__ and __format__. The __bytes__ method is analogous to __str__: it’s called by bytes() to get the object represented as a byte sequence. Regarding __format__, both the built-in function format() and the str.format() method call it to get string displays of objects using special formatting codes. 


__iter__ makes a Vector2d iterable; this is what makes unpacking work (e.g, x, y = my_vector). We implement it simply by using a generator expression to yield the components one after the other.


Converting x and y to float in __init__ catches errors early, which is helpful in case Vector2d is called with unsuitable arguments.

__repr__ builds a string by interpolating the components with {!r} to get their repr; because Vector2d is iterable, *self feeds the x and y components to format.
 

Method __eq__ in Example 9-2 works for Vector2d operands but also returns True when comparing Vector2d instances to other iterables holding the same numeric values (e.g., Vector(3, 4) == [3, 4]). This may be considered a feature or a bug. Further discussion needs to wait until Chapter 13, when we cover operator overloading.

Example 9-3. Part of vector2d_v1.py: this snippet shows only the frombytes class method, added to the Vector2d definition in vector2d_v0.py (Example 9-2)     @classmethod   
    def frombytes(cls, octets):   
        typecode = chr(octets[0])   
        memv = memoryview(octets[1:]).cast(typecode)   
        return cls(*memv)      
Class method is modified by the classmethod decorator.
   
No self argument; instead, the class itself is passed as cls.
   
Read the typecode from the first byte.
   
Create a memoryview from the octets binary sequence and use the typecode to cast it.[52]
   
Unpack the memoryview resulting from the cast into the pair of arguments needed for the constructor.


Let’s start with classmethod. Example 9-3 shows its use: to define a method that operates on the class and not on instances. classmethod changes the way the method is called, so it receives the class itself as the first argument, instead of an instance. Its most common use is for alternative constructors, like frombytes in Example 9-3. Note how the last line of frombytes actually uses the cls argument by invoking it to build a new instance: cls(*memv). By convention, the first parameter of a class method should be named cls (but Python doesn’t care how it’s named). In contrast, the staticmethod decorator changes a method so that it receives no special first argument. In essence, a static method is just like a plain function that happens to live in a class body, instead of being defined at the module level. Example 9-4 contrasts the operation of classmethod and staticmethod. Example

Note The classmethod decorator is clearly useful, but I’ve never seen a compelling use case for staticmethod. If you want to define a function that does not interact with the class, just define it in the module. Maybe the function is closely related even if it never touches the class, so you want to them nearby in the code. Even so, defining the function right before or after the class in the same module is close enough for all practical purposes.

Displays The format() built-in function and the str.format() method delegate the actual formatting to each type by calling their .__format__(format_spec) method. The format_spec is a formatting specifier, which is either: 
The second argument in format(my_obj, format_spec), or

Whatever appears after the colon in a replacement field delimited with {} inside a format string used with str.format()
 For example: >

" If a class has no __format__, the method inherited from object returns str(my_object). Because Vector2d has a __str__, this works: >

When choosing the letter for the custom format code I avoided overlapping with codes used by other types. In Format Specification Mini-Language we see that integers use the codes 'bcdoxXn', floats use 'eEfFgGn%', and strings use 's'. So I picked 'p' for polar coordinates. Because each class interprets these codes independently, reusing a code letter in a custom format for a new type is not an error, but may be confusing to users. To

To make a Vector2d hashable, we must implement __hash__ (__eq__ is also required, and we already have it). We also need to make vector instances immutable, as we’ve seen in What Is Hashable?. Right now, anyone can do v1.x = 7 and there is nothing in the code to suggest that changing a Vector2d is forbidden.

Example 9-7. vector2d_v3.py: only the changes needed to make Vector2d immutable are shown here; see full listing in Example 9-9 class Vector2d:
    typecode = 'd'

    def __init__(self, x, y):
        self.__x = float(x)   
        self.__y = float(y)

    @property   
    def x(self):   
        return self.__x   

    @property   
    def y(self):
        return self.__y


property. Now that our vectors are reasonably immutable, we can implement the __hash__ method. It should return an int and ideally take into account the hashes of the object attributes that are also used in the __eq__ method, because objects that compare equal should have the same hash. The __hash__ special method documentation suggests using the bitwise XOR operator (^) to mix the hashes of the components, so that’s what we do.

It’s not strictly necessary to implement properties or otherwise protect the instance attributes to create a hashable type. Implementing __hash__ and __eq__ correctly is all it takes. But the hash value of an instance is never supposed to change, so this provides an excellent opportunity to talk about read-only properties. If

Name mangling is about safety, not security: it’s designed to prevent accidental access and not intentional wrongdoing

The name mangling functionality is not loved by all Pythonistas, and neither is the skewed look of names written as self.__x. Some prefer to avoid this syntax and use just one underscore prefix to “protect” attributes by convention (e.g., self._x). Critics of the automatic double-underscore mangling suggest that concerns about accidental attribute clobbering should be addressed by naming conventions. This is the full quote from the prolific Ian Bicking, cited at the beginning of this chapter: Never, ever use two leading underscores. This is annoyingly private. If name clashes are a concern, use explicit name mangling instead (e.g., _MyThing_blahblah). This is essentially the same thing as double-underscore, only it’s transparent where double underscore obscures.[55] The single underscore prefix has no special meaning to the Python interpreter when used in attribute names, but it’s a very strong convention among Python programmers that you should not access such attributes from outside the class.[56] It’s easy to respect the privacy of an object that marks its attributes with a single _, just as it’s easy respect the convention that variables in ALL_CAPS should be treated as constants.

To conclude: the Vector2d components are “private” and our Vector2d instances are “immutable”—with scare quotes—because there is no way to make them really private and immutable.

dict. Warning A __slots__ attribute inherited from a superclass has no effect. Python only takes into account __slots__ attributes defined in each class individually.

Attribute By default, Python stores instance attributes in a per-instance dict named __dict__. As we saw in Practical Consequences of How dict Works, dictionaries have a significant memory overhead because of the underlying hash table used to provide fast access. If you are dealing with millions of instances with few attributes, the __slots__ class attribute can save a lot of memory, by letting the interpreter store the instance attributes in a tuple instead

Example 9-11. vector2d_v3_slots.py: the slots attribute is the only addition to Vector2d class Vector2d:
    __slots__ = ('__x', '__y')

    typecode = 'd'

    # methods follow (omitted in book listing) By defining __slots__ in the class, you are telling the interpreter: “These are all the instance attributes in this class.” Python then stores them in a tuple-like structure in each instance, avoiding the memory overhead of the per-instance __dict__. This can make a huge difference in memory usage if your have millions of instances active at the same time. Tip

Tip If you are handling millions of objects with numeric data, you should really be using NumPy arrays (see NumPy and SciPy), which are not only memory-efficient but have highly optimized functions for numeric processing, many of which operate on the entire array at once. I designed the Vector2d class just to provide context when discussing special methods, because I try to avoid vague foo and bar examples when I can.

Warning When __slots__ is specified in a class, its instances will not be allowed to have any other attributes apart from those named in __slots__. This is really a side effect, and not the reason why __slots__ exists. It’s considered bad form to use __slots__ just to prevent users of your class from creating new attributes in the instances if they want to. __slots__ should used for optimization, not for programmer restraint. It

There is another special per-instance attribute that you may want to keep: the __weakref__ attribute is necessary for an object to support weak references (covered in Weak References). That attribute is present by default in instances of user-defined classes. However, if the class defines __slots__, and you need the instances to be targets of weak references, then you need to include '__weakref__' among the attributes named in __slots__.

.
 If your program is not handling millions of instances, it’s probably not worth the trouble of creating a somewhat unusual and tricky class whose instances may not accept dynamic attributes or may not support weak references. Like any optimization, __slots__ should be used only if justified by a present need and when its benefit is proven by careful profiling.

A distinctive feature of Python is how class attributes can be used as default values for instance attributes. In Vector2d there is the typecode class attribute. It’s used twice in the __bytes__ method, but we read it as self.typecode by design. Because Vector2d instances are created without a typecode attribute of their own, self.typecode will get the Vector2d.typecode class attribute by default. But if you write to an instance attribute that does not exist, you create a new instance attribute—e.g., a typecode instance attribute—and the class attribute by the same name is untouched. However, from then on, whenever the code handling that instance reads self.typecode, the instance typecode will be retrieved, effectively shadowing the class attribute by the same name. This opens the possibility of customizing an individual instance with a different typecode. The

you want to change a class attribute you must set it on the class directly, not through an instance. You could change the default typecode for all instances (that don’t have their own typecode) by doing this: >>> Vector2d.typecode = 'f' However, there is an idiomatic Python way of achieving a more permanent effect, and being more explicit about the change. Because class attributes are public, they are inherited by subclasses, so it’s common practice to subclass just to customize a class data attribute. The Django class-based views use this technique extensively.

Is vector2d_v3.py (Example 9-9) more Pythonic than vector2d_v0.py (Example 9-2)? The Vector2d class in vector2d_v3.py certainly exhibits more Python features. But whether the first or the last Vector2d implementation is more idiomatic depends on the context where it would be used. Tim Peter’s Zen of Python says: Simple is better than complex. A Pythonic object should be as simple as the requirements allow—and not a parade of language 

Throughout the chapter, I mentioned how design choices in the examples were informed by studying the API of standard Python objects. If this chapter can be summarized in one sentence, this is it: To build Pythonic objects, observe how real Python objects behave. —
Ancient Chinese proverb

Python Essential Reference, 4th Edition, by David Beazley
 
Covers the data model in detail in the context of Python 2.6 and Python

covered every special method related to object representation, except __index__. It’s used to coerce an object to an integer index in the specific context of sequence slicing, and was created to solve a need in NumPy. In practice, you and I are not likely to need to implement __index__ unless we decide to write a new numeric data type, and we want it to be usable as arguments to __getitem__. If you are curious about it, A.M. Kuchling’s What’s New in Python 2.5 has a short explanation, and PEP 357 — Allowing Any Object to be Used for Slicing details the need for __index__, from the perspective of an implementor of a C-extension, Travis Oliphant, the lead author of NumPy.

The 1996 article “How to Display an Object as a String: printString and displayString” by Bobby Woolf discusses the implementation of the printString and displayString methods in that language. From that article, I borrowed the pithy descriptions “the way the developer wants to see it” and “the way the user wants to see it” when defining repr() and str() in Object Representations.

Ward Cunningham, inventor of the wiki and an Extreme Programming pioneer, recommends asking “What’s the simplest thing that could possibly work?” The idea is to focus on the goal.[59] Implementing setters and getters up front is a distraction from the goal. In Python, we can simply use public attributes knowing we can change them to properties later, if the need arises.

Perl doesn’t have an infatuation with enforced privacy. It would prefer that you stayed out of its living room because you weren’t invited, not because it has a shotgun. —
Larry Wall
Creator of Perl
 Python and Perl are polar opposites in many regards, but Larry and Guido seem to agree on object privacy. Having

Don’t check whether it is-a duck: check whether it quacks-like-a duck, walks-like-a duck, etc, etc, depending on exactly what subset of duck-like behavior you need to play your language-games with. (comp.lang.python, Jul. 26, 2000) —
Alex Martelli

. We could make Vector(3, 4) and Vector(3, 4, 5) work, by taking arbitrary arguments with *args in __init__, but the best practice for a sequence constructor is to take the data as an iterable argument in the constructor, like all built-in sequence types do. Example

When a Vector has more than six components, the string produced by repr() is abbreviated with ... as seen in the last line of Example 10-1. This is crucial in any collection type that may contain a large number of items, because repr is used for debugging (and you don’t want a single large object to span thousands of lines in your console or log). Use the reprlib module to produce limited-length representations,

Because of its role in debugging, calling repr() on an object should never raise an exception. If something goes wrong inside your implementation of __repr__, you must deal with the issue and do your best to produce some serviceable output that gives the user a chance of identifying the target object. Note

In the context of object-oriented programming, a protocol is an informal interface, defined only in documentation and not in code. For example, the sequence protocol in Python entails just the __len__ and __getitem__ methods. Any class Spam that implements those methods with the standard signature and semantics can be used anywhere a sequence is expected. Whether Spam is a subclass of this or that is irrelevant; all that matters is that it provides the necessary methods.

As you can see, even slicing is supported—but not very well. It would be better if a slice of a Vector was also a Vector instance and not a array. The old FrenchDeck class has a similar problem: when you slice it, you get a list. In the case of Vector, a lot of functionality is lost when slicing produces plain arrays. Consider the built-in sequence types: every one of them, when sliced, produces a new instance of its own type, and not of some other type. To make Vector produce slices as Vector instances, we can’t just delegate the slicing to array. We need to analyze the arguments we get in __getitem__ and do the right thing.

In Example 10-5, calling dir(slice) reveals an indices attribute, which turns out to be a very interesting but little-known method. Here is what help(slice.indices) reveals: 
S.indices(len) -> (start, stop, stride)
 
  Assuming a sequence of length len, calculate the start and stop indices, and the stride length of the extended slice described by S. Out of bounds indices are clipped in a manner consistent with the handling of normal slices.
 In other words, indices exposes the tricky logic that’s implemented in the built-in sequences to gracefully handle missing or negative indices and slices that are longer than the target sequence. This method produces “normalized” tuples of nonnegative start, stop, and stride integers adjusted to fit within the bounds of a sequence of the given length.

As I write this, the slice.indices method is apparently not documented in the online Python Library Reference. The Python Python/C API Reference Manual documents a similar C-level function, PySlice_GetIndicesEx. I discovered slice.indices while exploring slice objects in the Python console, using dir() and help(). Yet another evidence of the value of the interactive console as a discovery tool. In

Note Excessive use of isinstance may be a sign of bad OO design, but handling slices in __getitem__ is a justified use case. Note in Example 10-6 the test  against numbers.Integral—an Abstract Base Class. Using ABCs in insinstance tests makes an API more flexible and future-proof. Chapter 11 explains why. Unfortunately, there is no ABC for slice in the Python 3.4 standard library.

 Vector2d, we provided read-only access to x and y using the @property decorator (Example 9-7). We could write four properties in Vector, but it would be tedious. The __getattr__ special method provides a better way. “The __getattr__ method is invoked by the interpreter when attribute lookup fails. In simple terms, given the expression my_obj.x, Python checks if the my_obj instance has an attribute named x; if not, the search goes to the class
(my_obj.__class__), and then up the inheritance graph.[61] If the x attribute is not found, then the __getattr__ method defined in the class of my_obj is called with self and the name of the attribute as a string (e.g., 'x'). Example

The inconsistency in Example 10-9 was introduced because of the way __getattr__ works: Python only calls that method as a fall back, when the object does not have the named attribute. However, after we assign v.x = 10, the v object now has an x attribute, so __getattr__ will no longer be called to retrieve v.x: the interpreter will just return the value 10 that is bound to v.x. On the other hand, our implementation of __getattr__ pays no attention to instance attributes other than self._components, from where it retrieves the values of the “virtual attributes” listed in shortcut_names. We need to customize the logic for setting attributes in our Vector class in order to avoid this inconsistency. Recall that in the latest Vector2d examples from Chapter 9, trying to assign to the .x or .y instance attributes raised AttributeError. In Vector we want the same exception with any attempt at assigning to all single-letter lowercase attribute names, just to avoid confusion. To do that, we’ll implement __setattr__

Even without supporting writing to the Vector components, here is an important takeaway from this example: very often when you implement __getattr__ you need to code __setattr__ as well, to avoid inconsistent behavior in your objects. If

zip returns a generator that produces tuples on demand.
   
Here we build a list from it just for display; usually we iterate over the generator.
   
zip has a surprising trait: it stops without warning when one of the iterables is exhausted.[64]
   
The itertools.zip_longest function behaves differently: it uses an optional fillvalue (None by default) to complete missing values so it can generate tuples until the last iterable is exhausted.
 The enumerate built-in is another generator function often used in for loops to avoid manual handling of index variables. If you are not familiar with enumerate, you should definitely check it out in the “Built-in functions” documentation. The zip and enumerate built-ins, along with several other generator functions in the standard library, are covered in Generator Functions in the Standard Library. Vector

In the Python documentation, you can often tell when a protocol is being discussed when you see language like “a file-like object.” This is a quick way of saying “something that behaves sufficiently like a file, by implementing the parts of the file interface that are relevant in the context.” You may think that implementing only part of a protocol is sloppy, but it has the advantage of keeping things simple. Section 3.3 of the “Data Model” chapter suggests: When implementing a class that emulates any built-in type, it is important that the emulation only be implemented to the degree that it makes sense for the object being modeled. For example, some sequences may work well with retrieval of individual elements, but extracting a slice may not make sense. —
“Data Model” chapter of The Python Language Reference
 When we don’t need to code nonsense methods just to fulfill some over-designed interface contract and keep the compiler happy, it becomes easier to follow the KISS principle. I

We will implement a new ABC to see how that works, but Alex Martelli and I don’t want to encourage you to start writing your own ABCs left and right. The risk of over-engineering with ABCs is very high. Warning ABCs, like descriptors and metaclasses, are tools for building frameworks. Therefore, only a very small minority of Python developers can create ABCs without imposing unreasonable limitations and needless work on fellow programmers.

Tip When you follow established protocols, you improve your chances of leveraging existing standard library and third-party code, thanks to duck typing. The

The signature of the __setitem__ special method is defined in The Python Language Reference in “3.3.6. Emulating container types”. Here we named the arguments deck, position, card—and not self, key, value as in the language reference—to show that every Python method starts life as a plain function, and naming the first argument self is merely a convention. This is OK in a console session, but in a Python source file it’s much better to use self, key, and value as documented. The trick is that set_card knows that the deck object has an attribute named _cards, and _cards must be a mutable sequence. The set_card function is then attached to the FrenchDeck class as the __setitem__ special method. This is an example of monkey patching: changing a class or module at runtime, without touching the source code. Monkey patching is powerful, but the code that does the actual patching is very tightly coupled with the program to be patched, often handling private and undocumented parts. Besides

Does this matter?  It depends on the context!  For such purposes as deciding how best to cook a waterfowl once you’ve bagged it, for example, specific observable traits (not all of them—plumage, for example, is de minimis in such a context), mostly texture and flavor (old-fashioned phenetics!), may be far more relevant than cladistics.  But for other issues, such as susceptibility to different pathogens (whether you’re trying to raise waterfowl in captivity, or preserve them in the wild), DNA closeness can matter much more… So, by very loose analogy with these taxonomic revolutions in the world of waterfowls, I’m recommending supplementing (not entirely replacing—in certain contexts it shall still serve) good old duck typing with… goose typing! What goose typing means is: isinstance(obj, cls) is now just fine… as long as cls is an abstract base class—in other words, cls’s metaclass is abc.ABCMeta. You can find many useful existing abstract classes in collections.abc (and additional ones in the numbers module of The Python Standard Library).[69] Among

Sometimes you don’t even need to register a class for an ABC to recognize it as a subclass! That’s the case for the ABCs whose essence boils down to a few special methods. For example: >>> class Struggle:
...     def __len__(self): return 23
...
>>> from collections import abc
>>> isinstance(Struggle(), abc.Sized)
True As you see, abc.Sized recognizes Struggle as “a subclass,” with no need for registration, as implementing the special method named __len__ is all it takes

So, here’s my valediction: whenever you’re implementing a class embodying any of the concepts represented in the ABCs in numbers, collections.abc, or other framework you may be using, be sure (if needed) to subclass it from, or register it into, the corresponding ABC. At the start of your programs using some library or framework defining classes which have omitted to do that, perform the registrations yourself; then, when you must check for (most typically) an argument being, e.g, “a sequence,” check whether: isinstance(the_arg, collections.abc.Sequence) And,

] On the other hand, it’s usually OK to perform an insinstance check against an ABC if you must enforce an API contract: “Dude, you have to implement this if you want to call me,” as technical reviewer Lennart Regebro put it. That’s particularly useful in systems that have a plug-in architecture. Outside of frameworks, duck typing is often simpler and more flexible than type checks.

There are two modules named abc in the standard library. Here we are talking about collections.abc. To reduce loading time, in Python 3.4, it’s implemented outside of the collections package, in Lib/_collections_abc.py), so it’s imported separately from collections. The other abc module is just abc (i.e., Lib/abc.py) where the abc.ABC class is defined. Every ABC depends on it, but we don’t need to import it ourselves except to create a new ABC. Figure

Iterable, Container, and Sized
 
Every collection should either inherit from these ABCs or at least implement compatible protocols. Iterable supports iteration with __iter__, Container supports the in operator with __contains__, and Sized supports len() with __len__.
 
Sequence, Mapping, and Set
 
These are the main immutable collection types, and each has a mutable subclass. A detailed diagram for MutableSequence is in Figure 11-2; for MutableMapping and MutableSet, there are diagrams in Chapter 3 (Figures 3-1 and 3-2).
 
MappingView
 
In Python 3, the objects returned from the mapping methods .items(), .keys(), and .values() inherit from ItemsView, ValuesView, and ValuesView, respectively. The first two also inherit the rich interface of Set, with all the operators we saw in Set Operations.
 
Callable and Hashable
 
These ABCs are not so closely related to collections, but collections.abc was the first package to define ABCs in the standard library, and these two were deemed important enough to be included. I’ve never seen subclasses of either Callable or Hashable. Their main use is to support the insinstance built-in as a safe way of determining whether an object is callable or hashable.[73]
 
Iterator
 
Note that iterator subclasses Iterable. 

The numbers package defines the so-called “numerical tower” (i.e., this linear hierarchy of ABCs), where Number is the topmost superclass, Complex is its immediate subclass, and so on, down to Integral:

Somewhat surprisingly, decimal.Decimal is not registered as a virtual subclass of numbers.Real. The reason is that, if you need the precision of Decimal in your program, then you want to be protected from accidental mixing of decimals with other less precise numeric types, particularly floats. After


 So if you need to check for an integer, use isinstance(x, numbers.Integral) to accept int, bool (which subclasses int) or other integer types that may be provided by external libraries that register their types with the numbers ABCs. And to satisfy your check, you or the users of your API may always register any compatible type as a virtual subclass of numbers.Integral. If, on the other hand, a value can be a floating-point type, you write isinstance(x, numbers.Real), and your code will happily take bool, int, float, fractions.Fraction, or any other noncomplex numerical type provided by an external library, such as NumPy, which is suitably registered.

Tip An abstract method can actually have an implementation. Even if it does, subclasses will still be forced to override it, but they will be able to invoke the abstract method with super(), adding functionality to it instead of implementing from scratch. See the abc module documentation for details on @abstractmethod usage. The

The best way to declare an ABC is to subclass abc.ABC or any other ABC. However,

ABCs. Besides the @abstractmethod, the abc module defines the @abstractclassmethod, @abstractstaticmethod, and @abstractproperty decorators. However, these last three are deprecated since Python 3.3, when it became possible to stack decorators on top of @abstractmethod, making the others redundant. For example, the preferred way to declare an abstract class method is: class MyABC(abc.ABC):
    @classmethod
    @abc.abstractmethod
    def an_abstract_classmethod(cls, ...):

The order of stacked function decorators usually matters, and in the case of @abstractmethod, the documentation is explicit: When abstractmethod() is applied in combination with other method descriptors, it should be applied as the innermost decorator, …[78] In other words, no other decorator may appear between @abstractmethod and the def statement.

Example 11-13 illustrates an idiom worth mentioning: in __init__, self._balls stores list(iterable) and not just a reference to iterable (i.e., we did not merely assign iterable to self._balls). As mentioned before,[79] this makes our LotteryBlower flexible because the iterable argument may be any iterable type. At the same time, we make sure to store its items in a list so we can pop items. And even if we always get lists as the iterable argument, list(iterable) produces a copy of the argument, which is a good practice considering we will be removing items from it and the client may not be expecting the list of items she provided to be changed.[80

Virtual subclasses do not inherit from their registered ABCs, and are not checked for conformance to the ABC interface at any time, not even when they are instantiated. It’s up to the subclass to actually implement all the methods needed to avoid runtime errors.

Note that because of the registration, the functions issubclass and isinstance act as if TomboList is a subclass of Tombola: >>> from tombola import Tombola
>>> from tombolist import TomboList
>>> issubclass(TomboList, Tombola)
True
>>> t = TomboList(range(100))
>>> isinstance(t, Tombola)
True However, inheritance is guided by a special class attribute named __mro__—the Method Resolution Order. It basically lists the class and its superclasses in the order Python uses to search for methods.[82] If you inspect the __mro__ of TomboList, you’ll see that it lists only the “real” superclasses—list and object: >>> TomboList.__mro__
(<class 'tombolist.TomboList'>, <class 'list'>, <class 'object'>) Tombola is not in Tombolist.__mro__, so Tombolist does not inherit any methods from Tombola.


__subclasses__()
 
Method that returns a list of the immediate subclasses of the class. The list does not include virtual subclasses.

even if register can now be used as a decorator, it’s more widely deployed as a function to register classes defined elsewhere. For example, in the source code for the collections.abc module, the built-in types tuple, str, range, and memoryview are registered as virtual subclasses of Sequence like this: Sequence.register(tuple)
Sequence.register(str)
Sequence.register(range)
Sequence.register(memoryview) Several other built-in types are registered to ABCs in _collections_abc.py. Those registrations happen only when that module is imported, which is OK because you’ll have to import it anyway to get the ABCs: you need access to MutableMapping to be able to write isinstance(my_dict, MutableMapping). We

In his Waterfowl and ABCs essay, Alex shows that a class can be recognized as a virtual subclass of an ABC even without registration. Here is his example again, with an added test using issubclass: >>> class Struggle:
...     def __len__(self): return 23
...
>>> from collections import abc
>>> isinstance(Struggle(), abc.Sized)
True
>>> issubclass(Struggle, abc.Sized)
True Class Struggle is considered a subclass of abc.Sized by the issubclass function (and, consequently, by isinstance as well) because abc.Sized implements a special class method named __subclasshook__.

.
 If you are interested in the details of the subclass check, see the source code for the ABCMeta.__subclasscheck__ method in Lib/abc.py. Beware: it has lots of ifs and two recursive calls. The __subclasshook__ adds some duck typing DNA to the whole goose typing proposition. You can have formal interface definitions with ABCs, you can make isinstance checks everywhere, and still have a completely unrelated class play along just because it implements a certain method (or because it does whatever it takes to convince a __subclasshook__ to vouch for it). Of course, this only works for ABCs that do provide a __subclasshook__. Is it a good idea to implement __subclasshook__ in our own ABCs? Probably not. All the implementations of __subclasshook__ I’ve seen in the Python source code are in ABCs like Sized that declare just one special method, and they simply check for that special method name. Given their “special” status, you can be pretty sure that any method named __len__ does what you expect. But even in the realm of special methods and fundamental ABCs, it can be risky to make such assumptions.

. I am not ready to believe that any class named Spam that implements or inherits load, pick, inspect, and loaded is guaranteed to behave as a Tombola. It’s better to let the programmer affirm it by subclassing Spam from Tombola, or at least registering: Tombola.register(Spam). Of course, your __subclasshook__ could also check method signatures and other features, but I just don’t think it’s worthwhile. Chapter

Here is a fitting quote to end this chapter: Although ABCs facilitate type checking, it’s not something that you should overuse in a program. At its heart, Python is a dynamic language that gives you great flexibility. Trying to enforce type constraints everywhere tends to result in code that is more complicated than it needs to be. You should embrace Python’s flexibility.[86] —
David Beazley and Brian Jones
Python Cookbook
 Or, as technical reviewer Leonardo Rochael wrote: “If you feel tempted to create a custom ABC, please first try to solve your problem through regular duck-typing.

About Face is by far the best book about UI design I’ve read—and I’ve read a few. Letting go of metaphors as a design paradigm, and replacing it with “idiomatic interfaces” was the most valuable thing I learned from Cooper’s work. As mentioned, Cooper does not deal with APIs, but the more I think about his ideas, the more I see how they apply to Python. The fundamental protocols of the language are what Cooper calls “idioms.” Once we learn what a “sequence” is we can apply that knowledge in different contexts. This is a main theme of Fluent Python: highlighting the fundamental idioms of the language, so your code is concise, effective, and readable—for a fluent Pythonista.

Before Python 2.2, it was not possible to subclass built-in types such as list or dict. Since then, it can be done but there is a major caveat: the code of the built-ins (written in C) does not call special methods overridden by user-defined classes. A good short description of the problem is in the documentation for PyPy, in “Differences between PyPy and CPython”, section Subclasses of built-in types: Officially, CPython has no rule at all for when exactly overridden method of subclasses of built-in types get implicitly called or not. As an approximation, these methods are never called by other built-in methods of the same object. For example, an overridden __getitem__() in a subclass of dict will not be called by e.g. the built-in get() method.

Subclassing built-in types like dict or list or str directly is error-prone because the built-in methods mostly ignore user-defined overrides. Instead of subclassing the built-ins, derive your classes from the collections module using UserDict, UserList, and UserString, which are designed to be easily extended.

The ambiguity of a call like d.pong() is resolved because Python follows a specific order when traversing the inheritance graph. That order is called MRO: Method Resolution Order. Classes have an attribute called __mro__ holding a tuple of references to the superclasses in MRO order, from the current class all the way to the object class.

The recommended way to delegate method calls to superclasses is the super() built-in function, which became easier to use in Python 3, as method pingpong of class D in Example 12-4 illustrates.[91]. However, it’s also possible, and sometimes convenient, to bypass the MRO and invoke a method on a superclass directly. For example, the D.ping method could be written as:     def ping(self):
        A.ping(self)  # instead of super().ping()
        print('post-ping:', self) Note that when calling an instance method directly on a class, you must pass self explicitly, because you are accessing an unbound method.

The MRO is computed using an algorithm called C3. The canonical paper on the Python MRO explaining C3 is Michele Simionato’s “The Python 2.3 Method Resolution Order”. If you are interested in the subtleties of the MRO, Further Reading has other pointers. But don’t fret too much about this, the algorithm is sensible; as Simionato writes: […] unless you make strong use of multiple inheritance and you have non-trivial hierarchies, you don’t need to understand the C3 algorithm, and you can easily skip this paper. To

. Distinguish Interface Inheritance from Implementation Inheritance When dealing with multiple inheritance, it’s useful to keep straight the reasons why subclassing is done in the first place. The main reasons are: 
Inheritance of interface creates a subtype, implying an “is-a” relationship.

Inheritance of implementation avoids code duplication by reuse.
 In practice, both uses are often simultaneous, but whenever you can make the intent clear, do it. Inheritance for code reuse is an implementation detail, and it can often be replaced by composition and delegation. On the other hand, interface inheritance is the backbone of a framework.

Make Interfaces Explicit with ABCs In modern Python, if a class is designed to define an interface, it should be an explicit ABC. In Python ≥ 3.4, this means: subclass abc.ABC or another ABC (see ABC Syntax Details if you need to support older Python versions). 3.

Reuse If a class is designed to provide method implementations for reuse by multiple unrelated subclasses, without implying an “is-a” relationship, it should be an explicit mixin class. Conceptually, a mixin does not define a new type; it merely bundles methods for reuse. A mixin should never be instantiated, and concrete classes should not inherit only from a mixin. Each mixin should provide a single specific behavior, implementing few and very closely related methods.

There is no formal way in Python to state that a class is a mixin, so it is highly recommended that they are named with a …Mixin suffix. Tkinter does not follow this advice, but if it did, XView would be XViewMixin, Pack would be PackMixin, and so on with all the classes where I put the «mixin» tag in Figure 12-

Because an ABC can implement concrete methods, it works as a mixin as well. An ABC also defines a type, which a mixin does not. And an ABC can be the sole base class of any other class, while a mixin should never be subclassed alone except by another, more specialized mixin—not a common arrangement in real code. One restriction applies to ABCs and not to mixins: the concrete methods implemented in an ABC should only collaborate with methods of the same ABC and its superclasses. This implies that concrete methods in an ABC are always for convenience, because everything they do, a user of the class can also do by calling other methods of the ABC.

Concrete classes should have zero or at most one concrete superclass.[93] In other words, all but one of the superclasses of a concrete class should be ABCs or mixins. For example, in the following code, if Alpha is a concrete class, then Beta and Gamma must be ABCs or mixins: class MyConcreteClass(Alpha, Beta, Gamma):
    """This is a concrete

Users If some combination of ABCs or mixins is particularly useful to client code, provide a class that brings them together in a sensible way. Grady Booch calls this an aggregate class.[94] For example, here is the complete source code for tkinter.Widget: class Widget(BaseWidget, Pack, Place, Grid):
    """Internal class.

    Base class for a widget which can be positioned with the
    geometry managers Pack, Place or Grid."""
    pass The body of Widget is empty, but the class provides a useful service: it brings together four superclasses so that anyone who needs to create a new widget does not need to remember all those mixins, or wonder if they need to be declared in a certain order in a class statement. A better example of this is the Django ListView class, which we’ll discuss shortly, in A Modern Example: Mixins in Django Generic Views. 8.

. Once you get comfortable with inheritance, it’s too easy to overuse it. Placing objects in a neat hierarchy appeals to our sense of order; programmers do it just for fun. However, favoring composition leads to more flexible designs. For example, in the case of the tkinter.Widget class, instead of inheriting the methods from all geometry managers, widget instances could hold a reference to a geometry manager, and invoke its methods. After all, a Widget should not “be” a geometry manager, but could use the services of one via delegation. Then you could add a new geometry manager without touching the widget class hierarchy and without worrying about name clashes. Even with single inheritance, this principle enhances flexibility, because subclassing is a form of tight coupling, and tall inheritance trees tend to be brittle. Composition and delegation can replace the use of mixins to make behaviors available to different classes, but cannot replace the use of interface inheritance to define a hierarchy of types. We

Raymond Hettinger’s post Python’s super() considered super! explains the workings of super and multiple inheritance in Python from a positive perspective. It was written in response to Python’s Super is nifty, but you can’t use it (a.k.a. Python’s Super Considered Harmful) by James Knight. Despite the titles of those posts, the problem is not really the super built-in—which in Python 3 is not as ugly as it was in Python 2. The real issue is multiple inheritance, which is inherently complicated and tricky. Michele Simionato goes beyond criticizing and actually offers a solution in his Setting Multiple Inheritance Straight: he implements traits, a constrained form of mixins that originated in the Self language. Simionato has a long series of illuminating blog posts about multiple inheritance in Python, including The wonders of cooperative inheritance, or using super in Python 3; Mixins considered harmful, part 1 and part 2; and Things to Know About Python Super, part 1, part 2 and part 3. The oldest posts use the Python 2 super syntax, but are still relevant. I

the first edition of Grady Booch’s Object Oriented Analysis and Design, 3E (Addison-Wesley, 2007), and highly recommend it as a general primer on object oriented thinking, independent of programming language. It is a rare book that covers multiple inheritance without prejudice.

Types. It’s easy to support the unary operators. Simply implement the appropriate special method, which will receive just one argument: self. Use whatever logic makes sense in your class, but stick to the fundamental rule of operators: always return a new object. In other words, do not modify self, but create and return a new instance of a suitable type.

Everybody expects that x == +x, and that is true almost all the time in Python, but I found two cases in the standard library where x != +x. The first case involves the decimal.Decimal class. You can have x != +x if x is a Decimal instance created in an arithmetic context and +x is then evaluated in a context with different settings. For example, x is calculated in a context with a certain precision, but the precision of the context is changed and then +x is evaluated. See Example 13-2 for a demonstration. Example

The second case where x != +x you can find in the collections.Counter documentation. The Counter class implements several arithmetic operators, including infix + to add the tallies from two Counter instances. However, for practical reasons, Counter addition discards from the result any item with a negative or zero count. And the prefix + is a shortcut for adding an empty Counter, therefore it produces a new Counter preserving only the tallies that are greater than 

Special methods implementing unary or infix operators should never change their operands. Expressions with such operators are expected to produce results by creating new objects. Only augmented assignment operators may change the first operand (self), as discussed in Augmented Assignment Operators.

Do not confuse NotImplemented with NotImplementedError. The first, NotImplemented, is a special singleton value that an infix operator special method should return to tell the interpreter it cannot handle a given operand. In contrast, NotImplementedError is an exception that stub methods in abstract classes raise to warn that they must be overwritten by subclasses.

Often, __radd__ can be as simple as that: just invoke the proper operator, therefore delegating to __add__ in this case. This applies to any commutative operator;

The problems in Examples 13-8 and 13-9 actually go deeper than obscure error messages: if an operator special method cannot return a valid result because of type incompatibility, it should return NotImplemented and not raise TypeError. By returning NotImplemented, you leave the door open for the implementer of the other operand type to perform the operation when Python tries the reversed method call. In the spirit of duck typing, we will refrain from testing the type of the other operand, or the type of its elements. We’ll catch the exceptions and return NotImplemented.

Python 3.4 does not have an infix operator for the dot product. However, as I write this, Python 3.5 pre-alpha already implements PEP 465 — A dedicated infix operator for matrix multiplication, making the @ sign available for that purpose (e.g., a @ b is the dot product of a and b). The @ operator is supported by the special methods __matmul__, __rmatmul__, and __imatmul__, named for “matrix multiplication.” These methods are not used anywhere in the standard library at this time, but are recognized by the interpreter in Python 3.5 so the NumPy team—and the rest of us—can support the @ operator in user-defined types. The parser was also changed to handle the infix @ (a @ b is a syntax error in Python 3.4). Just

A Vector is also considered equal to a tuple or any iterable with numeric items of equal value.
 The last one of the results in Example 13-12 is probably not desirable. I really have no hard rule about this; it depends on the application context. But the Zen of Python says: In the face of ambiguity, refuse the temptation to guess. Excessive liberality in the evaluation of operands may lead to surprising results, and programmers hate surprises. Taking a clue from Python itself, we can see that [1,2] == (1, 2) is False. Therefore, let’s be conservative and do some type checking. If the second operand is a Vector instance (or an instance of a Vector subclass), then use the same logic as the current __eq__. Otherwise, return NotImplemented and let Python handle 

As I write this, the rich comparison method documentation states: “The truth of x==y does not imply that x!=y is false. Accordingly, when defining __eq__(), one should also define __ne__() so that the operators will behave as expected.” That was true for Python 2, but in Python 3 that’s not good advice, because a useful default __ne__ implementation is inherited from the object class, and it’s rarely necessary to override it. The new behavior is documented in Guido’s What’s New in Python 3.0, in the section “Operators And Special Methods.” The documentation bug is recorded as issue 4395. After

Note that the += operator is more liberal than + with regard to the second operand. With +, we want both operands to be of the same type (AddableBingoCage, in this case), because if we accepted different types this might cause confusion as to the type of the result. With the +=, the situation is clearer: the lefthand object is updated in place, so there’s no doubt about the type of the result. Tip

validated the contrasting behavior of + and += by observing how the list built-in type works. Writing my_list + x, you can only concatenate one list to another list, but if you write my_list += x, you can extend the lefthand list with items from any iterable x on the righthand side. This is consistent with how the list.extend() method works: it accepts any iterable 

In general, if a forward infix operator method (e.g., __mul__) is designed to work only with operands of the same type as self, it’s useless to implement the corresponding reverse method (e.g., __rmul__) because that, by definition, will only be invoked when dealing with an operand of a different type. This

The functools.total_ordering function is a class decorator (supported in Python 2.7 and later) that automatically generates methods for all rich comparison operators in any class that defines at least a couple of them.

 you are curious about operator method dispatching in languages with dynamic typing, two seminal readings are “A Simple Technique for Handling Multiple Polymorphism” by Dan Ingalls (member of the original Smalltalk team) and “Arithmetic and Double Dispatching in Smalltalk-80” by Kurt J. Hebel and Ralph Johnson (Johnson became famous as one of the authors of the original Design Patterns book). Both papers provide deep insight into the power of polymorphism in languages with dynamic typing, like Smalltalk, Python, and Ruby. Python does not use double dispatching for handling operators as described in those articles. The Python algorithm using forward and reverse operators is easier for user-defined classes to support

In contrast, classic double dispatching is a general technique you can use in Python or any OO language beyond the specific context of infix operators, and in fact Ingalls, Hebel, and Johnson use very different examples to describe it. The

.
 That is why any Python sequence is iterable: they all implement __getitem__. In fact, the standard sequences also implement __iter__, and yours should too, because the special handling of __getitem__ exists for backward compatibility reasons and may be gone in the future (although it is not deprecated as I write this

Tip As of Python 3.4, the most accurate way to check whether an object x is iterable is to call iter(x) and handle a TypeError exception if it isn’t. This is more accurate than using isinstance(x, abc.Iterable), because iter(x) also considers the legacy __getitem__ method, while the Iterable ABC does not.

Because the only methods required of an iterator are __next__ and __iter__, there is no way to check whether there are remaining items, other than to call next() and catch StopInteration. Also, it’s not possible to “reset” an iterator. If you need to start over, you need to call iter(…) on the iterable that built the iterator in the first place. Calling iter(…) on the iterator itself won’t help, because—as mentioned—Iterator.__iter__ is implemented by returning self, so this will not reset a depleted iterator. To 

It may be tempting to implement __next__ in addition to __iter__ in the Sentence class, making each Sentence instance at the same time an iterable and iterator over itself. But this is a terrible idea. It’s also a common anti-pattern, according to Alex Martelli who has a lot of experience with Python code reviews. The “Applicability” section[107] of the Iterator design pattern in the GoF book says: Use the Iterator pattern 
to access an aggregate object’s contents without exposing its internal representation.

to support multiple traversals of aggregate objects.

to provide a uniform interface for traversing different aggregate structures (that is, to support polymorphic iteration).
 To “support multiple traversals” it must be possible to obtain multiple independent iterators from the same iterable instance, and each iterator must keep its own internal state, so a proper implementation of the pattern requires each call to iter(my_iterable) to create a new, independent, iterator.

An iterable should never act as an iterator over itself. In other words, iterables must implement __iter__, but not __next__. On the other hand, for convenience, iterators should be iterable. An iterator’s __iter__ should just return self. Now

The only syntax distinguishing a plain function from a generator function is the fact that the latter has a yield keyword somewhere in its body. Some argued that a new keyword like gen should be used for generator functions instead of def, but Guido did not agree. His arguments are in PEP 255 — Simple Generators.[

Any Python function that contains the yield keyword is a generator function.
   
Usually the body of a generator function has loop, but not necessarily; here I just repeat yield three times.

 find it helpful to be strict when talking about the results obtained from a generator: I say that a generator yields or produces values. But it’s confusing to say a generator “returns” values. Functions return values. Calling a generator function returns a generator. A generator yields or produces values. A generator doesn’t “return” values in the usual way: the return statement in the body of a generator function causes StopIteration to be raised by the generator object.

Whenever you are using Python 3 and start wondering “Is there a lazy way of doing this?”, often the answer is “Yes.” The re.finditer function is a lazy version of re.findall which, instead of a list, returns a generator producing re.MatchObject instances on demand. If there are many matches, re.finditer saves a lot of memory. Using it, our third version of Sentence is now lazy: it only produces the next word when it is needed. 

 generator expression can be understood as a lazy version of a list comprehension: it does not eagerly build a list, but returns a generator that will lazily produce the items on demand. In other words, if a list comprehension is a factory of lists, a generator expression is a factory of generators. Example

Tip When a generator expression is passed as the single argument to a function or constructor, you don’t need to write a set of parentheses for the function call and another to enclose the generator expression. A single pair will do, like in the Vector call from the __mul__ method in Example 10-16, reproduced here. However, if there are more function arguments after the generator expression, you need to enclose it in parentheses to avoid a SyntaxError: def __mul__(self, scalar):
    if isinstance(scalar, numbers.Real):
        return Vector(n * scalar for n in self)
    else:
        return NotImplemented The

 def __iter__(self):
        result = type(self.begin + self.step)(self.begin)   
        forever = self.end is None   
        index = 0
        while forever or result < self.end:   
            yield result   
            index += 1
            result = self.begin + self.step * index

The ArithmeticProgression class from Example 14-11 works as intended, and is a clear example of the use of a generator function to implement the __iter__ special method. However, if the whole point of a class is to build a generator by implementing __iter__, the class can be reduced to a generator function. A generator function is, after all, a generator factory. Example

However, itertools.count never stops, so if you call list(count()), Python will try to build a list larger than available memory and your machine will be very grumpy long before the call fails. On the other hand, there is the itertools.takewhile function: it produces a generator that consumes another generator and stops when a given predicate evaluates to False.

Table 14-1. Filtering generator functions ModuleFunctionDescription itertools compress(it, selector_it) Consumes two iterables in parallel; yields items from it whenever the corresponding item in selector_it is truthy itertools dropwhile(predicate, it) Consumes it skipping items while predicate computes truthy, then yields every remaining item (no further checks are made) (built-in) filter(predicate, it) Applies predicate to each item of iterable, yielding the item if predicate(item) is truthy; if predicate is None, only truthy items are yielded itertools filterfalse(predicate, it) Same as filter, with the predicate logic negated: yields items whenever predicate computes falsy itertools islice(it, stop) or islice(it, start, stop, step=1) Yields items from a slice of it, similar to s[:stop] or s[start:stop:step] except it can be any iterable, and the operation is lazy itertools takewhile(predicate, it) Yields items while predicate computes truthy, then stops and no

exhausted. Table 14-2. Mapping generator functions ModuleFunctionDescription itertools accumulate(it, [func]) Yields accumulated sums; if func is provided, yields the result of applying it to the first pair of items, then to the first result and next item, etc. (built-in) enumerate(iterable, start=0) Yields 2-tuples of the form (index, item), where index is counted from start, and item is taken from the iterable (built-in) map(func, it1, [it2, …, itN]) Applies func to each item of it, yielding the result; if N iterables are given, func must take N arguments and the iterables will be consumed in parallel itertools starmap(func, it) Applies func to each item of it, yielding the result; the input iterable should yield iterable items iit, and func is applied as func(*iit) Example

Table 14-3. Generator functions that merge multiple input iterables ModuleFunctionDescription itertools chain(it1, …, itN) Yield all items from it1, then from it2 etc., seamlessly itertools chain.from_iterable(it) Yield all items from each iterable produced by it, one after the other, seamlessly; it should yield iterable items, for example, a list of iterables itertools product(it1, …, itN, repeat=1) Cartesian product: yields N-tuples made by combining items from each input iterable like nested for loops could produce; repeat allows the input iterables to be consumed more than once (built-in) zip(it1, …, itN) Yields N-tuples built from items taken from the iterables in parallel, silently stopping when the first iterable is exhausted itertools zip_longest(it1, …, itN, fillvalue=None) Yields N-tuples built from items taken from the iterables in parallel, stopping only when the last iterable is exhausted, filling the blanks with the fillvalue

Table 14-4. Generator functions that expand each input item into multiple output items ModuleFunctionDescription itertools combinations(it, out_len) Yield combinations of out_len items from the items yielded by it itertools combinations_with_replacement(it, out_len) Yield combinations of out_len items from the items yielded by it, including combinations with repeated items itertools count(start=0, step=1) Yields numbers starting at start, incremented by step, indefinitely itertools cycle(it) Yields items from it storing a copy of each, then yields the entire sequence repeatedly, indefinitely itertools permutations(it, out_len=None) Yield permutations of out_len items from the items yielded by it; by default, out_len is len(list(it)) itertools repeat(item, [times]) Yield the given item repeadedly, indefinetly unless a number of times is given The

common use of repeat: providing a fixed argument in map; here it provides the 5 multiplier.


Table 14-5. Rearranging generator functions ModuleFunctionDescription itertools groupby(it, key=None) Yields 2-tuples of the form (key, group), where key is the grouping criterion and group is a generator yielding the items in the group (built-in) reversed(seq) Yields items from seq in reverse order, from last to first; seq must be a sequence or implement the __reversed__ special method itertools tee(it, n=2) Yields a tuple of n generators, each yielding the items of the input iterable independently Example

] As you can see, yield from i replaces the inner for loop completely. The use of yield from in this example is correct, and the code reads better, but it seems like mere syntactic sugar. Besides replacing a loop, yield from creates a channel connecting the inner generator directly to the client of the outer generator. This channel becomes really important when generators are used as coroutines and not only produce but also consume values from the client code. Chapter 16 dives into coroutines, and has several pages explaining why yield from is much more than syntactic sugar. After

As we’ve seen, Python calls iter(x) when it needs to iterate over an object x. But iter has another trick: it can be called with two arguments to create an iterator from a regular function or any callable object. In this usage, the first argument must be a callable to be invoked repeatedly (with no arguments) to yield values, and the second argument is a sentinel: a marker value which, when returned by the callable, causes the iterator to raise StopIteration instead of yielding the sentinel. The

whatever argument is passed to .send() becomes the value of the corresponding yield expression inside the generator function body. In other words, .send() allows two-way data exchange between the client code and the generator—in contrast with .__next__(), which only lets the client receive data from the generator. This is such a major “enhancement” that it actually changes the nature of generators: when used in this way, they become coroutines. David Beazley—probably the most prolific writer and speaker about coroutines in the Python community—warned in a famous PyCon US 2009 tutorial: 
Generators produce data for iteration

Coroutines are consumers of data

To keep your brain from exploding, you don’t mix the two concepts together

Coroutines are not related to iteration

Note: There is a use of having yield produce a value in a coroutine, but it’s not tied to iteration.[117]
 —
David Beazley
“A Curious Course on Coroutines and Concurrency”


else block will run only if and when the while loop exits because the condition became falsy (i.e., not when the while is aborted with a break).
 
try
 
The else block will only run if no exception is raised in the try block. The official docs also state: “Exceptions in the else clause are not handled by the preceding except clauses.”
 In all cases, the else clause is also skipped if an exception or a return, break, or continue statement causes control to jump out of the main block of the compound 

 think else is a very poor choice for the keyword in all cases except if. It implies an excluding alternative, like “Run this loop, otherwise do that,” but the semantics for else in loops is the opposite: “Run this loop, then do that.” This suggests then as a better keyword—which would also make sense in the try context: “Try this, then do that.” However, adding a new keyword is a breaking change to the language, and Guido avoids it like the plague.

In the case of try/except blocks, else may seem redundant at first. After all, the after_call() in the following snippet will run only if the dangerous_call() does not raise an exception, correct? try:
    dangerous_call()
    after_call()
except OSError:
    log('OSError...') However, doing so puts the after_call() inside the try block for no good reason. For clarity and correctness, the body of a try block should only have the statements that may generate the expected exceptions. This

Context manager objects exist to control a with statement, just like iterators exist to control a for statement. The with statement was designed to simplify the try/finally pattern, which guarantees that some operation is performed after a block of code, even if the block is aborted because of an exception, a return or sys.exit() call. The code in the finally clause usually releases a critical resource or restores some previous state that was temporarily changed. The context manager protocol consists of the __enter__ and __exit__ methods. At the start of the with, __enter__ is invoked on the context manager object. The role of the finally clause is played by a call to __exit__  on the context manager object at the end of the with block.

 TextIOWrapper.__exit__ method is called and closes the file.
 The first callout in Example 15-1 makes a subtle but crucial point: the context manager object is the result of evaluating the expression after with, but the value bound to the target variable (in the as clause) is the result of calling __enter__ on the context manager object.

 just happens that in Example 15-1, the open() function returns an instance of TextIOWrapper, and its __enter__ method returns self. But the __enter__ method may also return some other object instead of the context manager. When control flow exits the with block in any way, the __exit__ method is invoked on the context manager object, not on whatever is returned by __enter__. The as clause of the with statement is optional. In the case of open, you’ll always need it to get a reference to the file, but some context managers return None because they have no useful object to give back to the user. Example

When real applications take over standard output, they often want to replace sys.stdout with another file-like object for a while, then switch back to the original. The contextlib.redirect_stdout context manager does exactly that: just pass it the file-like object that will stand in for sys.stdout.

contextmanager The @contextmanager decorator reduces the boilerplate of creating a context manager: instead of writing a whole class with __enter__/__exit__ methods, you just implement a generator with a single yield that should produce whatever you want the __enter__ method to return. In a generator decorated with @contextmanager, yield is used to split the body of the function in two parts: everything before the yield will be executed at the beginning of the while block when the interpreter calls __enter__; the code after yield will run when __exit__ is called at the end of the block. Here

. A key point that Raymond Hettinger made in his PyCon US 2013 keynote is that with is not just for resource management, but it’s a tool for factoring out common setup and teardown code, or any pair of operations that need to be done before and after another procedure (

generator Note the error message: it’s quite clear. The initial call next(my_coro) is often described as “priming” the coroutine (i.e., advancing it to the first yield to make it ready for use as a live coroutine). To

It’s crucial to understand that the execution of the coroutine is suspended exactly at the yield keyword. As mentioned before, in an assignment statement, the code to the right of the = is evaluated before the actual assignment happens. This means that in a line like b = yield a, the 

value of b will only be set when the coroutine is activated later by the client code. It takes some effort to get used to this fact, but understanding it is essential to make sense of the use of yield in asynchronous programming, as we’ll see later. Execution

Example 16-7 suggests one way of terminating coroutines: you can use send with some sentinel value that tells the coroutine to exit. Constant built-in singletons like None and Ellipsis are convenient sentinel values. Ellipsis has the advantage of being quite unusual in data streams. Another sentinel value I’ve seen used is StopIteration—the class itself, not an instance of it (and not raising it). In other words, using it like: my_coro.send(StopIteration). Since Python 2.5, generator objects have two methods that allow the client to explicitly send exceptions into the coroutine—throw and close: 
generator.

.
 Note that the value of the return expression is smuggled to the caller as an attribute of the StopIteration exception. This is a bit of a hack, but it preserves the existing behavior of generator objects: raising StopIteration when exhausted. Example

from The first thing to know about yield from is that it is a completely new language construct. It does so much more than yield that the reuse of that keyword is arguably misleading. Similar constructs in other languages are called await, and that is a much better name because it conveys a crucial point: when a generator gen calls yield from subgen(), the subgen takes over and will yield values to the caller of gen; the caller will in effect drive subgen directly. Meanwhile gen will be blocked, waiting until subgen terminates.[

The main feature of yield from is to open a bidirectional channel from the outermost caller to the innermost subgenerator, so that values can be sent and yielded back and forth directly from them, and exceptions can be thrown all the way in without adding a lot of exception handling boilerplate code in the intermediate coroutines. This is what enables coroutine delegation in a way that was not possible before. The use of yield from requires a nontrivial arrangement of code. To talk about the required moving parts, PEP 380 uses some terms in a very specific way: 


The Meaning of yield from While developing PEP 380, Greg Ewing—the author—was questioned about the complexity of the proposed semantics. One of his answers was “For humans, almost all the important information is contained in one paragraph near the top.” He then quoted part of the draft of PEP 380 which at the time read as follows: “When the iterator is another generator, the effect is the same as if the body of the subgenerator were inlined at the point of the yield from expression. Furthermore, the subgenerator is allowed to execute a return statement with a value, and that value becomes the value of the yield from expression.”[136] Those soothing words are no longer part of the PEP—because they don’t cover all the corner cases. But they are OK as a first approximation.

David Beazley is the ultimate authority on Python generators and coroutines. The Python Cookbook, 3E (O’Reilly) he coauthored with Brian Jones has numerous recipes with coroutines. Beazley’s PyCon tutorials on the subject are legendary for their depth and breadth. The first was at PyCon US 2008: “Generator Tricks for Systems Programmers”. PyCon US 2009 saw the legendary “A Curious Course on Coroutines and Concurrency” (hard-to-find video links for all three parts: part 1, part 2, part 3). His most recent tutorial from PyCon 2014 in Montréal was “Generators: The Final Frontier,” in which he tackles more concurrency examples—so it’s really more about topics in Chapter 18 of Fluent Python. Dave can’t resist making brains explode in his classes, so in the last part of “The Final Frontier,” coroutines replace the classic Visitor pattern

Coroutines allow new ways of organizing code, and just as recursion or polymorphism (dynamic dispatch), it takes some time getting used to their possibilities. An interesting example of classic algorithm rewritten with coroutines is in the post “Greedy algorithm with coroutines,” by James Powell.

Brett Slatkin’s Effective Python (Addison-Wesley) has an excellent short chapter titled “Consider Coroutines to Run Many Functions Concurrently” (available online as a sample chapter). That chapter includes the best example of driving generators with yield from I’ve seen: an implementation of John Conway’s Game of Life in which coroutines are used to manage the state of each cell as the game runs. The example code for Effective Python can be found in a GitHub repository. I refactored the code for the Game of Life example—separating the functions and classes that

The main features of the concurrent.futures package are the ThreadPoolExecutor and ProcessPoolExecutor classes, which implement an interface that allows you to submit callables for execution in different threads or processes, respectively. The classes manage an internal pool of worker threads or processes, and a queue of tasks to be executed. But the interface is very high level and we don’t need to know about any of those details for a simple use case like our flag downloads. Example

Note that the download_one function from Example 17-3 is essentially the body of the for loop in the download_many function from Example 17-2. This is a common refactoring when writing concurrent code: turning the body of a sequential for loop into a function to be called concurrently. The

As of Python 3.4, there are two classes named Future in the standard library: concurrent.futures.Future and asyncio.Future. They serve the same purpose: an instance of either Future class represents a deferred computation that may or may not have completed. This is similar to the Deferred class in Twisted, the Future class in Tornado, and Promise objects in various JavaScript libraries.

happens. Both types of Future have a .done() method that is nonblocking and returns a Boolean that tells you whether the callable linked to that future has executed or not. Instead of asking whether a future is done, client code usually asks to be notified. That’s why both Future classes have an .add_done_callback() method: you give it a callable, and the callable will be invoked with the future as the single argument when the future is done. There is also a .result() method, which works the same in both classes when the future is done: it returns the result of the callable, or re-raises whatever exception might have been thrown when the callable was executed. However, when the future is not done, the behavior of the result method is very different between the two flavors of Future. In a concurrency.futures.Future instance, invoking f.result() will block the caller’s thread until the result is ready.

available. An important thing to know about futures in general is that you and I should not create them: they are meant to be instantiated exclusively by the concurrency framework, be it concurrent.futures or asyncio. It’s easy to understand why: a Future represents something that will eventually happen, and the only way to be sure that something will happen is to schedule its execution. Therefore, concurrent.futures.Future instances are created only as the result of scheduling something for execution with a concurrent.futures.Executor subclass.

GIL The CPython interpreter is not thread-safe internally, so it has a Global Interpreter Lock (GIL), which allows only one thread at a time to execute Python bytecodes. That’s why a single Python process usually cannot use multiple CPU cores at the same time.[150] When we write Python code, we have no control over the GIL, but a built-in function or an extension written in C can release the GIL while running time-consuming tasks. In fact, a Python library coded in C can manage the GIL, launch its own OS threads, and take advantage of all available CPU cores. This complicates the code of the library considerably, and most library authors don’t do it. However, all standard library functions that perform blocking I/O release the GIL when waiting for a result from the OS. This means Python programs that are I/O bound can benefit from using threads at the Python level: while one Python thread is waiting for a response from the network, the blocked I/O function releases the GIL so another thread can run.

Every blocking I/O function in the Python standard library releases the GIL, allowing other threads to run. The time.sleep() function also releases the GIL. Therefore, Python threads are perfectly usable in I/O-bound applications, despite the GIL.

The Executor.map function is easy to use but it has a feature that may or may not be helpful, depending on your needs: it returns the results exactly in the same order as the calls are started: if the first call takes 10s to produce a result, and the others take 1s each, your code will block for 10s as it tries to retrieve the first result of the generator returned by map. After that, you’ll get the remaining results without blocking because they will be done. That’s OK when you must have all the results before proceeding, but often it’s preferable to get the results as they are ready, regardless of the order they were submitted. To do that, you need a combination of the Executor.submit method and the futures.as_completed function, as we saw in Example 17-4. We’ll come back to this technique in Using futures.as_completed.

The combination of executor.submit and futures.as_completed is more flexible than executor.map because you can submit different callables and arguments, while executor.map is designed to run the same callable on the different arguments. In addition, the set of futures you pass to futures.as_completed may come from more than one executor—perhaps some were created by a ThreadPoolExecutor instance while others are from a ProcessPoolExecutor. In

rid of the Global Interpreter Lock?”). Also worth reading are posts by Guido van Rossum and Jesse Noller (contributor of the multiprocessing package): “It isn’t Easy to Remove the GIL” and “Python Threads and the Global Interpreter Lock.” Finally, David Beazley has a detailed exploration on the inner workings of the GIL: “Understanding the Python GIL.”[155] In slide #54 of the presentation, Beazley reports some alarming results, including a 20× increase in processing time for a particular benchmark with the new GIL algorithm introduced in Python 3.2. However, Beazley apparently used an empty while True: pass to simulate CPU-bound work, and that is not realistic. The issue is not significant with real workloads, according to a comment by Antoine Pitrou—who implemented the new GIL algorithm—in the bug report submitted by Beazley. While the GIL is real problem and is not likely to go away soon, Jesse Noller and Richard Oudkerk contributed a library to make it easier to work around it in CPU-bound applications: the multiprocessing package, which emulates the threading API across processes, along with supporting infrastructure of locks, queues, pipes, shared memory, etc. The package was introduced in PEP 371 — Addition of the multiprocessing package to the standard library. The official documentation for the package is a 93 KB .rst file—that’s about 63 pages—making it one of the longest

High Performance Python (O’Reilly) by Micha Gorelick and Ian Ozsvald and The Python Standard Library by Example (Addison-Wesley), by Doug Hellmann, also cover threads and processes. For a modern take on concurrency without threads or callbacks, Seven Concurrency Models in Seven Weeks, by Paul Butcher (Pragmatic Bookshelf) is an excellent read. I love its subtitle: “When Threads Unravel.” In that book, threads and locks are covered in Chapter 1, and the remaining six chapters are devoted to modern alternatives to concurrent programming,

Warning Never use time.sleep(…) in asyncio coroutines unless you want to block the main thread, therefore freezing the event loop and probably the whole application as well. If a coroutine needs to spend some time doing nothing, it should yield from asyncio.sleep(DELAY

The use of the @asyncio.coroutine decorator is not mandatory, but highly recommended: it makes the coroutines stand out among regular functions, and helps with debugging by issuing a warning when a coroutine is garbage collected without being yielded from—which means some operation was left unfinished and is likely a bug. This is not a priming decorator.

coroutines, everything is protected against interruption by default. You must explicitly yield to let the rest of the program run. Instead of holding locks to synchronize the operations of multiple threads, you have coroutines that are “synchronized” by definition: only one of them is running at any time. And when you want to give up control, you use yield or yield from to give control back to the scheduler. That’s why it is possible to safely cancel a coroutine: by definition, a coroutine can only be cancelled when it’s suspended at a yield point, so you can perform cleanup by handling the CancelledError exception. We’ll now see how the asyncio.Future class differs

There are a lot of new concepts to grasp in asyncio but the overall logic of Example 18-5 is easy to follow if you employ a trick suggested by Guido van Rossum himself: squint and pretend the yield from keywords are not there. If you do that, you’ll notice that the code is as easy to read as plain old sequential code. For



This was followed by a discussion of how coroutines solve the main problems of callbacks: loss of context when carrying out multistep asynchronous tasks, and lack of a proper context for error handling. The

There are, however, excellent presentations about asyncio. The best I found is Brett Slatkin’s “Fan-In and Fan-Out: The Crucial Components of Concurrency,” subtitled “Why do we need Tulip? (a.k.a., PEP 3156—asyncio),” which he presented at PyCon 2014 in Montréal (video). In 30 minutes, Slatkin shows a simple web crawler example, highlighting how asyncio is intended to be used. Guido van Rossum is in the audience and mentions that he also wrote a web crawler as a motivating example for asyncio; Guido’s code does not depend on aiohttp—it uses only the standard library. Slatkin also wrote the insightful post “Python’s asyncio Is for Composition, Not Raw Performance.”

this. Web services are going to be an important use case for asyncio. Your code will likely depend on the aiohttp library led by Andrew Svetlov. You’ll also want to set up an environment to test your error handling code, and the Vaurien “chaos TCP proxy” designed by Alexis Métaireau and Tarek Ziadé is invaluable for that. Vaurien was created for the Mozilla Services project and lets you introduce delays and random errors into the TCP traffic between your program and backend servers such as databases and web services providers. Soapbox

The FrozenJSON class has a limitation: there is no special handling for attribute names that are Python keywords.

syntax You can always do this, of course: >>> getattr(grad, 'class')
1982 But the idea of FrozenJSON is to provide convenient access to the data, so a better solution is checking whether a key in the mapping given to FrozenJSON.__init__ is a keyword, and if so, append an _ to it, so the attribute can be read like this: >>> grad.class_
1982 This

Such problematic keys are easy to detect in Python 3 because the str class provides the s.isidentifier() method, which tells you whether s is a valid Python identifier according to the language grammar. But turning a key that is not a valid identifier into valid attribute name is not trivial. Two simple solutions would be raising an exception or replacing the invalid keys with generic names like attr_0, attr_1, and so on.

We often refer to __init__ as the constructor method, but that’s because we adopted jargon from other languages. The special method that actually constructs an instance is __new__: it’s a class method (but gets special treatment, so the @classmethod decorator is not used), and it must return an instance. That instance will in turn be passed as the first argument self of __init__.

suffices. The path just described, from __new__ to __init__, is the most common, but not the only one. The __new__ method can also return an instance of a different class, and when that happens, the interpreter does not call __init__. In other words, the process of building

The __new__ method gets the class as the first argument because, usually, the created object will be an instance of that class. So, in FrozenJSON.__new__, when the expression super().__new__(cls) effectively calls object.__new__(FrozenJSON), the instance built by the object class is actually an instance of FrozenJSON—i.e., the __class__ attribute of the new instance will hold a reference to FrozenJSON—even though the actual construction is performed by object.__new__, implemented in C, in the guts of the interpreter.

The funny name of the standard shelve module makes sense when you realize that pickle is the name of the Python object serialization format—and of the module that converts objects to/from that format. Because pickle jars are kept in shelves, it makes sense that shelve provides pickle storage. The shelve.open high-level function returns a shelve.Shelf instance—a simple key-value object database backed by the dbm module, with these  characteristics: 
shelve.Shelf subclasses abc.MutableMapping, so it provides the essential methods we expect of a mapping type

In addition, shelve.Shelf provides a few other I/O management methods, like sync and close; it’s also a context manager.

Keys and values are saved whenever a new value is assigned to a key.

The keys must be strings.

The values must be objects that the pickle module can handle.

The Record.__init__ method illustrates a popular Python hack. Recall that the __dict__ of an object is where its attributes are kept—unless __slots__ is declared in the class, as we saw in Saving Space with the __slots__ Class Attribute. So, updating an instance __dict__ with a mapping is a quick way to create a bunch of attributes in that instance.

Custom exceptions are usually marker classes, with no body. A docstring explaining the usage of the exception is better than a mere pass statement.
 

.
 In the venue property of Example 19-13, the last line returns self.__class__.fetch(key). Why not write that simply as self.fetch(key)? The simpler formula works  with the specific dataset of the OSCON feed because there is no event record with a 'fetch' key. If even a single event record had a key named 'fetch', then within that specific Event instance, the reference self.fetch would retrieve the value of that field, instead of the fetch class method that Event inherits from DbRecord. This is a subtle bug, and it could easily sneak through testing and blow up only in production when the venue or speaker records linked to that specific Event record are retrieved.

the Record class behaved more like a mapping, implementing a dynamic __getitem__ instead of a dynamic __getattr__, there would be no risk of bugs from overwriting or shadowing. A custom mapping is probably the Pythonic way to implement Record. But if I took that road, we’d not be reflecting on the tricks and traps of dynamic attribute programming. The

0 Now we have protected weight from users providing negative values. Although buyers usually can’t set the price of an item, a clerical error or a bug may create a LineItem with a negative price. To prevent that, we could also turn price into a property, but this would entail some repetition in our code. Remember the Paul Graham quote from Chapter 14: “When I see patterns in my programs, I consider it a sign of trouble.” The cure for repetition is abstraction. There are two ways to abstract away property definitions: using a property factory or a descriptor class. The descriptor class approach is more flexible, and we’ll devote Chapter 20 to a full discussion of it. Properties are in fact implemented as descriptor classes themselves. But here we will continue our exploration of properties by implementing a property factory as a function.

Although often used as a decorator, the property built-in is actually a class. In Python, functions and classes are often interchangeable, because both are callable and there is no new operator for object instantiation, so invoking a constructor is no different than invoking a factory function. And both can be used as decorators, as long as they return a new callable that is a suitable replacement of the decorated function. This is the full signature of the property constructor: property(fget=None, fset=None, fdel=None, doc=None) All arguments are optional, and if a function is not provided for one of them, the corresponding operation is not allowed by the resulting property object. The

The main point of this section is that an expression like obj.attr does not search for attr starting with obj. The search actually starts at obj.__class__, and only if there is no property named attr in the class, Python looks in the obj instance itself. This rule applies not only to properties but to a whole category of descriptors, the overriding descriptors. Further treatment of descriptors must wait for Chapter 20, where we’ll see that properties are in fact overriding descriptors. Now

When property is deployed as a decorator, the docstring of the getter method—the one with the @property decorator itself—is used as the documentation of the property as a whole. Figure 19-2 shows the 

.
 The bits of Example 19-24 that deserve careful study revolve around the storage_name variable. When you code each property in the traditional way, the name of the attribute where you will store a value is hardcoded in the getter and setter methods. But here, the qty_getter and qty_setter functions are generic, and they depend on the storage_name variable to know where to get/set the managed attribute in the instance __dict__. Each time the quantity factory is called to build a property, the storage_name must be set to a unique value. The functions qty_getter and qty_setter will be wrapped by the property object created in the last line of the factory function. Later when called to perform their duties, these functions will read the storage_name from their closures, to determine where to retrieve/store the managed attribute values. In

Attribute access using either dot notation or the built-in functions getattr, hasattr, and setattr trigger the appropriate special methods listed here. Reading and writing attributes directly in the instance __dict__ does not trigger these special methods—and that’s the usual way to bypass them if needed. “Section 3.3.9. Special method lookup” of the “Data model” chapter warns: For custom classes, implicit invocations of special methods are only guaranteed to work correctly if defined on an object’s type, not in the object’s instance dictionary. In other words, assume that the special methods will be retrieved on the class itself, even when the target of the action is an instance. For this reason, special methods are not shadowed by instance attributes with the same name.

In practice, because they are unconditionally called and affect practically every attribute access, the __getattribute__ and __setattr__ special methods are harder to use correctly than __getattr__—which only handles nonexisting attribute names. Using properties or descriptors is less error prone than defining these special methods. This

Python Cookbook, 3E by David Beazley and Brian K. Jones (O’Reilly) has several recipes covering the topics of this chapter, but I will highlight three that are outstanding: “Recipe 8.8. Extending a Property in a Subclass” addresses the thorny issue of overriding the methods inside a property inherited from a superclass; “Recipe 8.15. Delegating Attribute Access” implements a proxy class showcasing most special methods from Special Methods for Attribute Handling in this book; and the awesome “Recipe 9.21. Avoiding Repetitive Property Methods,” which was the basis for the property factory function presented in Example 19-24. Python in a Nutshell, 2E (O’Reilly), by Alex Martelli, 

Bertrand Meyer, quoted in the Uniform Access Principle definition in this chapter opening, wrote the excellent Object-Oriented Software Construction, 2E (Prentice-Hall). The book is more than 1,250 pages long, and I confess I did not read it all, but the first six chapters provide one of the best conceptual introductions to OO analysis and design I’ve seen, Chapter 11 introduces Design by Contract (Meyer invented the method and coined the term), and Chapter 35 offers his assessments of some key OO languages: Simula, Smalltalk, CLOS (the Lisp OO extension), Objective-C, C++, and Java, with brief comments on some others. Meyer is also the inventor of the pseudo-pseudocode: only in the last page of the book he reveals that the “notation” he uses throughout as pseudocode is in fact Eiffel.

Sometimes when designing APIs, I’ve wondered whether every method that does not take an argument (besides self), returns a value (other than None), and is a pure function (i.e., has no side effects) should be replaced by a read-only property. In this chapter, the LineItem.subtotal method (as in Example 19-23) would be a good candidate to become a read-only property. Of course, this excludes methods that are designed to change the object, such as my_list.clear(). 

versa. A descriptor is a class that implements a protocol consisting of the __get__, __set__, and __delete__ methods. The property class implements the full descriptor protocol. As usual with protocols, partial implementations are OK. In fact, most descriptors we see in real code implement only __get__ and __set__, and many implement only one of these methods. Descriptors are a distinguishing feature of Python, deployed not only at the application level but also in the language infrastructure. Besides properties, other Python features that leverage descriptors are methods and the classmethod and staticmethod decorators. Understanding descriptors is key to Python mastery. This is what this chapter is about. Descriptor

It may be tempting, but wrong, to store the value of each managed attribute in the descriptor instance itself. In other words, in the __set__ method, instead of coding:     instance.__dict__[self.storage_name] = value the tempting but bad alternative would be:     self.__dict__[self.storage_name] = value To understand why this would be wrong, think about the meaning of the first two arguments to __set__: self and instance. Here, self is the descriptor instance, which is actually a class attribute of the managed class. You may have thousands of LineItem instances in memory at one time, but you’ll only have two instances of the descriptors: LineItem.weight and LineItem.price. So anything you store in the descriptor instances themselves is actually part of a LineItem class attribute, and therefore is shared among all LineItem instances. A

To generate the storage_name, we start with a '_Quantity#' prefix and concatenate an integer: the current value of a Quantity.__counter class attribute that we’ll increment every time a new Quantity descriptor instance is attached to a class. Using the hash character in the prefix guarantees the storage_name will not clash with attributes created by the user using dot notation, because nutmeg._Quantity#0 is not valid Python syntax. But we can always get and set attributes with such “invalid” identifiers using the getattr and setattr built-in functions, or by poking the instance __dict__. Example

Django users will notice that Example 20-4 looks a lot like a model definition. It’s no coincidence: Django model fields are descriptors.

As implemented so far, the Quantity descriptor works pretty well. Its only real drawback is the use of generated storage names like _Quantity#0, making debugging hard for the users. But automatically assigning storage names that resemble the managed attribute names requires a class decorator or a metaclass, topics we’ll defer to Chapter 

 descriptor that implements the __set__ method is called an overriding descriptor, because although it is a class attribute, a descriptor implementing __set__ will override attempts to assign to instance attributes. This is how Example 20-2 was implemented. Properties are also overriding descriptors: if you don’t provide a setter function, the default __set__ from the property class will raise AttributeError to signal that the attribute is read-only. Given the code in Example 20-8, experiments

Descriptor If a descriptor does not implement __set__, then it’s a nonoverriding descriptor. Setting an instance attribute with the same name will shadow the descriptor, rendering it ineffective for handling that attribute in that specific instance. Methods are implemented as nonoverriding descriptors. Example 20-11 shows the operation of a nonoverriding descriptor. Example

Warning Python contributors and authors use different terms when discussing these concepts. Overriding descriptors are also called data descriptors or enforced descriptors. Nonoverriding descriptors are also known as nondata descriptors or shadowable descriptors. In

Descriptors A function within a class becomes a bound method because all user-defined functions have a __get__ method, therefore they operate as descriptors when attached to a class. Example 20-13 demonstrates

Because functions do not implement __set__, they are nonoverriding descriptors, as the last line of Example 20-13 shows. The other key takeaway from Example 20-13 is that obj.spam and Managed.spam retrieve different objects. As usual with descriptors, the __get__ of a function returns a reference to itself when the access happens through the managed class. But when the access goes through an instance, the __get__ of the function returns a bound method object: a callable that wraps the function and binds the managed instance (e.g., obj) to the first argument of the function (i.e., self), like the functools.partial function does (as seen in Freezing Arguments with functools.partial). For

.
 The bound method object also has a __call__ method, which handles the actual invocation. This method calls the original function referenced in __func__, passing the __self__ attribute of the method as the first argument. That’s how the implicit binding of the conventional self argument works. The way functions are turned into bound methods is a prime example of how descriptors are used as infrastructure in the language. After

Caching can be done efficiently with __get__ only
 
If you code just the __get__ method, you have a nonoverriding descriptor. These are useful to make some expensive computation and then cache the result by setting an attribute by the same name on the instance. The namesake instance attribute will shadow the descriptor, so subsequent access to that attribute will fetch it directly from the instance __dict__ and not trigger the descriptor __get__ anymore.
 

[Metaclasses] are deeper magic than 99% of users should ever worry about. If you wonder whether you need them, you don’t (the people who actually need them know with certainty that they need them, and don’t need an explanation about why).[194] —
Tim Peters
Inventor of the timsort algorithm and prolific Python contributor
 Class

This is an exciting topic, and it’s easy to get carried away. So I must start this chapter with the following admonition: If you are not authoring a framework, you should not be writing metaclasses—unless you’re doing it for fun or to practice the concepts. We

Metaclasses are powerful, but hard to get right. Class decorators solve many of the same problems more simply. In fact, metaclasses are now so hard to justify in real code that my favorite motivating example lost much of its appeal with the introduction of class decorators in Python 2.6. Also

We usually think of type as a function, because we use it like one, e.g., type(my_object) to get the class of the object—same as my_object.__class__. However, type is a class. It behaves like a class that creates a new class when invoked with three arguments: MyClass = type('MyClass', (MySuperClass, MyMixin),
               {'x': 42, 'x2': lambda self: self.x * 2}) The three arguments of type are named name, bases, and dict—the latter being a mapping of attribute names and attributes for the new class.

’s good practice to avoid exec or eval for metaprogramming in Python. These functions pose serious security risks if they are fed strings (even fragments) from untrusted sources. Python offers sufficient introspection tools to make exec and eval unnecessary most of the time. However, the Python core developers chose to use exec when implementing namedtuple. The chosen approach makes the code generated for the class available in the ._source attribute.

Invoking type with three arguments is a common way of creating a class dynamically. If you peek at the source code for collections.namedtuple, you’ll see a different approach: there is _class_template, a source code template as a string, and the namedtuple function fills its blanks calling _class_template.format(…). The resulting source code string is then evaluated with the exec built-in function. Warning

Instances of classes created by record_factory have a limitation: they are not serializable—that is, they can’t be used with the dump/load functions from the pickle module. Solving this problem is beyond the scope of this example, which aims to show the type class in action in a simple use case. For the full solution, study the source code for collections.nameduple; search for the word “

way. A class decorator is very similar to a function decorator: it’s a function that gets a class object and returns the same class or a modified one. In Example

Class decorators are a simpler way of doing something that previously required a metaclass: customizing a class the moment it’s created.

significant drawback of class decorators is that they act only on the class where they are directly applied. This means subclasses of the decorated class may or may not inherit the changes made by the decorator, depending on what those changes are. 

For successful metaprogramming, you must be aware of when the Python interpreter evaluates each block of code. Python programmers talk about “import time” versus “runtime” but the terms are not strictly defined and there is a gray area between them. At import time, the interpreter parses the source code of a .py module in one pass from top to bottom, and generates the bytecode to be executed. That’s when syntax errors may occur. If there is an up-to-date .pyc file available in the local __pycache__, those steps are skipped because the bytecode is ready to run. Although compiling is definitely an import-time activity, other things may happen at that time, because almost every statement in Python is executable in the sense that they potentially run user code and change the state of the user program. In particular, the import statement is not merely a declaration[196] but it actually runs all the top-level code of the imported module when it’s imported for the first time in the process—further imports of the same module will use a cache, and only name binding occurs then. That top-level code may do anything, including actions typical of “runtime”, such as connecting to a database.[197] That’s why the border between “import time” and “runtime” is fuzzy: the import statement can trigger all sorts of “runtime” behavior. In

 makes sense that the interpreter evaluates the body of a decorated class before it invokes the decorator function that is attached on top of it: the decorator must get a class object to process, so the class object must be built first.

Consider the Python object model: classes are objects, therefore each class must be an instance of some other class. By default, Python classes are instances of type. In other words, type is the metaclass for most built-in and user-defined classes: >>> 'spam'.__class__
<class 'str'>
>>> str.__class__
<class 'type'>
>>> from bulkfood_v6 import LineItem
>>> LineItem.__class__
<class 'type'>
>>> type.__class__
<class 'type'> To avoid infinite regress, type is an instance of itself, as the last line shows. Note that I am not saying that str or LineItem inherit from type. What I am saying is that str and LineItem are instances of type. They all are subclasses of object.

The classes object and type have a unique relationship: object is an instance of type, and type is a subclass of object. This relationship is “magic”: it cannot be expressed in Python because either class would have to exist before the other could be defined. The fact that type is an instance of itself is also magical. Besides

Ultimately, the class of ABCMeta is also type. Every class is an instance of type, directly or indirectly, but only metaclasses are also subclasses of type. That’s the most important relationship to understand metaclasses: a metaclass, such as ABCMeta, inherits from type the power to construct classes.

When coding a metaclass, it’s conventional to replace self with cls. For example, in the __init__ method of the metaclass, using cls as the name of the first argument makes it clear that the instance under construction is a class. The

As we’ve seen, both the type constructor and the __new__ and __init__ methods of metaclasses receive the body of the class evaluated as a mapping of names to attributes. However, by default, that mapping is a dict, which means the order of the attributes as they appear in the class body is lost by the time our metaclass or class decorator can look at them. The solution to this problem is the __prepare__ special method, introduced in Python 3. This special method is relevant only in metaclasses, and it must be a class method (i.e., defined with the @classmethod decorator). The __prepare__ method is invoked by the interpreter before the __new__ method in the metaclass to create the mapping that will be filled with the attributes from the class body. Besides the metaclass as first argument, __prepare__ gets the name of the class to be constructed and its tuple of base classes, and it must return a mapping, which will be received as the last argument by __new__ and then __init__ when the metaclass builds a new class.

classes. Metaclasses are challenging, exciting, and—sometimes—abused by programmers trying to be too clever. To wrap up, let’s recall Alex Martelli’s final advice from his essay Waterfowl and ABCs: And, don’t define custom ABCs (or metaclasses) in production code… if you feel the urge to do so, I’d bet it’s likely to be a case of “all problems look like a nail”-syndrome for somebody who just got a shiny new hammer—you (and future maintainers of your code) will be much happier sticking with straightforward and simple code, eschewing such depths. —

all. For metaclasses, the main references are PEP 3115 — Metaclasses in Python 3000, in which the __prepare__ special method was introduced and Unifying types and classes in Python 2.2, authored by Guido van Rossum. The text applies to Python 3 as well, and it covers what were then called the “new-style” class semantics, including descriptors and metaclasses. It’s a must-read. One of the references cited by Guido is Putting Metaclasses to Work: a New Dimension in Object-Oriented Programming, by Ira R. Forman and Scott H. Danforth (Addison-Wesley, 1998), a book to

For Python 3.5—in alpha as I write this—PEP 487 - Simpler customization of class creation puts forward a new special method, __init_subclass__ that will allow a regular class (i.e., not a metaclass) to customize the initialization of its subclasses. As with class decorators, __init_subclass__ will make class metaprogramming more accessible and also make it that much harder to justify the deployment of the nuclear option—metaclasses. If you are into metaprogramming, you may wish Python had the ultimate metaprogramming feature: syntactic macros, as offered by Elixir and the Lisp family of languages. Be careful what you wish for. I’ll just say one word: MacroPy. Soapbox

Of course, Python is not perfect. Among the top irritants to me is the inconsistent use of CamelCase, snake_case and joinedwords in the standard library. But the language definition and the standard library are only part of an ecosystem. The community of users and contributors is the best part of the Python ecosystem. Here

Brandon Rhodes is an awesome Python teacher, and his talk “A Python Æsthetic: Beauty and Why I Python” is beautiful, starting with the use of Unicode U+00C6 (LATIN CAPITAL LETTER AE) in the title. Another awesome teacher, Raymond Hettinger, spoke of beauty in Python at PyCon US 2013: “Transforming Code into Beautiful, Idiomatic Python”. The

