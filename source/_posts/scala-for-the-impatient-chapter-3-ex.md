title: scala-for-the-impatient-chapter-3-ex
date: 2019-01-12 21:22:14
categories: programming
tags: scala
---

## ex 1
```scala
  /**
    * EX 1 :
    * Write a code snippet that sets a to an array of n random integers between 0 (inclusive) and n (exclusive).
    *
    * @param n
    * @return
    */
  private def randArray(n: Int): Array[Int] = {
    val a = new Array[Int](n)
    for (i <- a.indices) {
      a(i) = Random.nextInt(n)
    }
    a
  }
```


## ex 2

```scala
  /**
    * Ex 2:
    * Write a loop that swaps adjacent elements of an array of integers. For example, Array(1, 2, 3, 4, 5) becomes
    * Array(2, 1, 4, 3, 5).
    *
    * @param arr
    * @return
    */
  private def swapArray(arr: Array[Int]): Array[Int] = {
    for (i <- arr.indices) {
      if ((i & 1) == 1) {
        val temp = arr(i)
        arr(i) = arr(i - 1)
        arr(i - 1) = temp
      }
    }
    arr
  }

  /**
    * Ex 2:
    * Write a loop that swaps adjacent elements of an array of integers. For example, Array(1, 2, 3, 4, 5) becomes
    * Array(2, 1, 4, 3, 5).
    *
    * @param arr
    * @return
    */
  private def swapArray2(arr: Array[Int]): Array[Int] = {
    for (i <- arr.indices if (i & 1) == 1) { // advanced loop
      val temp = arr(i)
      arr(i) = arr(i - 1)
      arr(i - 1) = temp
    }
    arr
  }


  private def swapArray3(arr: Array[Int]): Array[Int] = {
    arr.grouped(2).flatMap(_.reverse).toArray
  }
```

## ex 3
```scala
  /**
    * Ex 3:
    *
    * Repeat the preceding assignment, but produce a new array with the swapped values. Use for/yield.
    *
    * @param arr
    * @return
    */
  private def foryieldArray(arr: Array[Int]) = {
    for (i <- arr.indices) yield if ((i & 0x1) == 0) if (i + 1 < arr.length) {
      arr(i + 1)
    } else {
      arr(i)
    } else {
      arr(i - 1)
    }
  }


  /**
    * Ex 3:
    *
    * Repeat the preceding assignment, but produce a new array with the swapped values. Use for/yield.
    *
    * @param arr
    * @return
    */
  private def foryieldArray2(arr: Array[Int]) = {
    for (i <- 0 until arr.length) yield arr(if (i % 2 == 0) { // be aware for yield loop , there is no `{` after
      // `for`, otherwise it is syntax error
      if (i + 1 < arr.length) {
        i + 1
      } else {
        i
      }
    } else {
      i - 1
    })
  }

  private def foryieldArray3(arr: Array[Int]) = {
//    (for {x <- arr.grouped(2); y <- x.reverse} yield y).toArray
    (for (x <- arr.grouped(2); y <- x.reverse) yield y).toArray

  }

```

## ex 4

```scala
  /**
    * Ex 4:
    * Given an array of integers, produce a new array that contains all positive values of the original array, in
    * their original order, followed by all values that are zero or negative, in their original order.
    *
    * @param arr
    * @return
    */
  private def reArrangeArray(arr: Array[Int]): Array[Int] = {
    val b = new Array[Int](arr.size)
    var idx = 0
    for (i <- arr.indices if arr(i) > 0) {
      b(idx) = arr(i)
      idx += 1
    }
    for (i <- arr.indices if arr(i) <= 0) {
      b(idx) = arr(i)
      idx += 1
    }
    b
  }

  private def reArrangeArray(arr: Array[Int]): Array[Int] = {
    val (a,b) = arr.partition(_ > 0)

    a ++ b

  }
```
