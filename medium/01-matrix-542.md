# 01 Matrix

Page on leetcode: https://leetcode.com/problems/01-matrix/

## Problem Statement

Given an m x n binary matrix mat, return the distance of the nearest 0 for each cell.

The distance between two adjacent cells is 1.

### Constraints

- m == mat.length
- n == mat[i].length
- 1 <= m, n <= 104
- 1 <= m \* n <= 104
- mat[i][j] is either 0 or 1.
- There is at least one 0 in mat.

### Example

```
Input: mat = [[0,0,0],[0,1,0],[0,0,0]]
Output: [[0,0,0],[0,1,0],[0,0,0]]
```

```
Input: mat = [[0,0,0],[0,1,0],[1,1,1]]
Output: [[0,0,0],[0,1,0],[1,2,1]]
```

## Solution

- No diagonal travel allowed
- If cell is already 0 it stays 0?
- BFS search, if any surrounding are 0 cell stays 1
- flag to show that cell has been searched?
- If I could find the 0 and work outwards from there that would be ideal

### Initial Pseudocode

If you find a zero, set current value to 1, update other three directions to 2
If you don't find a zero, update current value + 1, update parent to curr + 1

### Optimized Solution

There are a couple of ways to approach the problem. One is with Breadth First Search and the other is with Dynamic Programming. You can see a discussion of the two approached here: https://leetcode.com/problems/01-matrix/discuss/1369741/C%2B%2BJavaPython-BFS-DP-solutions-with-Picture-Clean-and-Concise-O(1)-Space

- BFS has a time and space complexity of O(m\*n)
- Dynamic Programming has a time complexity of O(m\*n) and a space complexity of O(1)

```javascript
// Breadth-First Search
const updateMatrix = function (mat) {
  const height = mat.length;
  const width = mat[0].length;

  const queue = [];
  // Finding all the zeros in the matrix and adding to the queue, mark all other cells -1 to know they are unprocessed
  for (let i = 0; i < height; i++) {
    for (let j = 0; j < width; j++) {
      if (mat[i][j] === 0) {
        queue.push([i, j]);
      } else {
        mat[i][j] = -1;
      }
    }
  }
  // directions for checking around a cell
  const rowDir = [0, 1, 0, -1];
  const colDir = [1, 0, -1, 0];
  while (queue.length > 0) {
    const [row, column] = queue.shift();

    // Make sure cell is in bounds and hasn't been processed
    for (let k = 0; k < 4; k++) {
      const rowNext = row + rowDir[k];
      const colNext = column + colDir[k];
      if (
        rowNext < 0 ||
        rowNext > height - 1 ||
        colNext < 0 ||
        colNext > width - 1 ||
        mat[rowNext][colNext] !== -1
      ) {
        continue;
      }

      // Set current cell one larger than parent and then add to queue to processes it's children
      mat[rowNext][colNext] = mat[row][column] + 1;
      queue.push([rowNext, colNext]);
    }
  }
  return mat;
};

// Dynamic Programming
const updateMatrix = function (mat) {
  const ht = mat.length;
  const wd = mat[0].length;

  // Moving from top left to bottom right and updating cells to the minimum + 1 of the cells that are to the top left of them
  for (let i = 0; i < ht; i++) {
    for (let j = 0; j < wd; j++) {
      if (mat[i][j] > 0) {
        const top = i > 0 ? mat[i - 1][j] : Infinity;
        const left = j > 0 ? mat[i][j - 1] : Infinity;
        mat[i][j] = Math.min(top, left) + 1;
      }
    }
  }
  // Moving from bottom right to top left and updating cells to the minimum + 1 of the cells that are to the bottom right of them
  for (let i = ht - 1; i >= 0; i--) {
    for (let j = wd - 1; j >= 0; j--) {
      if (mat[i][j] > 0) {
        const btm = i < ht - 1 ? mat[i + 1][j] : Infinity;
        const right = j < wd - 1 ? mat[i][j + 1] : Infinity;
        mat[i][j] = Math.min(btm + 1, right + 1, mat[i][j]);
      }
    }
  }

  return mat;
};
```
