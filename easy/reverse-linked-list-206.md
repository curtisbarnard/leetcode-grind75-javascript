# Reverse Linked List

Page on leetcode: https://leetcode.com/problems/reverse-linked-list/

## Problem Statement

Given the head of a singly linked list, reverse the list, and return the reversed list.

Follow up: A linked list can be reversed either iteratively or recursively. Could you implement both?

### Constraints

- The number of nodes in the list is the range [0, 5000].
- -5000 <= Node.val <= 5000

### Example

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

```
Input: head = [1,2]
Output: [2,1]
```

## Solution

### Pseudocode

1. While loop, start at head
2. store current node in array, go to next node
3. When next is null exit
4. for loop, start at tail
5. set next equal to i - 1
6. return head

Didn't end up using above approach because it didn't work.

### Initial Solution

This solution is iterative and has a time complexity of O(n) and space complexity of O(1).

```javascript
const reverseList = function (head) {
  let temp = null;
  while (head) {
    let previous = head;
    head = head.next;
    previous.next = temp;
    temp = previous;
  }
  return temp;
};
```

### Optimized Solution

The below solution is a recursive approach and is less ideal than the solution above. Time complexity is the same a O(n), however the space complexity is O(n) which is larger than above. This is due to the increasing call stack size as you recurse. You can watch explanations of both approaches here: https://www.youtube.com/watch?v=G0_I-ZF0S38

```javascript
const reverseList = function (head) {
  if (!head) {
    return null;
  }
  newHead = head;
  if (head.next) {
    newHead = reverseList(head.next);
    head.next.next = head;
  }
  head.next = null;
  return newHead;
};
```
