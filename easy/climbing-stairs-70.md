# Climbing Stairs

Page on leetcode: https://leetcode.com/problems/climbing-stairs/

## Problem Statement

You are climbing a staircase. It takes n steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

### Constraints

- 1 <= n <= 45

### Example

```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

## Solution

### Pseudocode

1. Not even sure where to start

### Optimized Solution

The optimal Dynamic Programming solution has a time complexity of O(n) and space complexity O(1). You can see an in-depth explanation here: https://www.youtube.com/watch?v=Y0lT9Fck7qI

A less optimal approach uses recursion and memoization, which is also explain in the above video. Time complexity is O(n) and space complexity is O(n). You can see discussion of this approach here: https://leetcode.com/problems/climbing-stairs/discuss/1531764/Python-%3ADetail-explanation-(3-solutions-easy-to-difficult)-%3A-Recursion-dictionary-and-DP

```javascript
// Bottom-up Dynamic Programming
const climbStairs = function (n) {
  let oneStep = 1,
    twoStep = 1;
  for (let i = n; i > 0; i--) {
    const temp = oneStep;
    oneStep += twoStep;
    twoStep = temp;
  }
  return oneStep;
};

// Recursion and Memoization
const climbStairs = function (n) {
  const map = new Map();
  map.set(1, 1);
  map.set(2, 2);

  function dfs(n) {
    if (map.has(n)) {
      return map.get(n);
    } else {
      map.set(n, dfs(n - 1) + dfs(n - 2));
      return map.get(n);
    }
  }
  return dfs(n);
};
```
