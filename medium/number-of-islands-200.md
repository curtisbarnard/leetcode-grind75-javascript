# Number of Islands

Page on leetcode: https://leetcode.com/problems/number-of-islands/

## Problem Statement

Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

### Constraints

- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 300
- grid[i][j] is '0' or '1'.

### Example

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

## Solution

- Only 1's that are horizontal/vertical connected count, no diagonals
- Need to store nodes that have been visited, maybe a set
- DFS or BFS search in four directions

### Pseudocode

1. Create set
2. Create counter for islands
3. Iterate thru matrix nest i j loops
4. If cell is 1 and isn't in set call dfs function
5. number of islands++
6. create dfs recursive helper
7. if cell is 1 and in bounds and not in set
8. then add to set
9. recursive calls on all four directions
10. return num of islands

### Initial Solution

This solution has a time and space complexity O(mn).

```javascript
const numIslands = function (grid) {
  const h = grid.length;
  const w = grid[0].length;

  let totalIslands = 0;

  // Iterate through the matrix
  for (let i = 0; i < h; i++) {
    for (let j = 0; j < w; j++) {
      if (grid[i][j] === '1') {
        dfs(i, j);
        totalIslands++;
      }
    }
  }
  return totalIslands;

  // DFS helper function
  function dfs(i, j) {
    // Check for in bounds
    const bounds = i >= 0 && i < h && j >= 0 && j < w;

    // If valid cell add to set and start recursion
    if (bounds && grid[i][j] === '1') {
      grid[i][j] = '2';
      dfs(i + 1, j);
      dfs(i, j + 1);
      dfs(i - 1, j);
      dfs(i, j - 1);
    }
  }
};
```

### Optimized Solution

The following solution has the same time and space complexity, but is approach with iterative BFS instead of recursive DFS. You can see and explanation here: https://www.youtube.com/watch?v=pV2kpPD66nE

```javascript
const numIslands = function (grid) {
  const h = grid.length;
  const w = grid[0].length;
  const visited = new Set();
  let totalIslands = 0;

  // Iterate through the matrix
  for (let i = 0; i < h; i++) {
    for (let j = 0; j < w; j++) {
      if (grid[i][j] === '1' && !visited.has(`${i}, ${j}`)) {
        bfs(i, j);
        totalIslands++;
      }
    }
  }

  return totalIslands;

  // BFS helper function
  function bfs(i, j) {
    const queue = [];
    visited.add(`${i}, ${j}`);
    queue.push([i, j]);

    while (queue.length > 0) {
      const [i, j] = queue.shift();
      const dir = [
        [0, 1],
        [1, 0],
        [0, -1],
        [-1, 0],
      ];
      for (let d of dir) {
        const row = i + d[0];
        const col = j + d[1];
        // Check for in bounds
        const bounds = row >= 0 && row < h && col >= 0 && col < w;
        if (bounds && !visited.has(`${row}, ${col}`) && grid[row][col] === '1') {
          visited.add(`${row}, ${col}`);
          queue.push([row, col]);
        }
      }
    }
  }
};
```
