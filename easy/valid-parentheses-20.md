# Valid Parentheses

Page on leetcode: https://leetcode.com/problems/valid-parentheses/

## Problem Statement

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

- Open brackets must be closed by the same type of brackets.
- Open brackets must be closed in the correct order.

### Constraints

- 1 <= s.length <= 104
- s consists of parentheses only '()[]{}'.

### Example

Input: s = "()[]{}"
Output: true

Input: s = "(]"
Output: false

Input: s = "(][)"
Output: false

Input: s = "([])"
Output: true

## Solution

### Pseudocode

1. create stack variable
2. iterate thru string
3. if open bracket add to stack
4. if closed bracket and matches open bracket in stack, pop from stack, else return false
5. after loop if stack length is greater than 0 return false
6. return true

### Initial Solution

```javascript
var isValid = function (s) {
  const stack = [];
  for (let i = 0; i < s.length; i++) {
    if (s[i] === '(' || s[i] === '{' || s[i] === '[') {
      stack.push(s[i]);
    } else if (s[i] === ')') {
      if (stack[stack.length - 1] === '(') {
        stack.pop();
      } else {
        return false;
      }
    } else if (s[i] === '}') {
      if (stack[stack.length - 1] === '{') {
        stack.pop();
      } else {
        return false;
      }
    } else if (s[i] === ']') {
      if (stack[stack.length - 1] === '[') {
        stack.pop();
      } else {
        return false;
      }
    }
  }
  if (stack.length > 0) {
    return false;
  }
  return true;
};
```

### Optimized Solution

This solutions is O(n) time complexity. See this video for more explanation: https://www.youtube.com/watch?v=WTzjTskDFMg

```javascript
const isValid = function (s) {
  // Checking for string lengths that are odd which wouldn't be valid
  if (s.length % 2 !== 0) {
    return false;
  }

  const stack = [];
  const brackets = {
    '(': ')',
    '{': '}',
    '[': ']',
  };

  for (const c of s) {
    if (brackets.hasOwnProperty(c)) {
      // add closing bracket to stack
      stack.push(brackets[c]);
    } else if (stack.pop() !== c) {
      // This will run if stack is empty and you try to add a closing bracket or closing brackets don't match
      return false;
    }
  }
  // If stack is empty returns true as all brackets matched up
  return stack.length === 0;
};
```
