title: leetcode-1428-leftmost-col-with-at-least-a-one
date: 2022-06-26 13:48:55
categories:
- algorithm and data structure
tags:
- leetcode
---

# leetcode 1428

## solution 1, traverse
runtime 221ms, 9%

![1428-1](https://user-images.githubusercontent.com/1326906/175833502-6c92709b-483c-4a81-af63-7896cee2dedf.png)

```python

    def leftMostColumnWithOne(self, binaryMatrix: 'BinaryMatrix') -> int:
        rows, cols = binaryMatrix.dimensions()
        r, c = 0, cols - 1

        while r < rows and c >= 0:
            if binaryMatrix.get(r, c) == 0:
                r += 1
            else:
                c -= 1

        # If we never left the last column, it must have been all 0's.
        return c + 1 if c != cols - 1 else -1
```


## solution 2, binary search
runtime 111ms, 92.5%
![1428-2](https://user-images.githubusercontent.com/1326906/175833507-9396869b-1c14-4533-82df-17d5cb6d5906.png)
```python

    # binary_search
    def leftMostColumnWithOne(self, binaryMatrix: 'BinaryMatrix') -> int:
        rows, cols = binaryMatrix.dimensions()

        # find the first on in column
        def find_one_columnwise(col):
            if col < 0:
                return -1
            for i in range(rows):
                if binaryMatrix.get(i, col) == 1:
                    return i
            return -1

        # find first one in row
        def binary_search(row, col):
            left, right = 0, col
            while left < right:
                mid = (left+right)>>1
                if binaryMatrix.get(row,mid) == 0:
                    left = mid + 1
                else:
                    right = mid

            return left

        # start from last column
        col = cols -1
        # find the first 1 in col, record its row r
        while (r := find_one_columnwise(col)) >= 0:
            # do binary search, find the first one in this row r
            # and record its column, and prepare start the loop from column-1 again
            col = binary_search(r, col) -1

        return -1 if col == cols -1 else col+1
```
