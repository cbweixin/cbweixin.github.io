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
