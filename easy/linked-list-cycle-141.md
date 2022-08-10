# Linked List Cycle

Page on leetcode: https://leetcode.com/problems/linked-list-cycle/

## Problem Statement

Given head, the head of a linked list, determine if the linked list has a cycle in it. There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

### Constraints

- The number of the nodes in the list is in the range [0, 104].
- -105 <= Node.val <= 105
- pos is -1 or a valid index in the linked-list.

### Example

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

## Solution

### Initial Attempt

```javascript
var hasCycle = function (head) {
  if (!head) {
    return false;
  }

  const map = {};

  while (head.next) {
    if (!map[head.val]) {
      map[head.val] = head;
    } else if (map[head.val] === head) {
      return true;
    }
    head = head.next;
  }
  return false;
};
```

### Optimized Solution

The first approach below has a time complexity of O(n) and space complexity of O(1). You can read more about fast & slow pointers here: https://www.geeksforgeeks.org/how-does-floyds-slow-and-fast-pointers-approach-work/

The second approach is using a hash map with a time complexity of O(n) and space complexity O(n). This approach is less ideal than using the slow and fast pointers. You can see an explanation of the approach here: https://leetcode.com/problems/linked-list-cycle/discuss/1048428/Python-3-Solutions-Explained

```javascript
// Two pointers Fast & Slow
const hasCycle = function (head) {
  if (!head) {
    return false;
  }
  let fast = head;
  let slow = head;
  while (fast && slow) {
    fast = fast.next?.next;
    slow = slow.next;
    if (fast === slow) {
      return true;
    }
  }
  return false;
};

// Hash Map
const hasCycle = function (head) {
  const map = new Map();

  while (head && head.next) {
    if (!map.has(head)) {
      map.set(head, head.val);
    } else {
      return true;
    }
    head = head.next;
  }

  return false;
};
```
