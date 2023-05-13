# [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

Check the last Approach: https://leetcode.com/problems/longest-repeating-character-replacement/editorial/.

This is hard and very unintuitive. Time complexity is O(n) and additional space complexity is O(1), since the alphabet size is fixed.

```python
def characterReplacement(self, s: str, k: int) -> int:
    n = len(s)
    start = ans = max_cnt = 0
    cnt = [0] * 26
    
    for end in range(n):
        cnt[ord(s[end]) - 65] += 1
        max_cnt = max(max_cnt, cnt[ord(s[end]) - 65])
        
        if end - start + 1 - max_cnt > k:
            cnt[ord(s[start]) - 65] -= 1
            start += 1
        ans = max(ans, end - start + 1)

    return ans
```

A more concise version (but more subtle):

```python
def characterReplacement(self, s: str, k: int) -> int:
    n = len(s)
    ans = max_cnt = 0
    cnt = [0] * 26
    
    for end in range(n):
        cnt[ord(s[end]) - 65] += 1
        max_cnt = max(max_cnt, cnt[ord(s[end]) - 65])
        
        if ans - max_cnt < k:
            ans += 1
        else:
            cnt[ord(s[end - ans]) - 65] -= 1

    return ans
```
