# Binary Tree Level Order Traversal

Page on leetcode: https://leetcode.com/problems/binary-tree-level-order-traversal/

## Problem Statement

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

### Constraints

- The number of nodes in the tree is in the range [0, 2000].
- -1000 <= Node.val <= 1000

### Example

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

## Solution

- BFS with a queue
- Only need to return the values
- Need to track the level so that once a level is done a new array is created
- Create a result array

### Pseudocode

1. Create a result array
2. Create a queue
3. Add root to queue with level 1
4. Loop while queue is not empty
5. Loop on level
6. Dequeue and push value to level array
7. If node.left is valid push to queue, same with right with level as curlevel + 1
8. Once level is done push level array to result array
9. Increment level
10. Return result

### Initial Solution

This solution has a time and space complexity O(n).

```javascript
const levelOrder = function (root) {
  const result = [];
  // Handle if root is null
  if (root) {
    const queue = [[root, 1]];
    let level = 1;
    while (queue.length > 0) {
      const levelArr = [];
      // Looping on current level
      while (queue.length > 0 && level === queue[0][1]) {
        const node = queue.shift();
        levelArr.push(node[0].val);
        if (node[0].left) {
          queue.push([node[0].left, node[1] + 1]);
        }
        if (node[0].right) {
          queue.push([node[0].right, node[1] + 1]);
        }
      }
      // Add everything from current level to result
      result.push(levelArr);
      level++;
    }
  }
  return result;
};
```

### Optimized Solution

This solution has a time and space complexity O(n). It's pretty much the same as above just using a cleaner method to track each level. You can see an explanation of the approach here: https://www.youtube.com/watch?v=6ZnyEApgFYg

```javascript
const levelOrder = function (root) {
  const result = [];
  // Handle if root is null
  if (root) {
    const queue = [root];
    while (queue.length > 0) {
      const qLen = queue.length;
      const levelArr = [];
      // Looping on current level
      for (let i = 0; i < qLen; i++) {
        const node = queue.shift();
        // queue will have null nodes. If nodes are null we just skip them.
        if (node) {
          levelArr.push(node.val);
          queue.push(node.left);
          queue.push(node.right);
        }
      }
      // Add everything from current level to result is it is non empty
      if (levelArr.length > 0) {
        result.push(levelArr);
      }
    }
  }
  return result;
};
```
