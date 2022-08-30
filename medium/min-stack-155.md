# Min Stack

Page on leetcode: https://leetcode.com/problems/min-stack/

## Problem Statement

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the MinStack class:

- MinStack() initializes the stack object.
- void push(int val) pushes the element val onto the stack.
- void pop() removes the element on the top of the stack.
- int top() gets the top element of the stack.
- int getMin() retrieves the minimum element in the stack.

You must implement a solution with O(1) time complexity for each function.

### Constraints

- -2<sup>31</sup> <= val <= 2<sup>31</sup> - 1
- Methods pop, top and getMin operations will always be called on non-empty stacks.
- At most 3 \* 10<sup>4</sup> calls will be made to push, pop, top, and getMin

### Example

```
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

## Solution

- pop, push and peek (top) are constant with a normal stack
- minheap can read min val in constant time
- Do a stack with a linked list instead? How can I reassign min?

### Initial Pseudocode

1. Initialize array that will be a stack
2. Push: push to array, then sort?
3. Pop: pop off array, then sort?
4. Top: pop off return then push back on

### Optimized Solution

This solution use O(1) for all operations by utilizing two stacks. You can see an explanation of this approach here: https://www.youtube.com/watch?v=qkLl7nAwDPo

```javascript
const MinStack = function () {
  this.stack = [];
  this.minStack = [];
};

/**
 * @param {number} val
 * @return {void}
 */
MinStack.prototype.push = function (val) {
  this.stack.push(val);
  const minStackSize = this.minStack.length;
  // update minStack with current min value
  if (minStackSize === 0 || this.minStack[minStackSize - 1] > val) {
    this.minStack.push(val);
  } else {
    this.minStack.push(this.minStack[minStackSize - 1]);
  }
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function () {
  this.stack.pop();
  this.minStack.pop();
};

/**
 * @return {number}
 */
MinStack.prototype.top = function () {
  return this.stack[this.stack.length - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function () {
  return this.minStack[this.minStack.length - 1];
};
```
