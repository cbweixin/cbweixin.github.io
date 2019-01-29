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
```
result :

```
test7: mutable tree map===============
 Elpased time : 17678666ns
 Elpased time : 17ms
test8: immutable tree map===============
 Elpased time : 17895029ns
 Elpased time : 17ms

```

there are duplicate code of read files, we can wrap it as a function also
```scala
  def processWords(process : String => Unit) : Unit = {
    val in = new Scanner(getClass.getResourceAsStream("/myfile.txt"))
    try {
      while (in.hasNext()) {
        process(in.next())
      }
    } finally {
      in.close()
    }

  }
```

then rewrite the code like this:

```scala
  private def countWordsWithImutableMap2() : immutable.Map[String, Int] = {
    var words = new immutable.HashMap[String, Int]
    processWords(w => words += w -> (words.getOrElse(w,0) + 1))
    words
  }

  private def countWordsInFile2(): mutable.Map[String, Int] = {
    val words = new mutable.HashMap[String, Int]
    processWords(w => words(w) = words.getOrElse(w, 0) + 1)
    words
  }

```


## ex 6
```scala
  /**
    * ex 6
    *
    * Define a linked hash map that maps "Monday" to java.util.Calendar.MONDAY, and similarly for the other weekdays.
    * Demonstrate that the elements are visited in insertion order.
    *
    * @return
    */
  def weekdaysLinkedHashMap(): mutable.Map[String, Int] = {
    import java.util.Calendar._
    val days = scala.collection.mutable.LinkedHashMap(
      "MONDAY" -> MONDAY,
      "TUESDAY" -> TUESDAY,
      "WEDNESDAY" -> WEDNESDAY,
      "THURSDAY" -> THURSDAY,
      "FRIDAY" -> FRIDAY,
      "SATURDAY" -> SATURDAY,
      "SUNDAY" -> SUNDAY
    )

    days
  }
```

## ex 7

```scala

  /**
    * Print a table of all Java properties reported by the getProperties method of the java.lang.System
    * class, like this:
    * 53
    * java.runtime.name
    * sun.boot.library.path
    * java.vm.version
    * java.vm.vendor
    * java.vendor.url
    * path.separator
    * java.vm.name
    * | Java(TM) SE Runtime Environment
    * | /home/apps/jdk1.6.0_21/jre/lib/i386 | 17.0-b16
    * | Sun Microsystems Inc.
    * | http://java.sun.com/
    * |:
    * | Java HotSpot(TM) Server VM
    *
    * @return
    */
  def formatJavaProperties(): List[String] = {
    import scala.collection.JavaConversions.propertiesAsScalaMap
    val props = propertiesAsScalaMap(System.getProperties)
    val maxKeyLen = props.keySet.map(_.length).max

    val result = ListBuffer[String]()
    for((key,value)<-props){
      result += key.padTo(maxKeyLen, ' ') + " | " + value
    }

    result.toList
  }
```

result:

```
test9: print propers===============
java.runtime.name             | Java(TM) SE Runtime Environment
sun.boot.library.path         | /Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib
java.vm.version               | 25.141-b15
gopherProxySet                | false
java.vm.vendor                | Oracle Corporation
java.vendor.url               | http://java.oracle.com/
path.separator                | :
java.vm.name                  | Java HotSpot(TM) 64-Bit Server VM
file.encoding.pkg             | sun.io
user.country                  | US
sun.java.launcher             | SUN_STANDARD
sun.os.patch.level            | unknown
java.vm.specification.name    | Java Virtual Machine Specification
user.dir                      | /Users/xwei/workspace/network/scala-for-the-impatient
java.runtime.version          | 1.8.0_141-b15
java.awt.graphicsenv          | sun.awt.CGraphicsEnvironment
java.endorsed.dirs            | /Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/endorsed
os.arch                       | x86_64
java.io.tmpdir                | /var/folders/jj/5z5gvtyd1cbbbg7wv6lknbbw64qwnz/T/
line.separator                |

java.vm.specification.vendor  | Oracle Corporation
os.name                       | Mac OS X
sun.jnu.encoding              | UTF-8
java.library.path             | /Users/xwei/Library/Java/Extensions:/Library/Java/Extensions:/Network/Library/Java/Extensions:/System/Library/Java/Extensions:/usr/lib/java:.
java.specification.name       | Java Platform API Specification
java.class.version            | 52.0
sun.management.compiler       | HotSpot 64-Bit Tiered Compilers
os.version                    | 10.14.2
user.home                     | /Users/xwei
user.timezone                 |
java.awt.printerjob           | sun.lwawt.macosx.CPrinterJob
file.encoding                 | UTF-8
java.specification.version    | 1.8
java.class.path               | /Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/charsets.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/deploy.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext/cldrdata.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext/dnsns.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext/jaccess.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext/jfxrt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext/localedata.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext/nashorn.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext/sunec.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext/sunjce_provider.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext/sunpkcs11.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext/zipfs.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/javaws.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/jce.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/jfr.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/jfxswt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/jsse.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/management-agent.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/plugin.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/resources.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/rt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/lib/ant-javafx.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/lib/dt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/lib/javafx-mx.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/lib/jconsole.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/lib/packager.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/lib/sa-jdi.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/lib/tools.jar:/Users/xwei/workspace/network/scala-for-the-impatient/target/scala-2.12/classes:/Users/xwei/.sbt/boot/scala-2.12.4/lib/scala-library.jar:/Applications/IntelliJ IDEA.app/Contents/lib/idea_rt.jar
user.name                     | xwei
java.vm.specification.version | 1.8
sun.java.command              | chapt4.Exercises
java.home                     | /Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre
sun.arch.data.model           | 64
user.language                 | en
java.specification.vendor     | Oracle Corporation
awt.toolkit                   | sun.lwawt.macosx.LWCToolkit
java.vm.info                  | mixed mode
java.version                  | 1.8.0_141
java.ext.dirs                 | /Users/xwei/Library/Java/Extensions:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/ext:/Library/Java/Extensions:/Network/Library/Java/Extensions:/System/Library/Java/Extensions:/usr/lib/java
sun.boot.class.path           | /Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/resources.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/rt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/sunrsasign.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/jsse.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/jce.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/charsets.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/lib/jfr.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home/jre/classes
java.vendor                   | Oracle Corporation
file.separator                | /
java.vendor.url.bug           | http://bugreport.sun.com/bugreport/
sun.io.unicode.encoding       | UnicodeBig
sun.cpu.endian                | little
sun.cpu.isalist               |

Process finished with exit code 0
```


## ex 8

```scala
  /**
    *
    * ex 8
    *
    * Write a function minmax(values: Array[Int]) that returns a pair
    * containing the smallest and the largest values in the array.
    *
    * @param values
    * @return
    */
  def minmax(values: Array[Int]): (Int, Int) = (values.min, values.max)
```

## ex 9

```scala

  /**
    * ex 9
    *
    * Write a function lteqgt(values: Array[Int], v: Int) that returns a triple containing the
    * counts of values less than v, equal to v, and greater than v.
    *
    * @param values
    * @param v
    * @return
    */
  def lteqgt(values: Array[Int], v: Int): (Int, Int, Int) = {
    (values.count(_ < v), values.count(_ == v), values.count(_ > v))
  }
```
