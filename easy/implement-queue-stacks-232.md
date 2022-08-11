# Implement Queue using Stacks

Page on leetcode: https://leetcode.com/problems/implement-queue-using-stacks/

## Problem Statement

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (push, peek, pop, and empty).

Implement the MyQueue class:

- void push(int x) Pushes element x to the back of the queue.
- int pop() Removes the element from the front of the queue and returns it.
- int peek() Returns the element at the front of the queue.
- boolean empty() Returns true if the queue is empty, false otherwise.

Notes:

- You must use only standard operations of a stack, which means only push to top, peek/pop from top, size, and is empty operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

Follow-up: Can you implement the queue such that each operation is amortized O(1) time complexity? In other words, performing n operations will take overall O(n) time even if one of those operations may take longer.

### Constraints

- 1 <= x <= 9
- At most 100 calls will be made to push, pop, peek, and empty.
- All the calls to pop and peek are valid.

### Example

```
Input
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 1, 1, false]

Explanation
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

## Solution

### Pseudocode

1. Create two stacks
2. Push

- If both stacks empty then push to stack 1
- If stacks not empty and last op push, push to stack 1
- else move items from non-empty to empty stack, then push
- set last op to push

3. Peek

- if lastop push, Pop items from stack and push to empty stack
- return last element in stack 2
- set lastop to peek

4. Pop

- if lastop push, Pop items from stack 1 and push to stack 2
- Pop from stack 2 and return
- set lastop to pop

5. Empty

- Check length of stack 1
- Check length of stack 2
- Return true if both are 0

### Initial Solution

This solution has a time complexity O(n) and space complexity O(1).

```javascript
const MyQueue = function () {
  this.lastOp = '';
  this.stack1 = [];
  this.stack2 = [];
};

/**
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function (x) {
  if (
    (this.stack1.length === 0 && this.stack2.length === 0) ||
    this.lastOp === 'push'
  ) {
    this.stack1.push(x);
  } else {
    const stackSize = this.stack2.length;
    for (let i = 0; i < stackSize; i++) {
      this.stack1.push(this.stack2.pop());
    }
    this.stack1.push(x);
  }
  this.lastOp = 'push';
};

/**
 * @return {number}
 */
MyQueue.prototype.pop = function () {
  if (this.lastOp === 'push') {
    const stackSize = this.stack1.length;
    for (let i = 0; i < stackSize; i++) {
      this.stack2.push(this.stack1.pop());
    }
  }
  this.lastOp = 'pop';
  return this.stack2.pop();
};

/**
 * @return {number}
 */
MyQueue.prototype.peek = function () {
  if (this.lastOp === 'push') {
    const stackSize = this.stack1.length;
    for (let i = 0; i < stackSize; i++) {
      this.stack2.push(this.stack1.pop());
    }
  }
  this.lastOp = 'peek';
  return this.stack2[this.stack2.length - 1];
};

/**
 * @return {boolean}
 */
MyQueue.prototype.empty = function () {
  if (this.stack1.length === 0 && this.stack2.length === 0) {
    return true;
  }
  return false;
};
```

### Optimized Solution

The below solution has a time complexity of O(1) amortized and a space complexity O(n). You can see some discussion of this approach here: https://leetcode.com/problems/implement-queue-using-stacks/discuss/64206/Short-O(1)-amortized-C%2B%2B-Java-Ruby

```javascript
const MyQueue = function () {
  this.stack1 = [];
  this.stack2 = [];
};

/**
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function (x) {
  this.stack1.push(x);
};

/**
 * @return {number}
 */
MyQueue.prototype.pop = function () {
  if (!this.stack2.length) {
    while (this.stack1.length) {
      this.stack2.push(this.stack1.pop());
    }
  }
  return this.stack2.pop();
};

/**
 * @return {number}
 */
MyQueue.prototype.peek = function () {
  if (!this.stack2.length) {
    while (this.stack1.length) {
      this.stack2.push(this.stack1.pop());
    }
  }
  return this.stack2[this.stack2.length - 1];
};

/**
 * @return {boolean}
 */
MyQueue.prototype.empty = function () {
  return !this.stack1.length && !this.stack2.length;
};
```
