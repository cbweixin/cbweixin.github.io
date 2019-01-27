title: scala-for-the-impatient-chapter-4-ex.md
date: 2019-01-26 14:42:26
categories: programming
tags: scala
---

firstly put the time profile function and print function

```scala
def prn(x: TraversableOnce[_]) = println(x.mkString(x.getClass.getSimpleName + "(", ", ", ")"))
```

stopwatch
```scala

  def time[R](block: => R): R = {
    val t0 = System.nanoTime()
    val result = block
    val t1 = System.nanoTime()
    println(" Elpased time : " + (t1 - t0) + "ns")
    println(" Elpased time : " + (t1 - t0) / 1000000 + "ms")
    result
  }
```

## ex 1
```scala
/**
    * Ex 1:
    *
    * Set up a map of prices for a number of gizmos that you covet. Then produce a second map with the same keys and
    * the prices at a 10 percent discount.
    *
    * @return
    */
  private def reducedPrices(): Map[String, Int] = {
    val gizmos = Map("iPhone" -> 600, "iPad" -> 500, "MacBook Pro" -> 2000, "ScalaDays 2016 Berlin" -> 750)
    gizmos.mapValues(price => price - price / 10)
  }


  private def reducedPrices2(): Map[String, Int] = {
    val gizmos = Map("iPhone" -> 600, "iPad" -> 500, "MacBook Pro" -> 2000, "ScalaDays 2016 Berlin" -> 750)
    for ((k, v) <- gizmos) yield (k, v - v / 10)
  }
```
test :
```scala
println("test1: mapValues==============")
prn(time(reducedPrices()))
println("test2: for yield===============")
prn(time(reducedPrices2()))
```

result:
```

test1: mapValues==============
 Elpased time : 74032222ns
 Elpased time : 74ms
$anon$2(iPhone -> 540, iPad -> 450, MacBook Pro -> 1800, ScalaDays 2016 Berlin -> 675)
test2: for yield===============
 Elpased time : 2313363ns
 Elpased time : 2ms
Map4(iPhone -> 540, iPad -> 450, MacBook Pro -> 1800, ScalaDays 2016 Berlin -> 675)
```

`for yield` a lot faster than map


## ex 2
```
  /**
    * ex 2:
    *
    * Write a program that reads words from a file. Use a mutable map to count how often each word appears. To read
    * the words, simply use a java.util.Scanner:
    * val in = new java.util.Scanner(new java.io.File("myfile.txt")) while (in.hasNext()) process in.next()
    * Or look at Chapter 9 for a Scalaesque way.
    *
    * @return
    */
  private def countWordsInFile(): mutable.Map[String, Int] = {
    val words = new mutable.HashMap[String, Int]
    val in = new Scanner(getClass.getResourceAsStream("/myfile.txt"))
    try {
      while (in.hasNext()) {
        words(in.next()) = words.getOrElse(in.next(), 0) + 1
      }
    } finally {
      in.close()
    }
    words
  }

```

## ex 3

```scala

  /**
    * Ex 3:
    *
    * Repeat the preceding exercise with an immutable map.
    *
    * @return
    */
  private def countWordsWithImutableMap() : immutable.Map[String, Int] = {
    var words = new immutable.HashMap[String, Int]
    val in = new Scanner(getClass.getResourceAsStream("/myfile.txt"))
    try {
      while (in.hasNext()) {
        words += (in.next() -> (words.getOrElse(in.next(), 0) + 1))
      }
    } finally {
      in.close()
    }
    words
  }

```

test result:

```
test3 : mutable map===============
 Elpased time : 61550627ns
 Elpased time : 61ms
test4 : immutable map===============
 Elpased time : 41690959ns
 Elpased time : 41ms
```

immutable faster than mutable


## ex 4
```scala
/**
    * ex  4
    *
    * Repeat the preceding exercise with a sorted map, so that the words are printed in sorted order.
    *
    * @param
    */

  private def countWordsWithSortedMap(): mutable.SortedMap[String, Int] = {
    var words = mutable.SortedMap[String, Int]()
    val in = new Scanner(getClass.getResourceAsStream("/myfile.txt"))
    try {
      while (in.hasNext()) {
        words(in.next()) = words.getOrElse(in.next(), 0) + 1
      }
    } finally {
      in.close()
    }
    words
  }


  /**
    * ex  4
    *
    * Repeat the preceding exercise with a sorted map, so that the words are printed in sorted order.
    *
    * @param
    */

  private def countWordsWithImutabeSortedMap(): immutable.SortedMap[String, Int] = {
    var words = immutable.SortedMap[String, Int]()
    val in = new Scanner(getClass.getResourceAsStream("/myfile.txt"))
    try {
      while (in.hasNext()) {
        words += (in.next() -> (words.getOrElse(in.next(), 0) + 1))
      }
    } finally {
      in.close()
    }
    words
  }


```

result:
```

test5 : mutable sorted map===============
 Elpased time : 51210905ns
 Elpased time : 51ms
test6: immutable sorted map===============
 Elpased time : 48273714ns
 Elpased time : 48ms
```
still immutable faster than mutable


## ex 5

```scala
/**
    * ex  5
    *
    * Repeat the preceding exercise with a tree map, so that the words are printed in sorted order.
    *
    * @param
    */

  private def countWordsWithImutabeTreeMap(): immutable.TreeMap[String, Int] = {
    var words = immutable.TreeMap[String, Int]()
    val in = new Scanner(getClass.getResourceAsStream("/myfile.txt"))
    try {
      while (in.hasNext()) {
        words += (in.next() -> (words.getOrElse(in.next(), 0) + 1))
      }
    } finally {
      in.close()
    }
    words
  }

  /**
    * ex  5
    *
    * Repeat the preceding exercise with a tree map, so that the words are printed in sorted order.
    *
    * @param
    */

  private def countWordsWithTreeMap(): mutable.TreeMap[String, Int] = {
    var words = mutable.TreeMap[String, Int]()
    val in = new Scanner(getClass.getResourceAsStream("/myfile.txt"))
    try {
      while (in.hasNext()) {
        words(in.next()) = words.getOrElse(in.next(), 0) + 1
      }
    } finally {
      in.close()
    }
    words
  }
result :
```
test7: mutable tree map===============
 Elpased time : 17678666ns
 Elpased time : 17ms
test8: immutable tree map===============
 Elpased time : 17895029ns
 Elpased time : 17ms
```

