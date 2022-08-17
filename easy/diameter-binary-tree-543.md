# Diameter of Binary Tree

Page on leetcode:https://leetcode.com/problems/diameter-of-binary-tree/

## Problem Statement

Given the root of a binary tree, return the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root. The length of a path between two nodes is represented by the number of edges between them.

### Constraints

- The number of nodes in the tree is in the range [1, 104].
- -100 <= Node.val <= 100

### Example

```
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
```

## Solution

Since we only need to return the length we can probably use a max variable. DFS seems like a decent approach.

### Pseudocode

1. Set max variable to 0
2. create dfs helper function
3. check if root is null, if true return 0
4. recursive check on left and right, add one for each call
5. set max to the max(max, left + right)
6. return max

### Initial Attempt

```javascript
const diameterOfBinaryTree = function (root) {
  let max = 0;

  function dfs(root) {
    if (!root) {
      return 0;
    }
    const left = 1 + dfs(root.left);
    const right = 1 + dfs(root.right);
    max = Math.max(max, left + right);
    console.log(left, right, root.val);
    return Math.max(left, right);
  }

  dfs(root);

  return max;
};
```

### Optimized Solution

The below solution has a time and space complexity of O(n). To see an approximate explanation of the approach see this video: https://www.youtube.com/watch?v=bkxqA8Rfv04

I made a slight tweak in the math where instead of returning a height of -1 for a null node I returned 0 and then on valid nodes I returned a height of 1 + max(left, right). You can see discussion of the solution here: https://leetcode.com/problems/diameter-of-binary-tree/discuss/101148/Intuitive-Javascript-Solution

```javascript
const diameterOfBinaryTree = function (root) {
  let max = 0;

  function dfs(root) {
    if (!root) {
      return 0;
    }
    const left = dfs(root.left);
    const right = dfs(root.right);
    max = Math.max(max, left + right);
    return 1 + Math.max(left, right);
  }

  dfs(root);

  return max;
};
```
