Lesson 1 - Lambda Expressions
=============================

Lambda Expression
-----------------

-   Anonymous function which is not a method

-   Body of lambda may throw exceptions

-   Has type but no object

Functional Interface
--------------------

-   An interface

-   Has only one abstract method

    -   implemented methods don’t count (e.g. default method, static method)

-   Default method allows multiple inheritance

-   Now JDK 8 allows static methods in interfaces

-   @FunctionalInterface annotation

    -   compiler can check whether interface has only one abstract method

java.util.function Package
--------------------------

-   Well defined set of general purpose functional interfaces

-   All have only one abstract method

-   **Lambda expressions can be used wherever these types are referenced**

-   Used extensively in Java class libraries (especially with the Stream API)

-   Set

    -   Consumer&lt;T&gt;: takes a single argument and returns no result

        -   e.g. String s -&gt; System.out.println(s)

        -   BiConsumer&lt;T,U&gt; takes two arguments and returns no result

    -   Supplier: The opposite of a Consumer (takes no arguments and returns a result

    -   Function&lt;T,R&gt;: takes one argument and returns a result

        -   e.g. String s -&gt; s.getName()

        -   BiFunction&lt;T,U,R&gt; takes two arguments and returns a result

    -   UnaryOperator&lt;T&gt;

        -   Specialized form of Function

        -   Single argument and result of the same type

        -   e.g. String s -&gt; s.toLowerCase()

    -   BinaryOperator&lt;T&gt;

        -   Specialized form of BiFunction

        -   Two arguments and a result, all of the same type

    -   Predicate

        -   One argument, boolean result

        -   e.g. String s -&gt; s.graduationYear() == 2011

        -   BiPredicate takes two arguments and returns a boolean result

Method and Constructor References
---------------------------------

-   **Shorthand forms of defining certain types of lambda expressions**

-   Method References

    -   This lets us reuse a method as a lambda expression

    -   Format: target\_reference::method\_name

    -   e.g. FileFilter x = File f -&gt; f.canRead(); =&gt; FileFilter x = File::canRead;

    -   Three types

        -   static method

            -   (args) -&gt; ClassName.staticMethod(args)

            -   ClassName::staticMethod

        -   Instance method of an arbitrary type

            -   the one not you’re not referring to in terms of variable that you have in your code

            -   (arg0, reset) -&gt; arg0.instanceMethod(rest)

            -   ClassName::instanceMethod

        -   Instance method of an existing object

            -   in your code

            -   (args) -&gt; expr.instanceMethod(args)

            -   reference::instanceMethod (e.g. this::getLength)

-   Constructor References

    -   Same concept as a method reference but for the contructor

    -   Factory&lt;List&lt;String&gt;&gt; f = () -&gt; return new ArrayList&lt;String&gt;();

    -   Factory&lt;List&lt;String&gt;&gt; f = ArrayList&lt;String&gt;::new;

Referencing External Variables in Lambda Expressions
----------------------------------------------------

-   can refer to effectively final local variables

    -   Effectively final: A variable that meets the requirements for final variables (i.e. assigned once), event if not explicitly declared final

    -   Closures on values, not variables

-   What does ‘this’ mean in a lambda

    -   Think of ‘this’ as a final predefined local

    -   Lambda is not associated with a class so there can be no ‘this’

    -   Use this carefully since it can be problematic in multi thread environment

Useful New Methods in JDK 8 That Can Use Lambdas
------------------------------------------------

-   Iterable Interface

    -   Iterable.forEach(Consumer c)

        -   myList.forEach(s -&gt; System.out.println(s));

        -   myList.forEach(System.out::println);

-   Collection Interface

    -   Collection.removeIf(Predicate p)

        -   myList.removeIf(s -&gt; s.length() == 0);

-   List Interface

    -   List.replaceAll(UnaryOperator o)

        -   myList.replaceAll(s -&gt; s.toUpperCase());

        -   myList.replaceAll(String::toUpperCase);

    -   List.sort(Comparator c)

        -   Replaces Collections.sort(List l, Comparator c)

        -   myList.sort((x,y) -&gt; x.length() - y.length());

-   Logger Class

    -   Common problem

        -   logger.finest(createComplexMessage());

        -   createComplexMessage() is always called, even when not required.

    -   Takes a Supplier as an argument

    -   logger.finest(() -&gt; createComplexMessage());

        -   pass this lambda to finest and it will use it only if necessary

        -   **passing behavior rather than value**

-   **Lambda provides behavior, not a value**

Summary
-------

-   Lambda expressions provide a simple way to pass behavior as a parameter, or assign to a value

-   They can be used wherever a functional interface type is used

    -   The Lambda provides the implementation of the single abstract method

-   Method and constructor references can be used as shorthand

-   Several useful new methods in JDK 8 that can use Lambdas

Lesson 2 - Introduction To The Streams API
==========================================

Introduction to Functional Programming Concepts
-----------------------------------------------

Imperative Programming

-   Use variables as an association between names and values

-   Use sequences of commands

Functional Programming

-   Based on structured function calls

-   Function call which calls other functions in turn

-   Each function receives values from, and passes values back the calling function

-   Names are only used as formal parameters

    -   Once value is assigned i cannot be changed

-   No concept of a command, as used in imperative code

    -   No concept of assigning value since it assigns only once

    -   Therefore no concept of repetition

Names And Values

-   Imperative: The same name may be associated with different values

-   Functional: A name is only ever associated with one value

Execution Order

-   Imperative

    -   Values associated with names can be changed

    -   The order of execution of commands forms a contract

        -   If it is changed, the behavior of the application may change

-   Functional

    -   Values associated with names cannot be changed

    -   The order of execution does not impact the results

    -   There is no fixed execution order

Repetition

-   Imperative

    -   Values associated with names may be changed by commands

    -   Commands may be repeated leading to repeated changes

    -   New values may be associated with the same name through repetition (loops)

-   Functional

    -   Values associated with names may not be changed

    -   Repeated changes are achieved by nested function calls

    -   New values may be associated with the same name through recursion

Functions as Values

-   Functional programming allows functions to be treated as values

-   This is why lambda expressions were required in JDK

-   To make this much simpler than anonymous inner classes

Elements of Stream
------------------

Stream Overview

-   At a high level

    -   Abstraction for specifying aggregate computation

        -   Not a data structure

            -   Streams is just a way of defining how we process that as sequence elements

        -   Can be infinite

    -   Simplifies the description of aggregate computations

        -   Exposes opportunities for optimisation

        -   Fusing, laziness and parallelism

-   Pipeline

    -   A stream pipeline consists of three types of things

        -   A source

            -   Set of elements of something that we want to process

        -   Zero or more intermediate operations

            -   Takes as input stream and generates as output stream

        -   A terminal operation

            -   Takes as input stream and produces a result or a side-effect

Stream Terminal Operations

-   The pipeline is only evaluated when the terminal operation is called

    -   All operations can execute sequentially or in parallel

    -   Intermediate operations can be merged

        -   Avoiding multiple redundant passes on data

        -   Short-circuit operations (e.g. findFirst)

        -   Lazy evaluation

    -   Stream characteristics help identify optimisations

        -   DISTINCT streams passed to distinct() is a no-op

Summary

-   Think of a stream as like a pipeline

-   Processes data from the source

    -   No explicit loop used

    -   Which means a Stream can easily be made parallel

Streams of Objects and Primitive Types
--------------------------------------

Objects And Primitives

-   The Java language is not truly object oriented (in order to improve performance)

-   Primitive types are included (e.g. byte, short, int, etc.)

-   For some situations these are wrapped as objects

    -   e.g. storage in collections

    -   Byte, Short, Integer, etc

-   Conversion between primitive and object representation is often handled by auto-boxing and unboxing

Object Streams

-   By default, a stream produces elements that are objects

-   Sometimes, this is not the best solution

Primitive Streams

-   To avoid a lot of unnecessary object creation and work we have three primitive stream types

    -   IntStream, DoubleStream, LongStream

-   These can be used with certain stream operations

-   e.g. mapToInt

Summary

-   Java has primitive values as well as object types

-   To improve steram efficiency we have three primitive stream types

    -   IntStream, DoubleStream, LongStream

-   Use methods like mapToInt(), mapToDouble(), mapToLong()

-   For reverse, mapToObj()

Stream Sources in JDK 8
-----------------------

JDK 8 Librearies

-   There are 95 methods in 23 classes that return a Stream

    -   Many of them, though are intermediate operations in the Stream interface

-   71 methods in 15 classes can be used as practical Stream sources

Collection Interface

-   stream()

    -   Provides a sequential stream of elements in the collection

-   parallelStream()

    -   Provides a parallel stream of elements in the collection

    -   Uses the fork-join framework for implementation

Arrays Class

-   stream()

    -   An array is a collection of data, so logical to be able to create Stream

    -   Provides a sequential stream

    -   overloaded methods for different types

        -   double, int, long, Object

Files Class

-   find(Path, BiPredicate, FileVisitOption)

    -   A stream of File references that match a given BiPredicate

-   list(Path)

    -   A stream of entries from a given directory

-   lines(Path)

    -   A stream of strings that are the lines read from a given file

-   walk(Path, FileVisitOption)

    -   A stream of File references walking from a given path

Random Numbers

-   Three random related classes

    -   Random, ThreadLocalRandom, SplittableRandom

-   Methods to produce finite or infinite sterams of random numbers

    -   ints(), doubles(), longs()

    -   Four versions of each

        -   Finite or infinite

        -   With and without seed

Miscellaneous Classes And Methods

-   JarFile/ZipFile: steram()

    -   Returns a File stream of the contents of the compress archive

-   BufferedReader: lines()

    -   Returns a stream of strings that are the lines read from the input

-   Pattern: splitAsStream()

    -   Returns a stream of strings of matches of a pattern

    -   like split(0, but returns a stream rather than an array

-   CharSequence

    -   chars(): Char values as ints for the sequence

    -   codePoints(): Code point values for this sequence

-   BitSet

    -   stream(): Indices of bits that are set

Stream Static Methods

-   These interfaces are primitive specializations of the Stream interface

-   concat(Stream, Stream), empty()

    -   Concatenates two specified streams, returns an empty stream

-   of(T… values)

    -   A stream that consists of the specified values

-   range(int, int), rangeClosed(int, int)

    -   A stream from a start to an end value (exclusive or inclusive)

-   generate(IntSupplier), iterate(int, IntUnaryOperator)

    -   An infinite stream created by a given Supplier

    -   iterate() uses a seed to start the stream

Summary

-   Numerous places to get stream sources

    -   Useful methods for retrieving lines from files, files from archives, etc

    -   Only Collection can provide a parallel stream directly

Intermediate Operations
-----------------------

Stream Interface

-   A stream provides a sequence of elements

    -   Supporting either sequential or parallel aggregate operations

-   Most operations take a parameter that describe its behavior

    -   Typically using a Lambda expression

    -   Must be non-interfering (does not modify the stream)

    -   Typically stateless

-   Streams may be changed from sequential to parallel (and vice-versa)

    -   All processing is done either sequentially or in parallel

    -   Last call wins

Filtering And Mapping

-   distinct()

    -   Returns a stream with no duplicate elements

-   filter(Predicate p)

    -   Returns a stream with only those elements that return true for the Predicate

-   map(Function f)

    -   Return a stream where the given Function is applied to each element on the input stream

-   mapToInt(), mapToDouble(), mapToLong()

    -   Like map(), but producing streams of primitives rather than objects

Maps and FlatMaps

-   Map: one-to-one mapping

-   FlatMap: 1-to-many mapping. Stream of streams. Takes input stream and apply 1-to-many mapping then concatenate results together.

Restricting The Size of A Stream

-   skip(long n)

    -   Returns a stream that skips the first n elements of the input stream

-   limit(long n)

    -   Returns a stream that only contains the first n elements of the input stream

Sorting and Unsorting

-   sorted(Comparator c)

    -   Returns a stream that is sorted with the order determined by the Comparator

    -   sorted() with no arguments sorts by natural order

-   unordered()

    -   Inherited from BaseStream

    -   Returns a stream that is unordered (used internally)

    -   Can improve efficiency of operations like distinct() and groupingBy()

Observing Stream Elements

-   peek(Consumer c)

    -   Returns an output stream that is identical to the input stream

    -   Each element is passed to the accept() method of the Consumer

    -   The Consumer must not modify the elements of the stream

    -   Useful for debugging and doing more than one thing with a stream

Summary

-   Stream interface represents aggregate operations on elements

-   Most methods can use Lambda expressions to define behavior

-   Powerful range of intermediate operations allow streams to be manipulated as required

    -   Build up complex processing from simple building blocks

Terminal Operations
-------------------

Terminal Operations

-   Terminates the pipeline of operations on the stream

-   Only at this point is any processing performed

    -   This allows for optimization of the pipeline

        -   Lazy evaluation

        -   Merged/fused operations

        -   Elimination of redundant operations

        -   Parallel execution

-   Generates an explicit result or a side effect

Matching Elements

-   findFirst(Predicate p)

    -   The first element that matches using given Predicate

-   findAny(Predicate p)

    -   Works the same as findFirst(), but for a parallel stream

    -   Gives result faster than findFirst() because it’s non-deterministic

-   boolean allMatch(Predicate p)

    -   Whether all the elements of the stream match using the Predicate

-   boolean anyMatch(Predicate p)

    -   Whether any of the elements of the stream match using the Predicate

-   boolean nonMatch(Predicate p)

    -   Whether no elements match using the Predicate

Collecting Results

-   collect(Collector c)

    -   Performs a mutable reduction on the stream

    -   Takes elements on the stream and puts them into whatever Collector defines as the place to put them

-   toArray()

    -   Returns an array containing the elements of the stream

Numerical Results

-   Object Stream

    -   count()

        -   Returns how many elements are in the stream

    -   max(Comparator c)

        -   The maximum value element of the stream using the Comparator

        -   Returns an Optional, since the stream may be empty

    -   min(Comparator c)

-   Primitive Type Streams (IntStream, DoubleStream, LongStream)

    -   average()

        -   Return the arithmetic mean of the stream

        -   Returns an Optional, as the stream may be empty

    -   sum()

        -   Returns the sum of the stream elements

Iteration

-   forEach(Consumer c)

    -   Performs an action for each element of this stream

-   forEachOrdered(Consumer c)

    -   Like forEach, but ensures that the order of the elements (if one exists) is respected when used for a parallel stream

-   Use with caution!

    -   Encourages non-functional (imperative) programming style

    -   More detail in Lesson 3

Folding A Stream

-   Creating a single result from multiple input elements

    -   reduce(BinaryOperator accumulator)

        -   Performs a reduction on the stream using the BinaryOperator

        -   The accumulator takes a partial result and the next element, and returns a new partial result

        -   Returns an Optional

        -   Two other versions

            -   One that takes an initial value (does not return an Optional)

            -   One that takes an initial value and BiFunction (equivalent to a fused map and reduce)

Summary

-   Terminal operations provide results or side effects

-   Many types of operation available

-   Ones like reduce and collect need to be looked at in more detail

The Optional Class
------------------

The problems of null

-   Certain situations in Java return a result which is a null

    -   Reference to an object that is not initialized

Helping to eliminate the NullPointerException

-   Terminal operations like min(), max(), may not return a direct result

    -   Supposed the input stream is empty?

-   Optional&lt;T&gt;

    -   Container for an object reference (null, or real object)

    -   Think of it like a stream of 0 or 1 elements

    -   Guaranteed that the Optional reference returned will not be null

Optional ifPresent()

-   Do something when set

-   opt.ifPresent(x -&gt; print(x));

-   opt.ifPresent(this::print);

Optinal filter()

-   Reject certain values of the Optional

-   opt.filter(x -&gt; x.contains(“a”)).ifPresent(this::print);

Optional map()

-   Transform value if present

-   opt.map(String::trim).filter(t -&gt; t.length() &gt; 0).ifPresent(this::print);

Optional flatMap()

-   Going deeper

-   Optional&lt;String&gt; similar = opt.flatMap(this::tryFindSimilar);

Summary

-   Optional class eliminates problems of NullPointerException

-   Can be used in powerful ways to provide complex conditional handling

Summary
-------

-   Streams provides a straightforward way for functional style programming in Java

-   Streams can either be objects or primitive types

-   A stream consists of a source, possible intermediate operations and a terminal operation

    -   Certain terminal operations return an Optional to avoid possible NullPointerException problems

Lesson 3 - Advanced Lambda and Stream Concepts
==============================================

Understanding and Using Reductions
----------------------------------

Find the right accumulator

-   Again, recall the accumulator takes a partial result and the next element, and returns a new partial result

-   In essence it does the same as our recursive solution

-   Without all the stack frames

-   (x, y) -&gt; { } : x in effect maintains state for us, by always holding the longest string found so far

Summary

-   Reduction takes a stream and reduces it to a single value

-   The way the reduction works is defined by the accumulator

    -   Which is a BinaryOperator

    -   The accumulator is applied successively to the stream elements

    -   The reduce() method maintains a partial result state

    -   Like a recursive approach, but without the resource overhead

-   Requires you to think differently to an imperative, loop based approach

Finite and Infinite Streams
---------------------------

Dealing With The Indeterminate

-   Imperative Java

    -   How to continue processing when we can’t predict for how long?

        -   while(true) & break

-   Making The Stream Finite

    -   Terminate the stream when a condition is met

        -   findFirst(Predicate p)

            -   Works with both sequential and parallel stream

            -   Gives always same result

        -   findAny(Predicate p)

            -   Works with parallel stream

            -   Gives you non-deterministic result

        -   int r = Random.ints().findFirst(i -&gt; i &gt; 256);

            -   Infinite stream of random integers

            -   stream terminates when a number greater than 256 is encountered

-   Keeping It Infinite

    -   Sometimes we need to continue to use a stream indefinitely

    -   What terminal operation should we use for this?

        -   Use forEach()

        -   This consumes the element from the stream

        -   But does not terminate it

-   Infinite Example

    -   Reading temperature from a serial sensor

        -   Converting from fahrenheit to celsius, removing F

        -   Notifying a listener of changes if registered

Summary

-   Streams can be infinite as well as finite

-   There is no concept of ‘breaking’ out of a stream

-   Use the appropriate terminal operation to stop processing

-   Or use the infinite stream indefinitely

Avoiding The Use of forEach
---------------------------

Using Streams Effectively

-   Stop thinking imperatively

    -   Imperative programming uses loops for repetitive behavior

    -   it also uses variables to hold state

    -   We can continue to do that in some ways with streams

    -   THIS IS WRONG

-   Legitimate Use of for Each

    -   No state being modified

    -   Simplified iteration

    -   May be made parallel if order is not important

    -   transactions.stream().forEach(t -&gt; t.print());

Summary

-   If you are thinking of using forEach(), stop

-   Can it be replaced with a combination of mapping and reduction?

-   if so, it is unlikely to be the right approach to be functional

-   Certain situations are valid for using forEach()

    -   e.g. printing values from the stream

Using Collectors
----------------

Collector Basics

-   A Collector performs a mutable reduction on a stream.

    -   Accumulates input elements into a mutable result container

    -   Results container can be a List, Map, String, etc

-   Use the collect() method to terminate the stream

-   Collectors utility class has many methods that can create a Collector

Composing Collectors

-   Several Collectors methods have versions with a downstream collector

-   Allows a second collector to be used

    -   collectingAndThen()

    -   groupingBy()/groupingByConcurrent()

    -   mapping()

    -   partitioningBy()

Collecting Into A Collection

-   toCollection(Supplier factory)

    -   Adds the elements of the stream to a Collection (created using factory)

    -   Uses encounter order

-   toList()

    -   Adds the elements of the stream to a List

-   toSet()

    -   Adds the elements of the stream to a Set

    -   Eliminates duplicates

Collecting To A Map

-   toMap(Function keyMapper, Function valueMapper)

    -   Create a Map from the elements of the stream

    -   key and value produced using provided functions

    -   Use Function.identity() to get the stream element

    -   Map&lt;Student, Double&gt; studentToScore = students.stream().collect(toMap(Functions.identity(), student -&gt; getScore(student)));

-   toMap(Function keyMapper, Function valueMapper, BinaryOperator merge)

    -   Handling Duplicate Keys

    -   Map&lt;String, String&gt; occupants = people.stream().collect(toMap(Person::getAddress, Person::getName, (x, y) -&gt; x + “,” + y));

Grouping Results

-   groupingBy(Function)

    -   Groups stream elements using the Function into a Map

    -   Result is Map&lt;K, List&lt;V&gt;&gt;

    -   Map m = words.stream().collect(Collectors.groupingBy(String::length));

-   groupingBy(Function, Collector)

    -   Groups stream elements using the Function

    -   A reduction is performed on each group using the downstream Collector

    -   Map m = words.stream().collect(Collectors.groupingBy(String::length, counting()));

Joining String Results

-   joining()

    -   Collector concatenates input strings

-   joining(delimiter)

    -   Collector concatenates stream strings using CharSequence delimiter

    -   collect(Collectors.joining(“,”)); // Create CSV

-   joining(delimiter, prefix, suffix)

    -   Collector concatenates the prefix, stream strings separated by delimiter and suffix

Numeric Collectors

-   Also available in Double and Long forms

-   averagingInt(ToIntFunction)

    -   Averages the results generated by the supplied function

-   summarizingInt(ToIntFunction)

    -   summarises (count, sum, min, max, average) results generated by supplied function

-   summingInt(ToIntFunction)

    -   equivalent to a map() then sum()

-   maxBy(Comparator), minBy(Comparator)

    -   Max or min value based on Comparator

Other Collectors

-   reducing(BinaryOperator)

    -   Equivalent Collector to reduce() terminal operation

    -   Only use for multi-level reductions, or downstream collectors

-   partitioningBy(Predicate)

    -   Creates a Map&lt;Boolean, List&gt; containing two groups based on Predicate

-   mapping(Function, Collector)

    -   Adapts a Collector to accept different type elements mapped by the Function

    -   Map&lt;City, Set&lt;String&gt;&gt; lastNamesByCity = people.stream().collect(groupingBy(Person::getCity, mapping(Person::getLastName, toSet())));

Summary

-   Collectors provide powerful ways to gather elements of an input stream

    -   Into collections

    -   In numerical ways like totals and averages

-   Collectors can be composed to build more complex ones

-   You can also create your own Collector

Parallel Streams (And When Not to Use Them)
-------------------------------------------

Serial And Parallel Streams

-   Collection stream sources

    -   stream()

    -   parallelStream(0

-   Stream can be made parallel or sequential at any point

    -   parallel()

    -   sequential()

-   The last call wins

    -   Whole stream is either sequential or parallel

-   Calling concat() with a sequential and parallel stream will produce a parallel stream

Parallel Streams

-   Implemented internally using the fork-join framework

-   Will default to as many threads for the pool as the OS reports processors

    -   Which may not be what you want

    -   OS가 보고한 쓰레드 숫자보다 더 적게 쓰고 싶을 때도 있음. workaround는 다음과 같다.

    -   System.setProperty(“java.util.concurrent.ForkJoinPool.common.parallelism”, “32767”);

        -   0 \~ 32767

-   Remember, parallel streams always need more work to process

    -   But they might finish it more quickly

Parallel Stream Considerations

-   findFirst() and findAny()

    -   findAny() is non-deterministic, so better for parallel stream performance

    -   Use findFirst() if a deterministic result is required

-   forEach() and forEachOrdered()

    -   forEach() is non-deterministic for a parallel stream and ordered data

    -   Use forEachOrdered() if a deterministic result is required

When To Use Parallel Streams

-   No simple answer

-   Data set size is important, as is the type of data structure

    -   ArrayList: Good

    -   HashSet, TreeSet: OK

    -   LinkedList: BAD

-   Operations are also important

    -   Certain operations decompose to parallel tasks better than others

    -   filter() and map() are excellent

        -   When you’re doing a mapping, you’re mapping each individual element; there’s no reliance between one element in the stream and another.

    -   sorted() and distinct() do not decompose well

        -   There’s a reliance on other elements in the input set in order to construct the result

-   Quantitative Considerations

    -   N = size of the data set

    -   Q = Cost per element through the Stream pipeline

    -   N x Q = Total cost of pipeline operations

    -   The bigger N x Q is the better a parallel stream will perform

    -   It is easier to know N than Q, but Q can be estimated

    -   If in doubt, profile

Summary

-   Streams can be processed sequentially or in parallel

    -   The whole stream is processed in sequentially or in parallel

    -   In most cases how the stream is defined will not affect the result

    -   findFirst(), findAny(), forEach(), forEachOrdered() do

-   Don’t assume that a parallel stream will return a result faster

    -   Many factors affect performance.

Debugging Lambdas and Streams
-----------------------------

Problems With Debugging Streams

-   Streams provide a high level abstraction

    -   This is good for making code clear and easy to understand

    -   This is bad for debugging

        -   A lot happens internally in the library code

        -   Setting breakpoints is not simple

        -   Stream operations are merged to improve efficiency

Simple Debugging - Finding what is happening between methods

-   Use peek()

    -   Like the use of print statements

    -   debug하고 싶은 stream operation 다음에 둬서 잘 나오는지 프린트해봄

Setting A Breakpoint

-   Add a peek() method call between stream operations

-   Use a Consumer that does nothing if required

    -   peek(s -&gt; s)

    -   Some debugging tools don’t like empty bodies

-   Using a method reference

    -   Lambda expressions do not compile to equivalent inner class

        -   Compiled to invokedynamic call

        -   Implementation decided at runtime

        -   Better chance of optimization, makes debugging harder

    -   Solution

        -   Extract the code from a lambda expression into a separate method

        -   Replace the Lambda with a method reference for the new method

        -   Set breakpoints on the statements in the new method

        -   Examine program state using debugger

Summary

-   Debugging is harder with Lambdas and streams

    -   Stream methods get merged

    -   Lambdas are converted to invokedynamic bytecodes and implementation is decided at runtime

    -   Harder to set breakpoints

-   peek() and method references can simplify things

Course Conclusions
------------------

Lambda Expressions

-   Lambda expressions give us a simple way to define behavior

    -   Can be assigned to a variable or passed as a parameter

-   Can be used wherever the type is a function interface

    -   One that has only one abstract method

    -   The lambda expression provides an implementation of the abstract method

Stream API

-   Pipeline of operations to process collections of data

    -   Multiple sources, not just from the Collections API

    -   Can be processed sequentially or in parallel

-   Sources, intermediate and terminal operations

-   Behavior of intermediate and terminal operations often defined using Lambda expressions

-   Terminal operations often return an Optional

-   We can now use a functional style of programming Java

Lambdas And Streams: Think Differently

-   Need to think functional rather than imperative

    -   Try to stop thinking in loops and using mutable state

-   Think of how to approach problems using recursion

    -   Rather than an explicit loop

    -   Avoid forEach (except for special cases)

-   Infinite streams don’t need to be infinite

    -   e.g. findFirst(), findAny()

-   Remember, parallel streams always invoke more work

    -   Sometimes they complete the work quicker
