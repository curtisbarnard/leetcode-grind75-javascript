# Product of Array Except Self

Page on leetcode: https://leetcode.com/problems/product-of-array-except-self/

## Problem Statement

Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

### Constraints

- 2 <= nums.length <= 105
- -30 <= nums[i] <= 30
- The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

### Example

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

## Solution

- Calculate the total product and store in variable
- Iterate thru array a second time and subtrack product of current element
- Can I store product left and right?
- Will flipping or doing something in reverse work?

### Initial Solution

This solution has a time and space complexity of O(n).

```javascript
const productExceptSelf = function (nums) {
  // Calculate left and right products
  const left = [];
  for (let i = 0; i < nums.length; i++) {
    if (i > 0) {
      const product = nums[i - 1] * left[left.length - 1];
      left.push(product);
    } else {
      left.push(1);
    }
  }
  const right = [];
  for (let i = nums.length - 1; i >= 0; i--) {
    if (i < nums.length - 1) {
      const product = nums[i + 1] * right[0];
      right.unshift(product);
    } else {
      right.unshift(1);
    }
  }
  console.log(left, right);
  // Calculate result based on what is left and right of current number
  const answer = [];
  for (let i = 0; i < nums.length; i++) {
    answer.push(left[i] * right[i]);
  }
  return answer;
};
```

### Optimized Solution

This solution has O(n) time complexity and O(1) space complexity. You can see an explanation of this solution here: https://www.youtube.com/watch?v=bNvIQI2wAjk

```javascript
const productExceptSelf = function (nums) {
  // Create initial array and put initial value of 1
  const output = Array(nums.length).fill(1);

  // Iterate thru nums and add prefixes to output
  let prefix = 1;
  for (let i = 0; i < nums.length; i++) {
    output[i] = prefix;
    prefix *= nums[i];
  }

  // Iterate thru nums backwards and calculate total with pre and post fix.
  let postfix = 1;
  for (let i = nums.length - 1; i >= 0; i--) {
    output[i] *= postfix;
    postfix *= nums[i];
  }

  return output;
};
```
