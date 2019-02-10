title: scala-PriorityQueue-ordering-explain
date: 2019-02-10 09:37:52
categories: programming
tags: scala
---
at my previous code, i used `PriorityQueue` to solve this issue. I am not that faimiliar with scala syntax, so it takes me some time to figure out how th customized the comparator.

for this part of code:

```scala
val pq = mutable.PriorityQueue[(String, Int)]()(Ordering.by {
        // in scala, by default, PQ is a max heap
        // first compare times, sort the appear time ascending, then compare string, sort the string descending
        // notice the "-t" ordering, the word appear few times, or the word alphabetical greater, woudl be on top
        // and deque firstly
        case (s, t) => (-t, s)
      })
```

you can see for `PriorityQueue` in scala, it uses `Ordering`, I am not sure of there is `Comparator` in scala or not,but the `Ordering` code looks more concise to me.

but you can still define a `Comprator -like` Ordering, such as [this](https://stackoverflow.com/questions/14927395/how-to-use-priority-queues-in-scala)
```scala
scala> import scala.collection.mutable.PriorityQueue
//  import scala.collection.mutable.PriorityQueue

scala> def diff(t2: (Int,Int)) = math.abs(t2._1 - t2._2)
// diff: (t2: (Int, Int))Int

scala> val x = new PriorityQueue[(Int, Int)]()(Ordering.by(diff))
// x: scala.collection.mutable.PriorityQueue[(Int, Int)] = PriorityQueue()

scala> x.enqueue(1 -> 1)

scala> x.enqueue(1 -> 2)

scala> x.enqueue(1 -> 3)

scala> x.enqueue(1 -> 4)

scala> x.enqueue(1 -> 0)

scala> x
```
you can see, the `diff` is a `Comparator`

also, you could define the `Ordering` - `implicitly`, such as [this](https://stackoverflow.com/questions/30893042/how-to-apply-ordering-to-a-scala-priority-queue):

```scala
implicit val MyOrdering: Ordering[(Int, (Int, Int), (Int, Int))] =
  Ordering.by {
    case (fst, (snd, _), (_, _)) => (fst, -snd)
  }
val queue = PriorityQueue(
  (1, (2,3), (4,5)),
  (2, (3,4), (5,6)),
  (2, (4,5), (6,7)))
```
If you define or import the implicit in a function, than it's everything until the end of the function. If you define it in a class, than it's everywhere whitin this class. the `queue` would implicitly ordered.

and finally, I met a small problem for inserting an element to priority queue.

![pic](https://i.imgur.com/xa2XvWR.png)

what wrong with this ` pq += (s,t)` ? isn't `(s,t)` a tuple ?

take a look at this pic below

for `+=` , it would also add multiple elements.

![doc](https://i.imgur.com/L0SuoIs.png)

so `(s,t)` is regarded as 2 element, instead of a `tuple`

solution:
1. `pq += (s->t)`  , you could create a tuple with '->'. see [link](https://alvinalexander.com/scala/scala-tuple-examples-syntax)
2. `pq +=((s,t))`
3. `pq.enque((s,t))`
4. `pq.enque(s->t)`









