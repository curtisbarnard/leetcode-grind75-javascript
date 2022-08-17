# Middle of the Linked List

Page on leetcode:https://leetcode.com/problems/middle-of-the-linked-list/

## Problem Statement

Given the head of a singly linked list, return the middle node of the linked list. If there are two middle nodes, return the second middle node.

### Constraints

- The number of nodes in the list is in the range [1, 100].
- 1 <= Node.val <= 100

### Example

```
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.
```

## Solution

### Pseudocode

1. Find the length of the linked list.
2. Find the middle floor(length/2) + 1
3. Run a second loop and update head to middle
4. Return head

### Initial Solution

This solution has a time complexity of O(n) as we have to touch every node and a space complexity of O(1) as we only stored the mid point.

```javascript
const middleNode = function (head) {
  let newHead = head;
  let length = 0;
  while (newHead) {
    length++;
    newHead = newHead.next;
  }

  let midpoint = Math.floor(length / 2) + 1;

  while (midpoint > 1) {
    head = head.next;
    midpoint--;
  }

  return head;
};
```

### Optimized Solution

While the below solution has the same time and space complexity of above it is more efficient. You can see and explanation here: https://www.youtube.com/watch?v=wmpivqMlClI

```javascript
const middleNode = function (head) {
  let slow = head;
  let fast = head;

  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
  }

  return slow;
};
```
