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

## ex 5:

```scala
  /**
    *
    * Make a class Student with read-write JavaBeans properties name (of type String) and id (of type Long).
    * What methods are generated? (Use javap to check.)
    * Can you call the JavaBeans getters and setters in Scala? Should you?
    */
  class Student {
    @BeanProperty
    var name: String = ""

    @BeanProperty
    var id: Long = 0
  }

class Student2(@BeanProperty var name : String, @BeanProperty var id : Long)
```

test:

```scala
println("ex 5==============")
  val st = new Student
  st.setId(124)
  st.setName("hello")
  println(st.id)
  println(st.name)

  val st2 = new Student2("hello",123)
  println(st2.id)
  println(st2.name)
  st2.setName("world")
  st2.setId(345)
  println(st2.id)
  println(st2.name)
```

result:

```
ex 5==============
124
hello
123
hello
345
world
```

decompile the class, got:

```shell
 xwei  mtvl15397a13f  ~  workspace  …  scala-2.12  classes  chapt5  master 4???? $  ls
Exercises$.class      Exercises$Student.class     Exercises$Time2.class
Exercises$BankAccount.class   Exercises$Student2.class    Exercises$delayedInit$body.class
Exercises$Counter.class     Exercises$Time.class      Exercises.class
 xwei  mtvl15397a13f  ~  workspace  …  scala-2.12  classes  chapt5  master 4???? $  javap -private Exercises\$Student
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF8
Warning: Binary file Exercises$Student contains chapt5.Exercises$Student
Compiled from "Exercises.scala"
public class chapt5.Exercises$Student {
  private java.lang.String name;
  private long id;
  public java.lang.String name();
  public void name_$eq(java.lang.String);
  public long id();
  public void id_$eq(long);
  public long getId();
  public java.lang.String getName();
  public void setId(long);
  public void setName(java.lang.String);
  public chapt5.Exercises$Student();
}
 xwei  mtvl15397a13f  ~  workspace  …  scala-2.12  classes  chapt5  master 4???? $  javap -private Exercises\$Student2
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF8
Warning: Binary file Exercises$Student2 contains chapt5.Exercises$Student2
Compiled from "Exercises.scala"
public class chapt5.Exercises$Student2 {
  private java.lang.String name;
  private long id;
  public java.lang.String name();
  public void name_$eq(java.lang.String);
  public long id();
  public void id_$eq(long);
  public long getId();
  public java.lang.String getName();
  public void setId(long);
  public void setName(java.lang.String);
  public chapt5.Exercises$Student2(java.lang.String, long);
}
```

## ex 6
```scala
/**
    *
    * In the Person class of Section 5.1, “Simple Classes and Parameterless Methods,” on page 55,
    * provide a primary constructor that turns negative ages to 0.
    * @param age
    */
  class Person(var age: Int = 0){
    if(age < 0 ) age = 0
  }
```

test:

```scala
println("ex 6==============")
  val p = new Person(5)
  println(p.age)

  val n = new Person(-5)
  println(n.age)
```

result:

```
ex 6==============
5
0

```


## ex 7

```scala

  /**
    * Write a class Person with a primary constructor that accepts a string containing a first name,
    * a space, and a last name, such as new Person("Fred Smith"). Supply read-only properties
    * firstName and lastName. Should the primary constructor
    * parameter be a var, a val, or a plain parameter? Why?
    *
    * @param name
    */
  class Person2(name: String) {
    val firstName = name.split(" ")(0)
    val lastName = name.split(" ")(1)

    override def toString: String = "Person2(" + firstName + ", " + lastName + ")"
  }


  // primary constructor is a private one
  class Person3 private (names: Array[String]) {
    val firstName : String = names(0)
    val lastName: String = names(1)

    // second(auxiliary) constructor
    def this(name: String){
      this(Person3.parsedName(name))
    }

    override def toString: String = "Person3(" + firstName + ", " + lastName + ")"
  }

  private object Person3{
    def parsedName(name: String) : Array[String] = {
      val names = name.split(" ")
      if(names.length < 2){
        throw new IllegalArgumentException("shuld have firstName and lastName")
      }
      names
    }
  }

```

test:

```scala
 println("ex 7==============")
  val p2 = new Person2("Bill Gates")
  println(p2)

  val p3 = new Person3("hello kitty")
  println(p3)```

result:

```
ex 7==============
Person2(Bill, Gates)
Person3(hello, kitty)
```

I have `Person2` and `Person3`m, what's the differences?

```shell
 xwei  mtvl15397a13f  ~  workspace  …  scala-2.12  classes  chapt5  master 6???? $  javap -private Exercises\$Person2.class
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF8
Compiled from "Exercises.scala"
public class chapt5.Exercises$Person2 {
  private final java.lang.String firstName;
  private final java.lang.String lastName;
  public java.lang.String firstName();
  public java.lang.String lastName();
  public java.lang.String toString();
  public chapt5.Exercises$Person2(java.lang.String);
}
 xwei  mtvl15397a13f  ~  workspace  …  scala-2.12  classes  chapt5  master 6???? $  javap -private Exercises\$Person3.class
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF8
Compiled from "Exercises.scala"
public class chapt5.Exercises$Person3 {
  private final java.lang.String firstName;
  private final java.lang.String lastName;
  public java.lang.String firstName();
  public java.lang.String lastName();
  public java.lang.String toString();
  private chapt5.Exercises$Person3(java.lang.String[]);
  public chapt5.Exercises$Person3(java.lang.String);
}
```

## ex 8

