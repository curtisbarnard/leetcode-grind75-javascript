# Add Binary

Page on leetcode: https://leetcode.com/problems/add-binary/

## Problem Statement

Given two binary strings a and b, return their sum as a binary string.

### Constraints

- 1 <= a.length, b.length <= 104
- a and b consist only of '0' or '1' characters.
- Each string does not contain leading zeros except for the zero itself.

### Example

```
Input: a = "11", b = "1"
Output: "100"
```

```
Input: a = "1010", b = "1011"
Output: "10101"
```

## Solution

### Pseudocode

1. Convert to decimal
2. Add numbers
3. Use toString method base 2 to convert to binary and return

### Initial Attempt

This solution has a time complexity O(a+b) however the solution doesn't work due to overflow.

```javascript
const addBinary = function (a, b) {
  const aDec = parseInt(a, 2);
  const bDec = parseInt(b, 2);
  const sum = aDec + bDec;
  return sum.toString(2);
};
```

### Optimized Solution

This solution has a time complexity of O(n) where n is the larger string and a space complexity of O(n). You can see an explanation of this approach here: https://www.youtube.com/watch?v=keuWJ47xG8g

```javascript
const addBinary = function (a, b) {
  let result = '',
    carry = 0,
    aPoint = a.length - 1,
    bPoint = b.length - 1;

  for (let i = 0; i < Math.max(a.length, b.length); i++) {
    let sum = carry;

    // Make sure pointer is inbounds
    const aVal = a[aPoint] ? +a[aPoint] : 0;
    const bVal = b[bPoint] ? +b[bPoint] : 0;

    // Update sum and carry based on division and modulo
    sum += aVal + bVal;
    carry = Math.floor(sum / 2);
    sum = sum % 2;

    // Add to sum to front of result string
    result = String(sum) + result;

    // Move to the next character left in each string
    aPoint--;
    bPoint--;
  }

  // If any leftover carry add a 1 to beginning of result
  if (carry) {
    result = '1' + result;
  }

  return result;
};
```
