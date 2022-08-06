---
title: Scala
updated: 2021-12-26 15:57:50Z
created: 2021-12-23 20:32:20Z
source: https://gist.github.com/heathermiller/2ab9ef36910fdfdd20e9
tags:
  - multi-paradigme
  - s1
---

# Evaluation Rules

```scala
    def example = 2      // evaluated when called
    val example = 2      // evaluated immediately
    lazy val example = 2 // evaluated once when needed
    
    def square(x: Double)    // call by value
    def square(x: => Double) // call by name
    def myFct(bindings: Int*) { ... } // bindings is a sequence of int, containing a varying # of arguments
```

# Higher order functions

These are functions that take a function as a parameter or return functions.

```scala
    // sum() returns a function that takes two integers and returns an integer  
    def sum(f: Int => Int): (Int, Int) => Int = {  
      def sumf(a: Int, b: Int): Int = {...}  
      sumf  
    } 
    
    // same as above. Its type is (Int => Int) => (Int, Int) => Int  
    def sum(f: Int => Int)(a: Int, b: Int): Int = { ... } 
    
    // Called like this
    sum((x: Int) => x * x * x)          // Anonymous function, i.e. does not have a name  
    sum(x => x * x * x)                 // Same anonymous function with type inferred
    
    def cube(x: Int) => x * x * x  
    sum(x => x * x * x)(1, 10) // sum of cubes from 1 to 10
    sum(cube)(1, 10)           // same as above      
```

# Currying

Converting a function with multiple arguments into a function with a single argument that returns another function.

```scala
    def f(a: Int, b: Int): Int // uncurried version (type is (Int, Int) => Int)
    def f(a: Int)(b: Int): Int // curried version (type is Int => Int => Int)
```

# Classes

```scala
    class MyClass(x: Int, y: Int) {           // Defines a new type MyClass with a constructor  
      require(y > 0, "y must be positive")    // precondition, triggering an IllegalArgumentException if not met  
      def this (x: Int) = { ... }             // auxiliary constructor   
      def nb1 = x                             // public method computed every time it is called  
      def nb2 = y  
      private def test(a: Int): Int = { ... } // private method  
      val nb3 = x + y                         // computed only once  
      override def toString =                 // overridden method  
          member1 + ", " + member2 
      }
    
    new MyClass(1, 2) // creates a new object of type
```

# Class Organization

- **scala.Any** base type of all types. Has methods `hashCode` and `toString` that can be overloaded
- **scala.AnyVal** base type of all primitive types. (`scala.Double`, `scala.Float`, etc.)
- **scala.AnyRef** base type of all reference types. (alias of `java.lang.Object`, supertype of `java.lang.String`, `scala.List`, any user-defined class)
- **scala.Null** is a subtype of any `scala.AnyRef` (`null` is the only instance of type `Null`), and `scala.Nothing` is a subtype of any other type without any instance.

# Type Parameters

Similar to C++ templates or Java generics. These can apply to classes, traits or functions.

```scala
    class MyClass[T](arg1: T) { ... }  
    new MyClass[Int](https://gist.github.com/heathermiller/1)  
    new MyClass(1)   // the type is being inferred, i.e. determined based on the value arguments  
```

It is possible to restrict the type being used, e.g.

```scala
    def myFct[T <: TopLevel](arg: T): T = { ... } // T must derive from TopLevel or be TopLevel
    def myFct[T >: Level1](arg: T): T = { ... }   // T must be a supertype of Level1
    def myFct[T >: Level1 <: Top Level](arg: T): T = { ... }
```

## Variance

Given `A <: B`

If `C[A] <: C[B]`, `C` is covariant

If `C[A] >: C[B]`, `C` is contravariant

Otherwise C is nonvariant

```scala
    class C[+A] { ... } // C is covariant
    class C[-A] { ... } // C is contravariant
    class C[A]  { ... } // C is nonvariant
```

For a function, if `A2 <: A1` and `B1 <: B2`, then `A1 => B1 <: A2 => B2`.

Functions must be contravariant in their argument types and covariant in their result types, e.g.

```scala
    trait Function1[-T, +U] {
      def apply(x: T): U
    } // Variance check is OK because T is contravariant and U is covariant
    
    class Array[+T] {
      def update(x: T)
    } // variance checks fails
```

