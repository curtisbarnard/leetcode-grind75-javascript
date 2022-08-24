# Number of Provinces

Page on leetcode: https://leetcode.com/problems/number-of-provinces/

## Problem Statement

There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

### Constraints

- 1 <= n <= 200
- n == isConnected.length
- n == isConnected[i].length
- isConnected[i][j] is 1 or 0.
- isConnected[i][i] == 1
- isConnected[i][j] == isConnected[j][i]

### Example

```
Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2
```

## Solution

### Initial Thoughts

- iterate thru arrays
- Have a counter, when you find a break add to counter
- minimum province is 1
- Can I check from i = 0 to i = n/2

### Optimized Solution

Time and space complexity for this solution would be O(n). This is a recursive DFS solution however it can also be solved with BFS and using a Union Find.
You can see a good solution to this problem here: https://www.youtube.com/watch?v=S5UUvCTM0V4

```javascript
const findCircleNum = function (isConnected) {
  const visited = new Set();
  let numOfProv = 0;

  // Iterate thru matrix
  for (let i = 0; i < isConnected.length; i++) {
    if (!visited.has(i)) {
      dfs(i);
      numOfProv++;
    }
  }

  return numOfProv;

  // Helper function for DFS search
  function dfs(i) {
    for (let j = 0; j < isConnected.length; j++) {
      if (isConnected[i][j] === 1 && !visited.has(j)) {
        visited.add(j);
        dfs(j);
      }
    }
  }
};
```
