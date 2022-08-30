# Rotting Oranges

Page on leetcode: https://leetcode.com/problems/rotting-oranges/

## Problem Statement

You are given an m x n grid where each cell can have one of three values:

- 0 representing an empty cell,
- 1 representing a fresh orange, or
- 2 representing a rotten orange.

Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.

### Constraints

- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 10
- grid[i][j] is 0, 1, or 2.

### Example

```
Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```

## Solution

- length of time is the depth from a 2 node to it's deepest 1 node
- traverse BFS and increment depth counter if you find a 2
- Need to find the minimum amount of time for all 1's to chang to 2's then find the max of that
- What happens if I encounter another 2 on the traversal?
- Possibly wipe from top left to bottom right and then in the reverse direction with DP?
- Max variable that compares after the second wipe?

### Initial Pseudocode

1. iterate thru i, j
2. if i, j is 2, check right and bottom
3. if a value is > 0 and not 2, set to 3, check right and bottom
4. if a value is > 0 and not 2, increment parent + 1, check right and bottom
5. iterate thru i,j towards top left
6. if i, j is 2, check top, left
7. if a value is > 0 and not 2, set to 3, check against max, check top, left
8. if a value is > 0 and not 2, increment parent + 1, check against max, check top, left
9. return max

### Optimized Solution

The below approach is with BFS and has a time and space complexity of O(mn). You can see an explanation of the approach here: https://www.youtube.com/watch?v=y704fEOx0s0

```javascript
const orangesRotting = function (grid) {
  const queue = [];
  let time = 0,
    fresh = 0;
  const ht = grid.length;
  const wd = grid[0].length;

  // Iterate thru matrix and find all fresh and rotten oranges
  for (let i = 0; i < ht; i++) {
    for (let j = 0; j < wd; j++) {
      if (grid[i][j] === 2) {
        queue.push([i, j]);
      } else if (grid[i][j] === 1) {
        fresh++;
      }
    }
  }

  // Directions for checking around an orange in the below loop
  const dir = [
    [0, 1],
    [1, 0],
    [0, -1],
    [-1, 0],
  ];

  // Work thru rotten oranges in queue
  while (queue.length > 0 && fresh > 0) {
    let level = queue.length;

    // Work thru this level of oranges
    while (level > 0) {
      const [row, col] = queue.shift();

      // Check all four directions
      for (let d of dir) {
        const r = row + d[0];
        const c = col + d[1];
        const bounds = r >= 0 && r < ht && c >= 0 && c < wd;

        if (bounds && grid[r][c] === 1) {
          // If orange satisfies conditions add to queue, change to rotten and remove from fresh counter.
          queue.push([r, c]);
          grid[r][c] = 2;
          fresh--;
        }
      }

      level--;
    }
    time++;
  }

  // Check that we turned all fresh oranges rotten
  return fresh === 0 ? time : -1;
};
```