Find out more about variance in [lecture 4.4](https://class.coursera.org/progfun-2012-001/lecture/81) and [lecture 4.5](https://class.coursera.org/progfun-2012-001/lecture/83 "Link: https://class.coursera.org/progfun-2012-001/lecture/83")

Pattern Matching

Pattern matching is used for decomposing data structures:

```scala
    unknownObject match {
      case MyClass(n) => ...
      case MyClass2(a, b) => ...
    }
```

Here are a few example patterns

```scala
    (someList: List[T]) match {
      case Nil => ...          // empty list
      case x :: Nil => ...     // list with only one element
      case List(x) => ...      // same as above
      case x :: xs => ...      // a list with at least one element. x is bound to the head,
                               // xs to the tail. xs could be Nil or some other list.
      case 1 :: 2 :: cs => ... // lists that starts with 1 and then 2
      case (x, y) :: ps => ... // a list where the head element is a pair
      case _ => ...            // default case if none of the above matches
    }
```

The last example shows that every pattern consists of sub-patterns: it only matches lists with at least one element, where that element is a pair. `x` and `y` are again patterns that could match only specific types.

### Options

Pattern matching can also be used for `Option` values. Some functions (like `Map.get`) return a value of type `Option[T]` which is either a value of type `Some[T]` or the value `None`:

```scala
    val myMap = Map("a" -> 42, "b" -> 43)
    def getMapValue(s: String): String = {
      myMap get s match {
        case Some(nb) => "Value found: " + nb
        case None => "No value found"
      }
    }
    getMapValue("a")  // "Value found: 42"
    getMapValue("c")  // "No value found"
```

Most of the times when you write a pattern match on an option value, the same expression can be written more concisely using combinator methods of the `Option` class. For example, the function `getMapValue` can be written as follows:

```scala
    def getMapValue(s: String): String =
      myMap.get(s).map("Value found: " + _).getOrElse("No value found")
```

### Pattern Matching in Anonymous Functions

Pattern matches are also used quite often in anonymous functions:

```scala
    val pairs: List[(Char, Int)] = ('a', 2) :: ('b', 3) :: Nil
    val chars: List[Char] = pairs.map(p => p match {
      case (ch, num) => ch
    })
```

Instead of `p => p match { case ... }`, you can simply write `{case ...}`, so the above example becomes more concise:

```scala
    val chars: List[Char] = pairs map {
      case (ch, num) => ch
    }
```

# Collections

Scala defines several collection classes:

## Base Classes

- `Iterable` (collections you can iterate on)
- `Seq` (ordered sequences)
- `Set`
- `Map` (lookup data structure)

## Immutable Collections

- `List` (linked list, provides fast sequential access)
- `Stream` (same as List, except that the tail is evaluated only on demand)
- `Vector` (array-like type, implemented as tree of blocks, provides fast random access)
- `Range` (ordered sequence of integers with equal spacing)
- `String` (Java type, implicitly converted to a character sequence, so you can treat every string like a `Seq[Char]`)
- `Map` (collection that maps keys to values)
- `Set` (collection without duplicate elements)

## Mutable Collections

- `Array` (Scala arrays are native JVM arrays at runtime, therefore they are very performant)
- Scala also has mutable maps and sets; these should only be used if there are performance issues with immutable types

# Pairs (similar for larger Tuples)

```scala
    val pair = ("answer", 42)   // type: (String, Int)
    val (label, value) = pair   // label = "answer", value = 42  
    pair._1 // "answer"  
    pair._2 // 42  
```

# Ordering

There is already a class in the standard library that represents orderings: `scala.math.Ordering[T]` which contains comparison functions such as `lt()` and `gt()` for standard types. Types with a single natural ordering should inherit from the trait `scala.math.Ordered[T]`.

```scala
    import math.Ordering  
    
    def msort[T](xs: List[T])(implicit ord: Ordering) = { ...}  
    msort(fruits)(Ordering.String)  
    msort(fruits)   // the compiler figures out the right ordering  
```

# For-Comprehensions

A for-comprehension is syntactic sugar for `map`, `flatMap` and `filter` operations on collections.

The general form is `for (s) yield e`

- `s` is a sequence of generators and filters
- `p <- e` is a generator
- `if f` is a filter
- If there are several generators (equivalent of a nested loop), the last generator varies faster than the first
- You can use `{ s }` instead of `( s )` if you want to use multiple lines without requiring semicolons
- `e` is an element of the resulting collection

## Example 1

```scala
    // list all combinations of numbers x and y where x is drawn from
    // 1 to M and y is drawn from 1 to N
    for (x <- 1 to M; y <- 1 to N)
      yield (x,y)
```

is equivalent to

```scala
    (1 to M) flatMap (x => (1 to N) map (y => (x, y)))
```

## Translation Rules

A for-expression looks like a traditional for loop but works differently internally

`for (x <- e1) yield e2` is translated to `e1.map(x => e2)`

`for (x <- e1 if f) yield e2` is translated to `for (x <- e1.filter(x => f)) yield e2`

`for (x <- e1; y <- e2) yield e3` is translated to `e1.flatMap(x => for (y <- e2) yield e3)`

This means you can use a for-comprehension for your own type, as long as you define `map`, `flatMap` and `filter`.

For more, see [lecture 6.5](https://class.coursera.org/progfun-2012-001/lecture/111 "Link: https://class.coursera.org/progfun-2012-001/lecture/111").

## Example 2

```scala
    for {  
      i <- 1 until n  
      j <- 1 until i  
      if isPrime(i + j)  
    } yield (i, j)  
```

is equivalent to

```scala
    for (i <- 1 until n; j <- 1 until i if isPrime(i + j))
        yield (i, j)  
```

is equivalent to

```scala
    (1 until n).flatMap(i => (1 until i).filter(j => isPrime(i + j)).map(j => (i, j)))
```