```scala
  /**
    *
    * Make a class Car with read-only properties for manufacturer, model name, and model year,
    * and a read-write property for the license plate. Supply four constructors.
    * All require the manufacturer and model name. Optionally, model year and license plate can
    * also be specified in the constructor. If not, the model year is set to -1 and the license
    * plate to the empty string. Which constructor are you choosing as the primary constructor? Why?
    *
    * @param manufacture
    * @param modelName
    * @param modelYear
    * @param licence
    */
  class Car(val manufacture: String,
            val modelName: String,
            val modelYear: Int = -1,
            val licence: String = "") {
    def this(manufacture: String, modelName: String, license: String) {
      this(manufacture, modelName, -1, license)
    }

    override def toString: String =
      "Car(" + manufacture + ", " + modelName + ", " + modelYear + ", " + licence + ")"
  }
```

test:

```scala
 println("ex 8==============")
  val car1 = new Car("honda", "civic", 1982, "12366")
  println(car1)
  val car2 = new Car("honda", "civic", 1982)
  println(car2)
  val car3 = new Car("honda", "civic", "12356")
  println(car3)
  val car4 = new Car("honda", "civic")
  println(car4)

```

result:

```
ex 8==============
Car(honda, civic, 1982, 12366)
Car(honda, civic, 1982, )
Car(honda, civic, -1, 12356)
Car(honda, civic, -1, )
```

after decompile, the java class is looks like this :

```java
package chapt5;

public class Exercises$Car
{
  private final String manufacture;

  public String manufacture()
  {
    return this.manufacture;
  }

  public String modelName()
  {
    return this.modelName;
  }

  public int modelYear()
  {
    return this.modelYear;
  }

  public String licence()
  {
    return this.licence;
  }

  public Exercises$Car(String manufacture, String modelName, String license)
  {
    this(manufacture, modelName, -1, license);
  }

  public String toString()
  {
    return "Car(" + manufacture() + ", " + modelName() + ", " + modelYear() + ", " + licence() + ")";
  }

  public Exercises$Car(String manufacture, String modelName, int modelYear, String licence) {}
}
```


## ex 10

```scala
  /**
    *
    * Consider the class
    *
    * class Employee(val name: String, var salary: Double) {
    *   def this() { this("John Q. Public", 0.0) }
    * }
    *
    * Rewrite it to use explicit fields and a default primary constructor. Which form do you prefer? Why?
    *
    */
  class Employee {
    val name : String = "John Q. Pbulic"
    val salary : Double = 0.0

    override def toString: String = "Employee(" + name + ", " + salary + ")"
  }

```

decompile:

```java
public class Exercises$Employee
{
  public String name()
  {
    return this.name;
  }

  private final String name = "John Q. Pbulic";

  public double salary()
  {
    return this.salary;
  }

  private final double salary = 0.0D;

  public String toString()
  {
    return "Employee(" + name() + ", " + salary() + ")";
  }
}
```

another attempt:

```scala
  class Employee2 {
    private var _name = "John Q. Public"
    var salary = 0.0

    def this(n: String, s: Double) {
      this()
      _name = n;
      salary = s;
    }

    def name = _name // read-only property, but private var
    override def toString = "Employee2(%s, %f)".format(name, salary)
  }
```

decompile:
```java
import scala.Predef.;
import scala.collection.immutable.StringOps;
import scala.runtime.BoxesRunTime;

public class Exercises$Employee2
{
  private String _name()
  {
    return this._name;
  }

  private void _name_$eq(String x$1)
  {
    this._name = x$1;
  }

  private String _name = "John Q. Public";

  public double salary()
  {
    return this.salary;
  }

  public void salary_$eq(double x$1)
  {
    this.salary = x$1;
  }

  private double salary = 0.0D;

  public Exercises$Employee2(String n, double s)
  {
    this();
    _name_$eq(n);
    salary_$eq(s);
  }

  public String name()
  {
    return _name();
  }

  public String toString()
  {
    return new StringOps(Predef..MODULE$.augmentString("Employee2(%s, %f)")).format(Predef..MODULE$.genericWrapArray(new Object[] { name(), BoxesRunTime.boxToDouble(salary()) }));
  }

  public Exercises$Employee2() {}
}

```

attempt 3:
```scala
  class Employee3(private var n: String, private var s: Double){
    def name: String = n
    def salary: Double = s

    override def toString = "Employee3(%s, %f)".format(name, salary)

  }


```
decompile,got:

```java
import scala.Predef.;
import scala.collection.immutable.StringOps;
import scala.runtime.BoxesRunTime;

public class Exercises$Employee3
{
  private String n;

  private String n()
  {
    return this.n;
  }

  private void n_$eq(String x$1)
  {
    this.n = x$1;
  }

  private double s()
  {
    return this.s;
  }

  private void s_$eq(double x$1)
  {
    this.s = x$1;
  }

  public String name()
  {
    return n();
  }

  public double salary()
  {
    return s();
  }

  public String toString()
  {
    return new StringOps(Predef..MODULE$.augmentString("Employee3(%s, %f)")).format(Predef..MODULE$.genericWrapArray(new Object[] { name(), BoxesRunTime.boxToDouble(salary()) }));
  }

  public Exercises$Employee3(String n, double s) {}
}
```

compare `Employee2` and `Employe3`, looks like only 2 has a auxiliary constructor.

test:

```scala
println("ex 10==============")
  val e = new Employee
  println(e)
  val e1 = new Employee2("abc", 10000)
  println(e1)
  val e2 = new Employee3("def", 200000)
  println(e2)
```

got:

```
ex 10==============
Employee(John Q. Pbulic, 0.0)
Employee2(abc, 10000.000000)
Employee3(def, 200000.000000)
```
