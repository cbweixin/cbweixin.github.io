title: maximum-of-minimum-values-in-all-subarrays
date: 2023-01-07 20:14:23
categories: programming
tags:
- leetcode
---

# description
[problem description](https://github.com/cbweixin/leetcode-python/blob/0a3e300812daf6077b11515da0f3550b3e15d0bc/leetcode-py3/leetcode/editor/en/doc/content/maximum-of-minimum-values-in-all-subarrays-1950.md#L8)


# analysis
this is a mono stack issue.
check the pic below : ![pic](https://raw.githubusercontent.com/cbweixin/seleniumdemo/master/2023/01/upgit_20230107_1673152107.png)

for each number, we try to expand both left and right until we hit numbers on both end which less than this number.

let's say A is the number, we firstly go to left , until we hit B, then go to right, until we hit C. so A is the min between B and C. 

assume we have a ans array, 
       `ans[0]` -stands for numbers with gap 0, ie, for each individual number, `max[a]`
       `ans[1]` - numbers with gap 1, so this means the maximum of minimum value of `max([[a[0],a[1]], [a[1], a[2]],...., [a[n-2], a[n-1]]])`
       ...
       `ans[n-1]` - `min(a)`
so if B->C gap is L1, then we update `ans[L1] = A`, after we got such an array, then we reverse traverse this array, update the `ans[i]` to `max(ans[j]) and j > i`, why?
assume we have another number `D`, after expand left and right, we got the gap, let's say `L1`, `L1 > L`, and `D > B`, because the problem require maximum ofminimum , so `ans[L] = D`.  

the first step , how we could get gap ? mono stack. 

given example [4, 3, 2, 7, 5]

firstly we put -1 to the stack, because we need calculate the gap, so this would give us some convinience. 

loop to 4, we get stack `[-1, 0], 4 index is 0`, the comes `3`, so we pop `0`, record its value `4`, gap is `1-(-1)-2 =
0`, and we got `ans[0] = 4`, then push `1` to stack, so we got `[-1, 1]`, why we need `x-2`, because as the above pic
shows, for point A, we need to exclude 2 point B and C, so we need `-2`

continue, we come to 2, pop 1 from stack, record value 3, then got gap `2-(-1)-2 = 1`, so `ans[1]=3`, push `2(index)` to stack, we got [-1,2] 
we come to 7, stack top `nums[2]=2 < 7`, so directly push index 3 to stack, `[-1, 2, 3]`

then comes 5, 7 > 5, so pop 3, record value 7, `gap = 4-2-2=0`, `ans[0] = max(ans[0], 7) = 7`, then we push 4 to stack, now stack is `[-1, 2, 4]`  

so we got a monostack now. but it is not completed yet, we still need got the gap of remaining elements.
pop 4, record value 5, `gap = len(nums)-stack.top-2 = 5-2-2=1`, `ans[1]=max(ans[1], 5) = 5`, notice now `ans[1]` update to `5`
pop 2, record value 2, `gap = 5-(-1)-2=4`, so `ans[4] = 2`, now stack has only -1 left, we stopped there. 

so we finally got : `ans : [7, 5, x, x, 2]`, so we reverse update this array, got [7, 5, 2, 2, 2]

time complexity: O(N)

this solution is pretty awesome. 


# code
```python
def findMaximums(self, nums: List[int]) -> List[int]:
    l, st = len(nums), [-1]
    ans = [0] * l

    for i in range(l):
        while st[-1] != -1 and nums[st[-1]] > nums[i]:
            min_e = nums[st[-1]]
            st.pop()
            ans[i-st[-1]-2] = max(ans[i-st[-1]-2], min_e)
        st.append(i)

    while st[-1] != -1:
        min_e = nums[st[-1]]
        st.pop()
        ans[l - st[-1]-2] = max(ans[l-st[-1]-2], min_e)

    for i in range(l-2, -1, -1):
        if ans[i] < ans[i+1]:
            ans[i] = ans[i+1]

    return ans
```




       

