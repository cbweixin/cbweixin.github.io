title: scala-for-the-impatient-chapter-1-ex
date: 2019-01-09 20:50:02
categories: programming
tags: scala
---


## ex 1

```scala
/**
 * ex 1 :
 * The signum of a number is 1 if the number is positive, –1 if it is negative, and 0 if it is zero. Write a
 * function that computes this value.
 *
 * @param num
 * @return
 */
  private def getSignum(num: Int): Int = num.signum


```

or using pattern match
```scala
  private def getSignum2(num: Int) = num match {
    case 0 => 0
    case x if x > 0 => 1
    case _ => -1

  }
```


## ex 2

```scala
  /**
    * ex 2 :
    * What is the value of an empty block expression {}? What is its type?
    *
    * val a = {} , we got a: unit = ()
    * when do println(a), we got : ()
    * res2: Unit = ()
    *
    * so value of empty block expression is `()`
    * its type is `Unit`
    *
    */
  private def getTypeofBlockExpr = {
    val a = {}
    println(a)
  }
```

the result is like this :

```shell
()
```


## ex 3

```scala
  /**
    * Task 3:
    *
    * Come up with one situation where the assignment `x = y = 1` is valid in Scala.
    * (Hint: Pick a suitable type for `x`.)
    */
  private def task3(): Unit = {
    var y: Int = 0
    var x: Unit = ()
    x = y = 1 //    var x: Int = 2
    //    x = y = 1
    println("ex 3")
    println(x)
    println(y)
  }
```

result

```shell
()
1
```

## ex 4
```scala
  /**
    * exercise 4:
    * Write a Scala equivalent for the Java loop
    * for (int i = 10; i >= 0; i--) System.out.println(i);
    *
    */
  private def rewriteForLoop(n: Int) = {
    println("ex 4")
    for (i <- Range(n, -1, -1)) println(i)
  }

  private def rewriteForLoop_2(n: Int) = {
    println("ex 4")
    for(i <- n to 0 by -1) println(i)

  }

 private def rewriteForLoop2(num: Int) = {
    for (i <- num to(0, -1)) println(i)
  }
```

## ex 6
```scala
  /**
    * ex 6:
    * Write a for loop for computing the product of the Unicode codes of all letters in a string. For example, the
    * product of the characters in "Hello" is 9415087488L.
    *
    * @param str
    * @return
    */
  private def productOfUnicode(str: String): Long = {
    var product = 1L
    for (i <- str) product *= i.toInt
    product
  }

  private def productOfUnicode2(str: String): Long =
    str.foldLeft(1L)(_ * _.toInt)
```


## ex 7



