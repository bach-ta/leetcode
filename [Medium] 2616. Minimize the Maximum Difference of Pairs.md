# [2616. Minimize the Maximum Difference of Pairs (Medium)](https://leetcode.com/problems/minimize-the-maximum-difference-of-pairs/)

First, sort the array and create a new array of length `n-1`, where each element is the difference between two adjacent elements in the sorted array. Assign this array to the original array.

The problem can now be reduced to: Given an array `nums` and a number `p`, find the smallest number `ans` such that there exists `p` pair-wise non-adjacent numbers in `nums`, each of which is at most `ans`. Note that we need 2 numbers d1 and d2 to be non-adjacent since if they are adjacent, d1 corresponds to `b-a` and d2 corresponds to `c-b`, where `a < b < c`, so we have `b` appears in 2 pairs, which is not allowed.

Then, we can use binary search and greedy algorithm to solve this problem. For each `ans`, we check if `ans` is valid (there exists `p` pair-wise non-adjacent numbers in `nums`, each of which is at most `ans`) as follows:
1. Find all contiguous blocks of numbers in `nums` such that in each block, all numbers are at most `ans`. For example, if `nums = [5, 4, 9, 3, 2, 3, 4, 5, 6, 7]` and `ans = 4`, then the blocks are `[5, 4], [3, 2, 3], [4]`.
2. For each of these blocks, we take as many non-adjacent numbers as possible (greedy). Naturally, the # of numbers taken from each block is ceil(len(block) / 2).
3. After considering all the blocks, `ans` is valid if and only if the total number of numbers taken is at least `p`.

The above step can be easily implemented by converting nums to a binary string of length `len(nums)` where each digit is 1 if the corresponding number in `nums` is at most `ans`, and 0 otherwise. Then, each block is a contiguous subsequence of 1's.

Original submission:
```python
def minimizeMax(self, nums: List[int], p: int) -> int:
    nums.sort()
    nums = [nums[i] - nums[i-1] for i in range(1, len(nums))]
    if len(nums) == 0: return 0
    
    l, r = 0, max(nums)
    
    c = lambda a, b: (a - 1) // b + 1
    
    def f(ans):
        s = ''.join(map(str, [int(x <= ans) for x in nums]))
        return sum(c(len(seq), 2) for seq in s.split('0')) >= p
    
    while l < r:
        m = l + (r - l) // 2
        if f(m):
            r = m
        else:
            l = m+1
    return l
```
