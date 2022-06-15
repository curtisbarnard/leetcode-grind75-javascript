# Convert Binary Number in a Linked List to Integer

Page on leetcode: https://leetcode.com/problems/convert-binary-number-in-a-linked-list-to-integer/

## Problem Statement

Given head which is a reference node to a singly-linked list. The value of each node in the linked list is either 0 or 1. The linked list holds the binary representation of a number. Return the decimal value of the number in the linked list.

### Constraints

- The Linked List is not empty.
- Number of nodes will not exceed 30.
- Each node's value is either 0 or 1.

### Example

Input: head = [1,0,1]
Output: 5
Explanation: (101) in base 2 = (5) in base 10

## Solution

### Pseudocode

1. Create array to store linked list values
2. while list isn't null
3. list.val push to array
4. list = list.next
5. Join array
6. convert string to integer with parseInt

### Initial Solution

This solution has a time complexity O(n<sup>3</sup>)

```javascript
const getDecimalValue = function (head) {
  const binary = [];
  while (head) {
    binary.push(head.val);
    head = head.next;
  }
  const binaryString = binary.join('');
  return parseInt(binaryString, 2);
};
```

### Optimized Solution

This solution has a reduced time complexity of O(n). You can see an explanation at this [leetcode discussion](https://leetcode.com/problems/convert-binary-number-in-a-linked-list-to-integer/discuss/629087/Detailed-explanation-Java-%3A-faster-than-100.00)

```javascript
const getDecimalValue = function (head) {
  let sum = 0;
  while (head) {
    sum *= 2;
    sum += head.val;
    head = head.next;
  }
  return sum;
};
```
