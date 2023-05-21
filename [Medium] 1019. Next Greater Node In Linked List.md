# 1019. Next Greater Node In Linked List (Medium)

Use a stack to keep track of the next larger node. At any time, the stack contains nodes (stored as (val, idx)) that are waiting for the next larger node.
For each node:
1. Pop all nodes in the stack that are smaller than the current node, and update the answer for those nodes to be the current node's value.
2. Push the current node into the stack.

One thing to prove is that at the start of each iteration, the stack is sorted in non-ascending order from bottom to top (i.e. the top of the stack is always the smallest node in the stack).

TOP
1
2
2
4
7
BOTTOM

Suppose for contradiction that there exists a node `n` that is greater than the node `m` below it in the stack. Then `m` must have been popped (and `ans[m]` was set to `n`'s value) before `n` was pushed into the stack. This is a contradiction.

This result explains why in order to pop all nodes in the stack that are smaller than the current node, we only need to stop the while loop when the top of the stack is greater than the current node, instead of going all the way to the bottom of the stack.

Time complexity: We have a while loop for each node, and each node is pushed and popped from the stack at most once. So the time complexity is O(n) (even though the while loop is nested inside the while loop, the inner while loop is not executed for each node).

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
        ans.append(0) # increase the size of ans to make room for (the ans of) the current node. the value doesn't matter. it will be overwritten later.
        head = head.next
    return ans
```
