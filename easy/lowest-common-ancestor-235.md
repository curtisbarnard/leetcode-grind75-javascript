# Lowest Common Ancestor of a Binary Search Tree

Page on leetcode: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/

## Problem Statement

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

### Constraints

- The number of nodes in the tree is in the range [2, 105].
- -109 <= Node.val <= 109
- All Node.val are unique.
- p != q
- p and q will exist in the BST.

### Example

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

## Solution

### Initial Pseudocode

1. Traverse tree to find p and q, DFS
2. If p or q is found, save lineage
3. Compare lineages find number that is the same

### Initial Attempt

```javascript
const lowestCommonAncestor = function (root, p, q) {
  const pLineage = [];
  const qLineage = [];
  const currLineage = [];
  const stack = [root];

  while (stack.length > 0) {
    const node = stack.pop();
    if (node?.val === undefined || node.val === null) {
      currLineage.splice(0, currLineage.length);
      continue;
    }

    currLineage.push(node.val);

    if (node.val === p) {
      pLineage.concat(currLineage);
    } else if (node.val === q) {
      qLineage.concat(currLineage);
    }
    stack.push(node.left, node.right);
  }
  const intersection = pLineage.filter((node) => qLineage.includes(node));
  return intersection[intersection.length - 1];
};
```

### Optimized Solution

The below solution has a time complexity of O(logn) because with each iteration of the loop we are eliminating half of the descendent nodes from the search. Space complexity is O(1). You can watch an explanation of the solution here: https://www.youtube.com/watch?v=gs2LMfuOR9k

```javascript
const lowestCommonAncestor = function (root, p, q) {
  while (root) {
    if (p.val > root.val && q.val > root.val) {
      root = root.right;
    } else if (p.val < root.val && q.val < root.val) {
      root = root.left;
    } else {
      return root;
    }
  }
};
```
