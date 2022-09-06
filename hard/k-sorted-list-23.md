# Merge k Sorted Lists

Page on leetcode: https://leetcode.com/problems/merge-k-sorted-lists/

## Problem Statement

You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

### Constraints

- k == lists.length
- 0 <= k <= 104
- 0 <= lists[i].length <= 500
- -104 <= lists[i][j] <= 104
- lists[i] is sorted in ascending order.
- The sum of lists[i].length will not exceed 104.

### Example

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

## Solution

- Need to handle no list
- Need to handle no nodes in a list
- Compare heads of each list and add to dummy list
- Do I need to use a stable sort?

### Pseudocode

1. Create a dummy node
2. Create a tail variable equal to the dummy node
3. Sort
4. Iterate thru the lists and add to tail
5. Set list added head to next
6. If null remove from array
7. If array length = 1 add remainder of list to tail
8. Sort heads again

### Initial Solution

This solution has a time complexity

```javascript
const mergeKLists = function (lists) {
  const dummyNode = new ListNode();
  let tail = dummyNode;

  while (lists.length > 1) {
    lists.sort((a, b) => a.val - b.val);
    let i = 0;
    while (i < lists.length && lists.length > 1) {
      tail.next = lists[i];
      lists[i] = lists[i].next;
      tail = tail.next;

      // Remove list from lists if all nodes added
      if (lists[i] === null) {
        lists.splice(i, 1);
        continue;
      }

      // Keep adding from list if it's val is less than next val
      if (i + 1 < lists.length && lists[i].val >= lists[i + 1].val) {
        i++;
      }
    }
  }

  tail.next = lists[0];

  return dummyNode.next;
};
```

### Optimized Solution

There are several approaches to this problem which can be seen on the leetcode problem page. You can see an explanation of the divide and conquer approach here: https://www.youtube.com/watch?v=q5a5OiGbT6Q

Time complexity is O(nlogk) and space complexity is O(1)

```javascript
const mergeKLists = function (lists) {
  // Handle edge cases
  if (lists.length === 0 || lists === null) {
    return null;
  }

  while (lists.length > 1) {
    const mergedList = [];

    for (let i = 0; i < lists.length; i += 2) {
      const list1 = lists[i];
      const list2 = i + 1 < lists.length ? lists[i + 1] : null;
      mergedList.push(mergeList(list1, list2));
    }
    lists = mergedList;
  }

  return lists[0];

  function mergeList(list1, list2) {
    const dummyNode = new ListNode();
    let tail = dummyNode;

    while (list1 && list2) {
      if (list1.val <= list2.val) {
        tail.next = list1;
        list1 = list1.next;
      } else {
        tail.next = list2;
        list2 = list2.next;
      }
      tail = tail.next;
    }

    tail.next = list1 ? list1 : list2;

    return dummyNode.next;
  }
};
```
