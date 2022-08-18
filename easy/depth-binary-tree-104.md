# Maximum Depth of Binary Tree

Page on leetcode: https://leetcode.com/problems/maximum-depth-of-binary-tree/

## Problem Statement

Given the root of a binary tree, return its maximum depth. A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

### Constraints

- The number of nodes in the tree is in the range [0, 104].
- -100 <= Node.val <= 100

### Example

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

```
Input: root = [1,null,2]
Output: 2
```

## Solution

Either DFS or BFS for traversal, I would select DFS. Recursion seems like the simplest way to approach. Edge cases would be null root or entire left branch null.

### Initial Pseudocode

1. Create max variable
2. Create recursive function
3. if node is null return 0
4. Create a count variable equal to a recursive call
5. Set max equal to max(max, count)
6. return 1
7. Call recursive function
8. return max

### Initial Solution

This solution has a time and space complexity of O(n).

```javascript
const maxDepth = function (root) {
  if (!root) {
    return 0;
  }

  const depthLeft = maxDepth(root.left);
  const depthRight = maxDepth(root.right);
  return 1 + Math.max(depthLeft, depthRight);
};
```

### Optimized Solution

These alternative solutions have the same time and space complexity as above. You can see an explanation of the solutions here: https://www.youtube.com/watch?v=hTM3phVI6YQ

```javascript
// Breadth-First Search
const maxDepth = function (root) {
  if (!root) {
    return 0;
  }

  const queue = [root];
  let depth = 0;
  while (queue.length > 0) {
    const levelLength = queue.length;
    for (let i = 0; i < levelLength; i++) {
      const node = queue.shift();
      if (node?.left) {
        queue.push(node.left);
      }
      if (node?.right) {
        queue.push(node.right);
      }
    }
    depth++;
  }

  return depth;
};

// Iterative Depth-First Search
const maxDepth = function (root) {
  if (!root) {
    return 0;
  }

  const stack = [[root, 1]];
  let max = 0;
  while (stack.length > 0) {
    const [node, depth] = stack.pop();
    max = Math.max(max, depth);

    if (node?.left) {
      stack.push([node.left, depth + 1]);
    }
    if (node?.right) {
      stack.push([node.right, depth + 1]);
    }
  }

  return max;
};
```
