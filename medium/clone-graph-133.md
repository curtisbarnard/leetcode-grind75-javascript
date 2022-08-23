# Clone Graph

Page on leetcode: https://leetcode.com/problems/clone-graph/

## Problem Statement

Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

**Test case format:**

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with val == 1, the second node with val == 2, and so on. The graph is represented in the test case using an adjacency list.

An adjacency list is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with val = 1. You must return the copy of the given node as a reference to the cloned graph.

### Constraints

- The number of nodes in the graph is in the range [0, 100].
- 1 <= Node.val <= 100
- Node.val is unique for each node.
- There are no repeated edges and no self-loops in the graph.
- The Graph is connected and all nodes can be visited starting from the given node.

### Example

```
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```

## Solution

- Traverse all nodes
- Make copies of nodes (use spread syntax?)
- Return new node 1
- No nodes then return node?
- Single node still need to make a copy
- BFS with queue, will it get stuck in a loop?

### Initial Pseudocode

1. Make empty queue
2. Create copy of first node
3. Add copy to queue
4. While queue isn't empty
5. Shift from queue
6. Create copy of node and add to stack

### Optimized Solution

The below is a recursive "DFS" approach to solving the problem with a time and space complexity of O(n). You can see an explanation of this approach here: https://www.youtube.com/watch?v=mQeF6bN8hMk A discussion about using DFS and BFS can be viewed here: https://leetcode.com/problems/clone-graph/discuss/1793212/C%2B%2Bor-Detailed-Explanation-w-DFS-and-BFS-or-Commented-code-with-extra-Test-Case

```javascript
const cloneGraph = function (node) {
  // Empty map for tracking which nodes are copied
  const map = {};

  function dfs(node) {
    // Null case, covers the no-node case in the constraints
    if (!node) {
      return node;
    }
    // If the node does exist in the map, skip this block and return the already copied node
    if (!map[node.val]) {
      // If it's not in the map then make a new deep copy of the node
      map[node.val] = new Node(node.val);
      // Make copies of each of the neighbor nodes (recursively)
      map[node.val].neighbors = node.neighbors.map((n) => dfs(n));
    }
    // After neighbors are processed on the node return it
    return map[node.val];
  }

  return dfs(node);
};
```
