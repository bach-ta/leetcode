# 2140. Solving Questions With Brainpower (Medium)

Backward DP - O(n) - AC

`dp[i]` = max points we can get for the questions i..n-1
- Base case: `dp[n-1] = last_point`
- Recurrence: nx = next possible question index
    - `dp[i] = max(dp[i+1], cur_point + dp[nx])` if nx exists
    - `dp[i] = max(dp[i+1], cur_point)` if nx does not exist (current question blocks all the following questions)

```python
def mostPoints(self, qs: List[List[int]]) -> int:
    n = len(qs)
    dp = [0] * n
    dp[n-1] = qs[n-1][0]

    for i in range(n-2, -1, -1):
        nx = i + qs[i][1] + 1
        dp[i] = max(dp[i+1], qs[i][0] + (dp[nx] if nx <= n-1 else 0))

    return dp[0]
```
