# [31. Next Permutation](https://leetcode.com/problems/next-permutation/)



- Find the longest non-increasing `seq` at the end, e.g. `abc 43321`
- If `abc` is empty, then the permutation is the largest permutation -> reset to the first permutation by sorting back.
- If `abc` is not empty, then `c` needs to be updated to the min number among `seq` that is strictly greater than `c`. Swap that with `c`. For example:

```
nums:   5425433221
seq:       ^^^^^^^
swap:   5425433221
          ^   ^
        5435432221
newseq:    ^^^^^^^
```
Note that after swapping, the seq is still in non-increasing order. Sort this seq.

Note that since when we "sort" a sequence, since it is already in non-increasing order, we can just simply reverse the sequence.

```python
def nextPermutation(self, nums: List[int]) -> None:
    n = len(nums)
    i = n - 1
    while i > 0 and nums[i] <= nums[i-1]:
        i -= 1

    # i is now the start index of the non-increasing sequence. i-1 needs to be updated
    
    if i > 0:
        s = n - 1 # index of number to swap with i - 1
        while s >= 0 and nums[s] <= nums[i-1]:
            s -= 1
        nums[s], nums[i-1] = nums[i-1], nums[s]

    j = n - 1
    while i < j:
        nums[i], nums[j] = nums[j], nums[i]
        i += 1
        j -= 1
```
