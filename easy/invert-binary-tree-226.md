# Invert Binary Tree

Page on leetcode: https://leetcode.com/problems/invert-binary-tree/

## Problem Statement

Given the root of a binary tree, invert the tree, and return its root.

### Constraints

- The number of nodes in the tree is in the range [0, 100].
- -100 <= Node.val <= 100

### Example

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

```
Input: root = [2,1,3]
Output: [2,3,1]
```

```
Input: root = []
Output: []
```

## Solution

### Pseudocode

1. Flip left and right
2. Recursively call function
3. If at last node return root

### Initial Attempt

This below is not a working solution. It was my first attempt at finding the solution.

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function (root) {
  if (!root.val) {
    return root;
  } else {
    const leftHolder = root.right;
    root.right = root.left;
    root.left = leftHolder;
    console.log(root.val);
    return invertTree(root.left);
  }
};
```

### Optimized Solution

There are three ways to approach the solution: recursion, Depth First Search and Breadth First Search. Time complexity of these solutions is O(n) as you will visit every node once. Discussion of these solutions can be found here: https://leetcode.com/problems/invert-binary-tree/discuss/399221/Clean-JavaScript-iterative-DFS-BFS-solutions

A video explaining the recursive solution can be seen here: https://www.youtube.com/watch?v=OnSn2XEQ4MY

To see an explanation of the difference between DFS and BFS see these videos: https://www.youtube.com/watch?v=pcKY4hjDrxk
https://youtu.be/tWVWeAqZ0WU?t=430

```javascript
// Recursion
var invertTree = function (root) {
  if (root === null) {
    return null;
  }
  [root.left, root.right] = [invertTree(root.right), invertTree(root.left)];
  return root;
};

// Iterative Depth First Search
function invertTree(root) {
  const stack = [root];

  while (stack.length) {
    const node = stack.pop();
    if (node !== null) {
      [node.left, node.right] = [node.right, node.left];
      stack.push(node.left, node.right);
    }
  }
  return root;
}

// Iterative Breadth First Search
function invertTree(root) {
  const queue = [root];

  while (queue.length) {
    const node = queue.shift();
    if (node !== null) {
      [node.left, node.right] = [node.right, node.left];
      queue.push(node.left, node.right);
    }
  }
  return root;
}
```
