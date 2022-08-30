# Validate Binary Search Tree

Page on leetcode: https://leetcode.com/problems/validate-binary-search-tree/

## Problem Statement

Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

### Constraints

- The number of nodes in the tree is in the range [1, 104].
- -231 <= Node.val <= 231 - 1

### Example

```
Input: root = [2,1,3]
Output: true
```

## Solution

- Probably use DFS to traverse the tree
- Might be able to exit early if we find any node that doesn't satisfy BST
- if null return
- if left and right val are null return true
- if left < val or left == null and right > val or right == null return true
- else return false

### Pseudocode

1. base case if root is null return true
2. l = dfs(left) and r = dfs(right)
3. if left and right are null return true
4. if left.val < val or left == null and right.val > val or right == null and l and r are true return true
5. else return false

### Initial Attempt

```javascript
const isValidBST = function (root) {
  if (!root) {
    return true;
  }
  const l = isValidBST(root.left);
  const r = isValidBST(root.right);
  const lVal = root.left ? root.left.val < root.val : true;
  const rVal = root.right ? root.right.val > root.val : true;

  if (lVal && rVal && l && r) {
    return true;
  } else {
    return false;
  }
};
```

### Optimized Solution

The time and space complexity for both solutions below is O(n). Using in order traversal is likely a better approach. You can see an explanation of this solution here: https://www.youtube.com/watch?v=s6ATEkipzow

```javascript
// Recursive DFS
const isValidBST = function (root) {
  function valid(root, left, right) {
    // Base case, null node is true
    if (!root) {
      return true;
    }
    // Recursive calls on left and right nodes passing update "boundaries"
    const lNode = valid(root.left, left, root.val);
    const rNode = valid(root.right, root.val, right);

    // Check that current node satisfies boundaries and child nodes are BST
    if (left < root.val && right > root.val && lNode && rNode) {
      return true;
    }

    return false;
  }

  return valid(root, -Infinity, Infinity);
};

// In order traversal
const isValidBST = function (root) {
  // Create the empty stack and set previous to null
  const stack = [];
  let prev = null;

  while (root !== null || stack.length > 0) {
    // Traverse down the left nodes as far as possible and add to stack
    while (root !== null) {
      stack.push(root);
      root = root.left;
    }

    root = stack.pop();
    // Checking if the previous value is larger than current, if it is then this is not a BST
    if (prev !== null && prev.val >= root.val) {
      return false;
    }
    // Update previous to the current root and then move root to the right
    prev = root;
    root = root.right;
  }
  // If we make it all the way here and didn't return false then it is a BST
  return true;
};
```
