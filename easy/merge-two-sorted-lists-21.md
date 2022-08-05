# Merge Two Sorted List

Page on leetcode: https://leetcode.com/problems/merge-two-sorted-lists/

## Problem Statement

You are given the heads of two sorted linked lists list1 and list2. Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists. Return the head of the merged linked list.

### Constraints

- The number of nodes in both lists is in the range [0, 50].
- -100 <= Node.val <= 100
- Both list1 and list2 are sorted in non-decreasing order.

### Example

Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]

Input: list1 = [-1,2,-4], list2 = [1,3,4]
Output: [-4,-1,1,2,3,4,]

Input: list1 = [], list2 = []
Output: []

Input: list1 = [1], list2 = []
Output: [1]

## Solution

### Initial Attempt

Solution below did not work as the linked list is not iterable. See optimized solution below.

```javascript
for (let i of list1) {
  if (list1[i] > list2[0]) {
    for (let j of list2) {
      if (list1[i] < list2[j]) {
        list2[j - 1].next = list1[i];
        list1.splice(i, 0, list2.splice(0, j));
        break;
      }
    }
  } else {
    if (list1[i].next > list2[0]) {
      for (let j of list2) {
        if (list1[i].next < list2[j]) {
          list2[j - 1].next = list1[i].next;
          list1.splice(i, 0, list2.splice(0, j));
          list1[i].next = list2[0];
          break;
        }
      }
    }
  }
}

return list1;
```

### Pseudocode

The following pseudocode is for the optimized solution.

```javascript
// initialize a dummy head node
// initialize a tail variable to track current tail, starting with the dummy head node
// while there are still nodes to compare in two lists
// if value of 1st node is less than value of 2nd node
// set the current node's link to list1 node
// set the list1 node to list1's next node
// else
// set the current node's link to list2 node
// set the list2 node to lsit2's next node
// move on to the next node
// if one of the lists no longer have any nodes to compare, point tails link to the remaining nodes in the other list
// return merged linked list
```

### Optimized Solution

https://www.youtube.com/watch?v=XIdigk956u0

```javascript
const mergeTwoLists = function (list1, list2) {
  let dummyNode = new ListNode(0);
  let tail = dummyNode;
  while (list1 && list2) {
    if (list1.val < list2.val) {
      tail.next = list1;
      list1 = list1.next;
    } else {
      tail.next = list2;
      list2 = list2.next;
    }
    tail = tail.next;
  }

  tail.next = list1 == null ? list2 : list1;
  return dummyNode.next;
};
```
