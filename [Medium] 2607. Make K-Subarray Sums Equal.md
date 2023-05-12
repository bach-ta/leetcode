# 2607. Make K-Subarray Sums Equal (Medium)

Observation: If `a[i..i+k] = a[i+1..i+k+1]`, then `a[i] = a[i+k+1]`. So we can create groups of numbers such that in each group, 2 adjacent numbers are `k` steps away from each other. Then we can make all numbers in each group equal, and the problem is solved.

For the group starting at index `i`, since the array is circular, we can not simply add the numbers at indices `i`, `i+k`, `i+2k`,... until EOA to the group, since it might be that jumping another `k` steps lands us at an unvisited index (from the front of the list). So instead, we need to keep track of visited indices. Keep adding numbers at indices `i`, `(i+k)%n`, `(i+2k)%n`,... to the group until we reach an index that has been visited before. Then we start a new group. Also, we need to keep track of the number of visited elements, so that we know when to stop.

Note that DSU is not needed here, since once we are done with a group, it is guaranteed that we will never visit any of its elements again. This is intuitive.

Now for each group, we need to find the number of steps to make all elements in the group equal. It can be proved that (the middle element)/(anything between the middle two elements) is the optimal choice for the target value. For simplicity, we just use the middle element if the length of the group is odd, or the right middle element if the length of the group is even.

The final answer is the sum of the number of steps for each group.

https://leetcode.com/problems/make-k-subarray-sums-equal/submissions/949149916/
```python
def makeSubKSumEqual(self, arr: List[int], k: int) -> int:
    n = len(arr)
    visited = [False] * n
    i = 0
    cnt = 0 # number of visited elements
    groups = [] # list of groups
    cur_group = []

    while cnt < n:
        if visited[i]:
            groups.append(cur_group)
            cur_group = []
            i += 1
        visited[i] = True
        cur_group.append(arr[i])
        cnt += 1
        i = (i + k) % n
    groups.append(cur_group)
    
    def getSteps(g):
        g.sort()
        return sum([abs(x - g[len(g) // 2]) for x in g])
    
    return sum([getSteps(l) for l in groups])
```

Note that by using math, we can see that for each group, the step is actually the gcd of `n` and `k`, and we don't need to consider circularity at all. So the solution can be simplified to:

https://leetcode.com/problems/make-k-subarray-sums-equal/submissions/949227597/
```python
def makeSubKSumEqual(self, arr: List[int], k: int) -> int:
    n = len(arr)
    k = gcd(k, n)
    ans = 0
    
    for i in range(k):
        target = int(median(arr[i::k]))
        ans += sum([abs(num - target) for num in arr[i::k]])

    return ans
```
