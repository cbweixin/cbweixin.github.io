title: scala-for-the-impatient-chapter-4-ex.md
date: 2019-01-26 14:42:26
categories: programming
tags: scala
---

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
```

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


