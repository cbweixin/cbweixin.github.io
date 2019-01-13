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

  /**
    * Ex 4:
    * Given an array of integers, produce a new array that contains all positive values of the original array, in
    * their original order, followed by all values that are zero or negative, in their original order.
    *
    * @param arr
    * @return
    */
  private def reArrangeArray2(arr: Array[Int]): Array[Int] = {
    arr.filter(_ > 0) ++ arr.filter(_ <= 0)
  }
```

## ex 5

```scala
  /**
    * Ex 5 :
    * How do you compute the average of an Array[Double]?
    *
    * @param arr
    * @return
    */
  private def calcAverage(arr: Array[Double]): Double = arr.sum / arr.size
```

## ex 6
```scala
  /**
    * Ex 6 :
    * How do you rearrange the elements of an Array[Int] so that they appear in
    * reverse sorted order? How do you do the same with an ArrayBuffer[Int]?
    *
    * @param arr
    * @return
    */
  private def reverseSortArray(arr: Array[Int]): Array[Int] = arr.sorted(Ordering.Int.reverse)

  /**
    * Ex 6 :
    * How do you rearrange the elements of an Array[Int] so that they appear in
    * reverse sorted order? How do you do the same with an ArrayBuffer[Int]?
    *
    * @param arr
    * @return
    */
  private def reverseSortArray2(arr: Array[Int]): Array[Int] = arr.sortWith(_ > _)

  /**
    *
    * Ex 6 :
    * reverse and sort an Arraybuf
    *
    * @param buf
    * @return
    */
  private def reverseSortArrayBuf(buf: ArrayBuffer[Int]): ArrayBuffer[Int] = {
    val arr = reverseSortArray(buf.toArray)
    buf.clear()
    arr.copyToBuffer(buf)
    buf
  }

  private def reverseSortArrayBuf(buf: ArrayBuffer[Int]): ArrayBuffer[Int] = {
    buf.sortWith(_ > _)
  }
```

## ex 7
```scala
  /**
    * Ex 7 :
    * Write a code snippet that produces all values from an array with duplicates
    * removed. (Hint: Look at Scaladoc.)
    *
    * @param arr
    * @return
    */
  private def getDistinctArray(arr: Array[Int]): Array[Int] = arr.distinct

  private def getDistinctArray2(arr: Array[Int]): Array[Int] = {
    arr.toSet.toArray
  }
```

## ex 8
```scala

  /**
    * EX 8 :
    *
    * Suppose you are given an array buffer of integers and want to remove all but the first negative number. Here is
    * a sequential solution that sets a flag when the first negative number is called, then removes all elements beyond.
    *
    * this method is wrong because it changes the postion of the original array
    *
    * @param arr
    * @return
    */
  private def keepFirstNegative(arr: ArrayBuffer[Int]): ArrayBuffer[Int] = {
    val neg = arr.filter(_ < 0)
    arr.filter(_ > 0) ++ neg.dropRight(neg.size - 1)

  }

  /**
    * EX 8 :
    *
    * Suppose you are given an array buffer of integers and want to remove all but the first negative number. Here is
    * a sequential solution that sets a flag when the first negative number is called, then removes all elements beyond.
    *
    * this method is wrong because it changes the postion of the original array
    *
    * @param arr
    * @return
    */
  private def keepFirstNegative2(arr: ArrayBuffer[Int]): ArrayBuffer[Int] = {
    val indexes = for (i <- arr.indices if arr(i) < 0) yield i
    val a = indexes.drop(1)
    for (i <- a.indices.reverse) { // notice you need to reverse it when drop
      arr.remove(a(i))
    }
    arr
  }
```


