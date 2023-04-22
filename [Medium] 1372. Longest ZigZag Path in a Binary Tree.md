# 1372. Longest ZigZag Path in a Binary Tree (Medium)

Use recursion. `lzz(root, right, soFar)` returns the longest zigzag path starting from `root`, with the first turn being right if `right` is `True`.

`soFar` is the length of the path so far (from the original root), assuming that the param `root` is not `None`. If root is `None`, then the path is `soFar - 1`, since we had counted an extra edge (from a leaf to `None`).

```python
def longestZigZag(self, root: Optional[TreeNode]) -> int:
    @lru_cache(None)
    def lzz(root: Optional[TreeNode], right: bool, soFar) -> int:
        if not root: return soFar - 1
        r = lzz(root.right, False, soFar + 1 if right else 1)
        l = lzz(root.left, True, soFar + 1 if not right else 1)
        return max(l, r)
    
    return lzz(root, False, 0)
```