# 1019. Next Greater Node In Linked List (Medium)

Use a stack to keep track of the next larger node:
1. If the stack is empty, push the current node.
2. If the stack is not empty, pop the top node and compare it with the current node:
    - If the top node is smaller than the current node, then the current node is the next larger node for the top node.
    - If the top node is larger than the current node, then push the current node to the stack.

```python
def nextLargerNodes(self, head: Optional[ListNoteBook]) -> List[int]:
    stack = [] # (val, idx in ans)
    ans = []
    while head:
        while stack and stack[-1][0] < head.val:
            # top of stack has val < current val
            idx = stack.pop()[1]
            ans[idx] = head.val
        stack.append( (head.val, len(ans)) )
        ans.append(0) # next larger of current node
        head = head.next
    return ans
```
