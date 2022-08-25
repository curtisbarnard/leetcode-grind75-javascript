# Evaluate Reverse Polish Notation

Page on leetcode: https://leetcode.com/problems/evaluate-reverse-polish-notation/

## Problem Statement

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, \*, and /. Each operand may be an integer or another expression.

Note that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

### Constraints

- 1 <= tokens.length <= 104
- tokens[i] is either an operator: "+", "-", "\*", or "/", or an integer in the range [-200, 200].

### Example

```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

## Solution

- It works from inside out with nest operations
- Possibly add numbers to a stack?

### Pseudocode

1. Create empty stack
2. Iterate thru array
3. If element is number add to stack
4. If element is operator, pop last two from stack, do operation, add result to stack
5. Return remaining single element in stack

### Initial Solution

This solution has a time and space complexity of O(n).

```javascript
const evalRPN = function (tokens) {
  const stack = [];
  const operators = ['+', '-', '*', '/'];

  for (let i = 0; i < tokens.length; i++) {
    // If the element is an operator
    if (operators.includes(tokens[i])) {
      // Grab last two numbers from stack
      const last = +stack.pop();
      const first = +stack.pop();

      let result;
      // Do math depending on the operator
      if (tokens[i] === '+') {
        result = first + last;
      } else if (tokens[i] === '-') {
        result = first - last;
      } else if (tokens[i] === '*') {
        result = first * last;
      } else if (tokens[i] === '/') {
        result = first / last;
        // Truncate towards zero
        if (result > 0) {
          result = Math.floor(result);
        } else {
          result = Math.ceil(result);
        }
      }
      // Put result back into stack
      stack.push(result);
    } else {
      stack.push(tokens[i]);
    }
  }

  return stack[0];
};
```

### Optimized Solution

The below has the same time and space complexity, but has been cleaned up to run a little more efficient. You can see a good explanation of the solution using a stack here: https://www.youtube.com/watch?v=iu0082c4HDE

```javascript
const evalRPN = function (tokens) {
  const stack = [];

  for (el of tokens) {
    // Do math depending on the operator
    if (el === '+') {
      const last = stack.pop();
      const first = stack.pop();
      stack.push(first + last);
    } else if (el === '-') {
      const last = stack.pop();
      const first = stack.pop();
      stack.push(first - last);
    } else if (el === '*') {
      const last = stack.pop();
      const first = stack.pop();
      stack.push(first * last);
    } else if (el === '/') {
      const last = stack.pop();
      const first = stack.pop();
      const result = first / last;
      // Truncate towards zero
      stack.push(Math.trunc(result));
    } else {
      // If element isn't an operator then just add to stack
      stack.push(+el);
    }
  }

  return stack[0];
};
```
