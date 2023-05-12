# [2657. Find the Prefix Common Array of Two Arrays (Medium)](https://leetcode.com/problems/find-the-prefix-common-array-of-two-arrays/)
https://leetcode.com/problems/find-the-prefix-common-array-of-two-arrays/submissions/945710097/

- Decrement all elements in both arrays by 1, so that the elements are in the range [0, n-1]. Each array is now just a permutation of its index set.
- Use an array `I` to store the index of number in `A`. For example, if `I[x] = y`, then `A[y] = x`.

```python
def findThePrefixCommonArray(self, A: List[int], B: List[int]) -> List[int]:
    n = len(A)
    ans = [0] * n
    I = [0] * n

    for i in range(n):
        A[i] -= 1
        B[i] -= 1
        I[A[i]] = i

    for idx_in_B in range(n):
        idx_in_A = I[B[idx_in_B]]
        begin = max(idx_in_A, idx_in_B)
        # the number B[idx_in_B] contributes 1 to all elements in ans[begin:]
        for j in range(begin, n):
            ans[j] += 1
    return ans
```
