# [2615. Sum of Distances (Medium)](https://leetcode.com/problems/sum-of-distances/)

Consider the following problem:

Given a sorted array of integers `nums`. Return an array of integers `ans` where `ans[i]` is the sum of the absolute differences between `nums[i]` and all the other elements in the array.

- Let `n` be the length of `nums`. 
- We can solve this problem in `O(n)` time and `O(n)` space by using a prefix sum array and a suffix sum array (`d(istance)l(eft)` and `dr` respectively in the code below). 
- `dl[i]`/`dr[i]` is the sum of the absolute differences between `nums[i]` and all the elements to the left/right of `nums[i]`. Then, `ans[i] = dl[i] + dr[i]`.
- We will show how to calculate `dr` in `O(n)` time. The calculation of `dl` is similar.
- First, `dr[0] = sum([nums[i] - nums[0] for i in range(n)])`. Note that we never need to use `abs` since `nums` is sorted.
- For `dr[i]` (`i >= 1`), we can calculate it from `dr[i-1]` in `O(1)` time: `dr[i] = dr[i-1] - (nums[i] - nums[i-1]) * (n - i)`. Intuitively, if we shift the pointer to the right by one from number `a` to `b`, then the "right sum" will decrease by the difference between `a` and `b` times the number of elements to the right of `a` (which includes `b`).


The original problem is basically `m` different instances of the above problem, where `m` is the number of unique elements in `nums`. We can solve each instance in `O(n)` time and `O(n)` space, thus the overall time complexity is `O(mn)`.

Original submission (the last line is a naive solution that runs in `O(mn^2)` time):

```python
def distance(self, nums: List[int]) -> List[int]:
    d = collections.defaultdict(list)
    for i, n in enumerate(nums):
        d[n].append(i)
    ans = [0] * len(nums)
    
    def assign(val):
        arr = d[val]
        if len(arr) == 1:
            ans[arr[0]] = 0
            return
        
        dr = [0] * len(arr)
        dr[0] = sum([arr[i] - arr[0] for i in range(len(arr))])
        for i in range(1, len(arr)):
            dr[i] = dr[i-1] - (arr[i] - arr[i-1]) * (len(arr) - i)
        
        dl = [0] * len(arr)
        dl[len(arr) - 1] = sum([arr[len(arr) - 1] - arr[i] for i in range(len(arr) - 1)])
        for i in range(len(arr) - 2, -1, -1):
            dl[i] = dl[i+1] - (arr[i+1] - arr[i]) * (i + 1)
            
        for (i, idx) in enumerate(arr):
            ans[idx] = dl[i] + dr[i]
        
    for val in d:
        assign(val)
        
    return ans
    # return [sum([abs(i - j) for j in d[nums[i]]]) for i in range(len(nums))]
```
