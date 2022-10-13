title: design-most-recently-used-queue
date: 2022-10-09 16:26:54
categories:
- programming
tags:
- leetcode
---

# leetcode 1756 design most recently used queue

## problem description
[lc 1756](https://github.com/cbweixin/leetcode-python/blob/0e46a1b889ac3d5594958fc9694865ddd24bd521/leetcode-py3/leetcode/editor/en/doc/content/design-most-recently-used-queue-1756.md)

## solution 1 - regular list
- time: `O(N)`
- space: `O(N)`
```python
class MRUQueue:

    def __init__(self, n: int):
        self.que = list(range(1, n+1))


    def fetch(self, k: int) -> int:
        n = self.que[k-1]
        self.que = self.que[:k-1] + self.que[k:] + [n]
        return n

```
- run time: 554ms
- memory: 15MB

## solution2 - red-black tree(balance tree)
- time: `O(lgN)`
- space: `O(N)`
```python
class MRUQueue:

    def __init__(self, n: int):
        """
        红黑树可以做到插入和删除都是O(lgn)时间，同时能保证排序。python的SortedList内部实现了红黑树，直接用就行。
        注意SortedList内的元素按OrderIndex排序，要记住并不断更新最大的OrderIndex，确保插入SortedList尾部的OrderIndex是最大的。

        作者：yuan-zhi-b
        链接：https://leetcode.cn/problems/design-most-recently-used-queue/solution/python3-hong-hei-shu-sortedlist-by-yuan-7ev74/
        来源：力扣（LeetCode）
        著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
        @param n:
        """
        from sortedcontainers import SortedList
        self.max_idx = n
        self.arr = SortedList([v, v] for v in range(1, n + 1))

    def fetch(self, k: int) -> int:
        _, val = self.arr.pop(k - 1)
        self.max_idx += 1
        self.arr.add([self.max_idx, val])
        return val
```

- runtime: 254ms
- memory: 15.8MB


## solution 3 - sqrt decomposition
- time: `O(sqrt(N))`
- space: `O(N)`

```python
class MRUQueue:

    def __init__(self, n: int):
        bucket_size = math.ceil(sqrt(n))
        self.buckets = [deque()]

        for i in range(1, n+1):
            if len(self.buckets[-1]) == bucket_size:
                self.buckets.append(deque())
            self.buckets[-1].append(i)

    def fetch(self, k: int) -> int:
        for idx, bucket in enumerate(self.buckets):
            # this speed up the look up process
            if k > len(bucket):
                k -= len(bucket)
            else:
                break
        # move element to the tail
        self.buckets[-1].append(bucket[k-1])

        # update bucket, since we removed bucket[k-1]
        new_bucket = deque()
        for i , e in enumerate(bucket, 1):
            if i != k:
                new_bucket.append(e)
        self.buckets[idx] = new_bucket

        # re-balancing
        for i in range(idx, len(self.buckets)-1):
            self.buckets[i].append(self.buckets[i+1].popleft())

        return self.buckets[-1][-1]
```
- runtime: 455ms
- memory: 15.2MB

## solution 4 - sqrt decomposition
similiar with above, but use a little bit match to speed up
```python
class MRUQueue:

    def __init__(self, n: int):
        bucket_size = math.ceil(sqrt(n))
        self.buckets = []
        # record the first index of number for each bucket, for ex, bucket 1 start from 1, bucekt 2 start from
        # bucket_size +1, bucket 3 start from 2*bucket_size + 1, etc
        self.index = []

        for i in range(1, n+1):
            ii = (i-1)//bucket_size
            # 1 -> bucket_size would go to bucket 1
            # bucket_size -> 2*bucket_size would go to bucket 2 , etc
            if ii == len(self.buckets):
                self.buckets.append([])
                self.index.append(i)
            self.buckets[-1].append(i)


    def fetch(self, k: int) -> int:
        # binary search, quickly locate the bucket for k
        idx = bisect_right(self.index, k)-1
        # pop the element at index k
        self.buckets[-1].append(self.buckets[idx].pop(k - self.index[idx]))

        # re-balancing, need to guarantee each has size sqrt(n)
        for i in range(idx, len(self.buckets)-1):
            self.buckets[i].append(self.buckets[i+1].pop(0))

        return self.buckets[-1][-1]

```
- runtime: 297ms
- memory: 15.3MB





