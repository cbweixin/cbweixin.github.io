title: scala-foldLeft-vs-foldRight.md
date: 2019-02-22 07:44:39
categories: programming
tags: scala
comments: true
---

## Definition of `foldLeft` and `foldRight`

### `foldLeft`

![foldLeft](http://imgur.com/8EkwDep.png)

please notice the definition of `op` : `(op: (B, A) =>  B)`

here, `A` is the element of the caller, `B` is the intermedia result of function `op`

### `foldRight`

![foldRight](http://imgur.com/RGrFri8.png)


## Example

### `foldLeft`

```scala
class Foo(val name: String, val age: Int, val sex: Symbol)

object Foo {
  def apply(name: String, age: Int, sex: Symbol) = new Foo(name, age, sex)
}

val fooList = Foo("Hugh Jass", 25, 'male) ::
  Foo("Biggus Dickus", 43, 'male) ::
  Foo("Incontinentia Buttocks", 37, 'female) ::
  Nil


val stringList = fooList.foldLeft[List[String]](List[String]()) { (z, f) =>
  val title = f.sex match {
    case 'male => "Mr."
    case 'female => "Ms."
  }
  z :+ s"$title ${f.name}, ${f.age}"
}
```

the result is :

```
stringList: List[String] = List(Mr. Hugh Jass, 25, Mr. Biggus Dickus, 43, Ms. Incontinentia Buttocks, 37)
```

### `foldRight`

```scala
val stringList2 = fooList.foldRight[List[String]](List[String]()) { (z, f) =>
  val title = f.sex match {
    case 'male => "Mr."
    case 'female => "Ms."
  }
  z :+ s"$title ${f.name}, ${f.age}"
}
```

you would get error:

```
Error:(34, 23) value sex is not a member of List[String]
val title = f.sex match {
```

why ?

notice this code snippet : `{ (z, f) =>  }`

for `foldRight`, `f` is the intermedia result of the function, of course he would have the `sex` symbol.

so we need to switch them like this:

```scala
val stringList2 = fooList.foldRight[List[String]](List[String]()) { (f, z) =>
  val title = f.sex match {
    case 'male => "Mr."
    case 'female => "Ms."
  }
  z :+ s"$title ${f.name}, ${f.age}"
}
```

result:

```
stringList2: List[String] = List(Ms. Incontinentia Buttocks, 37, Mr. Biggus Dickus, 43, Mr. Hugh Jass, 25)
```
