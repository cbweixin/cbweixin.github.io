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

	private def productOfUnicode3(str: String): Long = str.map(_.toLong).product
```


## ex 9
```scala

  /**
    * Ex 9
    * Make the function of the preceding exercise a recursive function.
    *
    * @param str
    * @param idx
    * @return
    */
  private def productRecursive(str: String, idx: Int): Long = {
    if (idx == -1) 1
    else str.charAt(idx).toLong * productRecursive(str, idx - 1)
  }

  private def productRecursive2(str: String): Long = {
    if (str.isEmpty) 1 else str.head.toLong * productRecursive2(str.tail)
  }
```


## ex 10

```scala
 /**
    *
    * Write a function that computes xn, where n is an integer. Use the following recursive definition:
    * • xn = y · y if n is even and positive, where y = xn / 2.
    * • xn = x · xn – 1 if n is odd and positive. • x0 = 1.
    * • xn = 1 / x–n if n is negative.
    *
    * @param x
    * @param n
    * @return
    */
  private def getPow(x: Int, n: Int): Double = {
    val sign = if (n >= 0) 1 else -1
    val m = Math.abs(n)
    val t =
      if (m == 0) 1L
      else if (m % 2 == 0) {
        val y = getPow(x, m / 2)
        y * y
      } else x * getPow(x, m - 1)
    if (sign == 1) t else 1L / t
  }

  private def getPow2(x: Int, n: Int): Double = {
    if (n == 0) 1
    else if (n < 0) 1 / getPow(x, -n)
    else {
      if (n % 2 == 0) {
        val y = getPow(x, n / 2)
        y * y
      } else x * getPow(x, n - 1)
    }
  }

```

## ex 11
```scala

  /**
    * Define a string interpolator date so that you can define a java.time.LocalDate as date"$year-$month-$day". You
    * need to define an “implicit” class with a date method, like this:
    * implicit class DateInterpolator(val sc: StringContext) extends AnyVal { def date(args: Any*): LocalDate = . . .
    * }
    * args(i) is the value of the ith expression. Convert each to a string and then to an integer, and pass them to
    * the LocalDate.of method. If you already know some Scala, add error handling. Throw an exception if there aren’t
    * three arguments, or if they aren’t integers, or if they aren’t separated by dashes. (You get the strings in
    * between the expressions as sc.parts.)
    *
    * @param sc
    */
  implicit class DateInterpolator(val sc: StringContext) extends AnyVal {
    def date(args: Any*): LocalDate = {
      if (args.length != 3)
        throw new IllegalArgumentException(
          "argument number is wrong, have to be 3")
      val paras = new Array[Integer](3)
      try {
        var i = 0
        for (arg <- args) {
          paras(i) = arg.toString.toInt
        }
      } catch {
        case e: Exception => e.printStackTrace()
      }

      LocalDate.of(paras(0), paras(1), paras(2))
    }
  }

}
```



