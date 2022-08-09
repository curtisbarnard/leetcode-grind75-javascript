# Maximum Subarray

Page on leetcode: https://leetcode.com/problems/maximum-subarray/

## Problem Statement

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum. A subarray is a contiguous part of an array.

### Constraints

- 1 <= nums.length <= 105
- -104 <= nums[i] <= 104

### Example

```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

```
Input: nums = [1]
Output: 1
```

```
Input: nums = [5,4,-1,7,8]
Output: 23
```

## Solution

### Initial Pseudocode

1. Set maxSum = 0
2. Loop thru array
3. Set left pointer equal to index 0
4. If left pointer value is less than 0 go to next index
5. Add right pointer
6. If sum is less than 0 move left pointer equal to right pointer
7. If not less than zero keep summing and update maxSum
8. return maxSum

### Initial Attempt

This attempt would've ended up being O(n<sup>2</sup>) since it has nested for loops.

```javascript
const maxSubArray = function (nums) {
  let maxSum = nums[0];
  for (let left = 0; left < nums.length; left++) {
    let sum = nums[left];
    maxSum = Math.max(maxSum, sum);
    if (nums[left] < 0) {
      continue;
    }
    for (let right = left + 1; right < nums.length; right++) {
      sum += nums[right];
      if (sum < 0) {
        continue;
      }
      maxSum = Math.max(maxSum, sum);
    }
  }
  return maxSum;
};
```

### Optimized Solution

This solution has a time complexity if O(n) and a space complexity of O(1). The first approach below is dynamic programming using Kadane's algorithm. You can watch an explanation of the approach here: https://www.youtube.com/watch?v=jnoVtCKECmQ

An alternative way to solve this problem is with divide and conquer which has a time complexity of O(nlogn) and space complexity of O(1). You can see an explanation of the approach here: https://www.geeksforgeeks.org/maximum-subarray-sum-using-divide-and-conquer-algorithm/ Note that I was not able to get that solution working. There is some issue in the maxCrossingSum function

```javascript
// Kadane's Algorithm
const maxSubArray = function (nums) {
  let prev = 0;
  let max = nums[0];

  for (let i = 0; i < nums.length; i++) {
    prev = Math.max(prev + nums[i], nums[i]);
    max = Math.max(prev, max);
  }
  return max;
};
```
