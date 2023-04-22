# 5. Longest Palindromic Substring (Medium)

Use dynamic programming. `dp[i][j]` is `True` if `s[i:j+1]` is a palindrome. `dp[i][j]` is `True` if `dp[i+1][j-1]` is `True` and `s[i] == s[j]`. The base case is `dp[i][i] = True`.

Note. If `j < i`, then `dp[i][j]` should be undefined, and we should avoid accessing it by checking `j >= i` in the recursive formula. However, to simplify the code, we can set `dp[i][j] = True` if `j < i`.

```python
def longestPalindrome(self, s: str) -> str:
    dp = [[True] * len(s) for _ in range(len(s))]
    l, r, maxLen = 0, 0, 1
    for j in range(0, len(s)):
        for i in range(j, -1, -1):
            if i == j:
                dp[i][j] = True
                continue
            dp[i][j] = dp[i+1][j-1] and s[i] == s[j]
            if dp[i][j] and j - i + 1 > maxLen:
                maxLen = j - i + 1
                l, r = i, j
    return s[l:r+1]
```
