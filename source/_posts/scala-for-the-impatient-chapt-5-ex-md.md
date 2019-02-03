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


## ex 3

```scala
  /**
    * Write a class Time with read-only properties hours and minutes and a method
    * before(other: Time): Boolean
    * that checks whether this time comes before the other.
    * A Time object should be constructed as new Time(hrs, min), where hrs is in
    * military time format (between 0 and 23).
    *
    * @param h
    * @param min
    */
  class Time(private val h: Int, private val min: Int) {

    def hour: Int = h
    def minute: Int = min

    def before(other: Time): Boolean ={
      if(h == other.h){
        return min < other.min
      }
      h < other.h
    }

    def before2(other: Time) : Boolean = h * 60 + min < other.h * 60 + other.min
  }
```

test:
```scala
 println("Time is: " + time.hour + ":" + time.minute)

  println(time.before(new Time(2, 30)))
  println(time.before(new Time(1, 10)))

  println(time.before2(new Time(2, 30)))
  println(time.before2(new Time(1, 10)))
```

result:

```
ex 3==============
Time is: 1:30
true
false
true
false
```


## ex 4
```scala
  /**
    *
    * Reimplement the Time class from the preceding exercise so that the internal representation is the number
    * of minutes since midnight (between 0 and 24 × 60 – 1). Do not change the public
    * interface. That is, client code should be unaffected by your change.
    *
    * @param h
    * @param min
    */
  class Time2(private val h: Int, private val min: Int) {
    private val mins: Int = h * 60 + min

    def hour: Int = h
    def minute: Int = min

    def before(other: Time2) : Boolean = mins < other.mins
  }
```
test:
```scala
 println("ex 4==============")
  val time2 = new Time2(1, 30)
  println("Time is: " + time2.hour + ":" + time2.minute)

  println(time2.before(new Time2(2, 30)))
  println(time2.before(new Time2(1, 10)))
```

result:
```
ex 4==============
Time is: 1:30
true
false
```
