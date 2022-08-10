# Balanced Binary Tree

Page on leetcode: https://leetcode.com/problems/balanced-binary-tree/

## Problem Statement

Given a binary tree, determine if it is height-balanced. For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

### Constraints

- The number of nodes in the tree is in the range [0, 5000].
- -104 <= Node.val <= 104

### Example

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

## Solution

### Initial Pseudocode

1. Get depth of left
2. Get depth of right
3. If depth of absolute left-right > 1, return false

### Initial Attempt

```javascript
const isBalanced = function (root) {
  let balanced = true;
  if (!root || root.left === null || root.right === null) {
    return balanced;
  }
  depth(root, 0, 0);
  return balanced;

  function depth(root, countL, countR) {
    console.log(countL, countR);
    if (Math.abs(countL - countR) > 2) {
      balanced = false;
    }
    if (root.left === null) {
      return;
    }
    depth(root.left, countL + 1, countR);
    depth(root.right, countL, countR + 1);
  }
};
```

### Optimized Solution

Time complexity for the below solution is O(n) and space complexity if the tree is balanced is O(logn) if it is unbalanced worst case is O(n). You can see an explanation of this solution here: https://www.youtube.com/watch?v=QfJsau0ItOY

```javascript
const isBalanced = function (root) {
  function dfs(root) {
    if (!root) return [true, 0];
    const [left, right] = [dfs(root.left), dfs(root.right)];
    const balanced = left[0] && right[0] && Math.abs(left[1] - right[1]) <= 1;
    return [balanced, 1 + Math.max(left[1], right[1])];
  }

  return dfs(root)[0];
};
```
