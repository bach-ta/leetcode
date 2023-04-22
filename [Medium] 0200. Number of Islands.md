# 200. Number of Islands (Medium)

Use BFS to traverse the grid. For each cell, if it's 1, then it's a land cell and NOT visited. If it's 0, then it's a water cell or visited.

If the cell is 1, increment counter by 1 and use BFS to traverse all the connected land cells. Mark each visited cell as 0.

```python
def numIslands(self, grid: List[List[str]]) -> int: 
    m, n = len(grid), len(grid[0])
    ans = 0

    def bfs(i, j):
        q = deque([(i, j)])
        while q:
            i, j = q.popleft()
            if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == '0':
                continue
            grid[i][j] = '0' # mark as visited
            q.append((i+1, j))
            q.append((i-1, j))
            q.append((i, j+1))
            q.append((i, j-1))

    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                ans += 1
                bfs(i, j)

    return ans
```
