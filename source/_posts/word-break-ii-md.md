title: word-break-ii.md
date: 2019-02-24 06:59:33
categories: programming
tags: scala
---

## leetcode workd break II with scala solution

### top-down DP

```scala
def wordBreak(s: String, wordDict: List[String]): List[String] = {
    if (s == null) {
      return List[String]()
    }
    val memo = mutable.Map[Int, ListBuffer[String]]()
    dfs(s, wordDict.toSet, 0, memo).toList
  }

  def dfs(s: String, words: Set[String], start: Int, memo: mutable.Map[Int, ListBuffer[String]]): ListBuffer[String] = {
    if (memo.contains(start)) {
      return memo(start)
    }
    val res = new ListBuffer[String]()

    for (i <- start until s.length) {
      val word = s.substring(start, i + 1)
      if (words.contains(word)) {
        if (i == s.length - 1) {
          res += word
        } else {
          val f: (String) => Unit = (s) => {
            val t = word + " " + s
            res += t
          }
          dfs(s, words, i + 1, memo).foreach(f)
        }
      }
    }

    memo += (start -> res)
    return res

```


### pattern match

```scala

 case class Memo[K, V](f: K => V) extends (K => V) {
    val cache = scala.collection.mutable.HashMap.empty[K, V]

    override def apply(x: K) = cache.getOrElseUpdate(x, f(x))
  }

object Solution {

    def wordBreak(s: String, wordDict: List[String]): List[String] = {

      val wordSet = wordDict.toSet

      type DP = Memo[String, List[String]]

      lazy val f: DP = Memo {
        case "" => List[String]("")
        case r =>
          wordDict.foldLeft(List[String]()) { (l, w) =>
            if (r.endsWith(w) && f(r.dropRight(w.length)).nonEmpty) {
              l.toList ::: f(r.dropRight(w.length)).map(str => if (str.nonEmpty) str + " " + w else w)
            } else l
          }
        case r if wordSet.contains(r) => List[String](r)
      }

      f(s)

    }
  }
```

