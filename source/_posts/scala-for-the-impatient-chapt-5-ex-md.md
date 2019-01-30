title: scala-for-the-impatient-chapt-5-ex.md
date: 2019-01-30 06:55:58
categories: programming
tags: scala
---

## ex 1
```scala
 /**
    * ex 1
    *
    * Improve the Counter class in Section 5.1, “Simple Classes and Parameterless Methods,”
    * on page 55 so that it doesn’t turn negative at Int.MaxValue.
    *
    */
  class Counter {
    private var value = 0 // You must initialize the field
    def increment(): Either[String, Int] = {
      //      if(value == Int.MaxValue){
      if (value == 3) {
        Left("hi, reach the sky already..")
      }
      else {
        value += 1
        Right(value)
      }
    } // Methods are public by default
    def current() = value
  }
```
print the result
```scala
println("ex 1==========")
  val cnt = new Counter
  println(cnt.increment())
  println(cnt.increment())
  println(cnt.increment())
  println(cnt.increment())

```
got :
```
ex 1==========
Right(1)
Right(2)
Right(3)
Left(hi, reach the sky already..)
```
## ex 2
```scala
  /**
    * ex 2
    * Write a class BankAccount with methods deposit and withdraw, and a read-only property balance.
    *
    */
  class BankAccount {
    private var sum = 0

    def deposit(amount: Int) = {
      sum += amount
    }

    def withDraw(amount: Int): Either[String, Int] = {
      if (amount > sum) {
        Left("dude, why not rob the bank...")
      }
      else {
        sum -= amount
        Right(amount)
      }
    }

    def total = sum
  }

```

test
```scala
 val bank = new BankAccount
  bank.deposit(100)

  bank.withDraw(200) match {
    case Left(s) => println(s)
    case Right(i) => println(i)
  }
  println(bank.total)
```
got:

```
ex 2==========
dude, why not rob the bank...
100
```
