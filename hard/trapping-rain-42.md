# Trapping Rain Water

Page on leetcode: https://leetcode.com/problems/trapping-rain-water/

## Problem Statement

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

### Constraints

- n == height.length
- 1 <= n <= 2 \* 104
- 0 <= height[i] <= 105

### Example

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

### Optimized Solution

There are several ways to solve this problem which can be seen on the solution page for the problem on leetcode. You can see an explanation of the dynamic programming and two pointer solutions here. at: https://www.youtube.com/watch?v=ZI2z5pq0TqA

Dynamic Programming has a time and space complexity of O(n)
Two pointers has a time complexity of O(n) and a space complexity of O(1)

```javascript
// Dynamic Programming
const trap = function (height) {
  const maxLeft = Array(height.length).fill(0);
  const maxRight = Array(height.length).fill(0);
  const min = [];
  let total = 0;

  // Find all the max heights to the left of each position in height
  for (let i = 1; i < height.length; i++) {
    if (maxLeft[i - 1] >= height[i - 1]) {
      maxLeft[i] = maxLeft[i - 1];
    } else {
      maxLeft[i] = height[i - 1];
    }
  }

  // Find all the max heights to the right of each position in height
  for (let i = height.length - 2; i >= 0; i--) {
    if (maxRight[i + 1] >= height[i + 1]) {
      maxRight[i] = maxRight[i + 1];
    } else {
      maxRight[i] = height[i + 1];
    }
  }

  // Find the minimum of between max left and right for each position in height
  for (let i = 0; i < height.length; i++) {
    if (maxLeft[i] < maxRight[i]) {
      min.push(maxLeft[i]);
    } else {
      min.push(maxRight[i]);
    }
  }

  // Find the amount of rain water each position can hold
  for (let i = 0; i < height.length; i++) {
    if (min[i] - height[i] > 0) {
      total += min[i] - height[i];
    }
  }
  return total;
};

// Two Pointers
const trap = function (height) {
  let total = 0,
    maxL = height[0],
    maxR = height[height.length - 1],
    left = 0,
    right = height.length - 1;

  while (left < right) {
    if (maxL <= maxR) {
      left++;
      if (maxL - height[left] > 0) {
        total += maxL - height[left];
      }
      maxL = Math.max(maxL, height[left]);
    } else {
      right--;
      if (maxR - height[right] > 0) {
        total += maxR - height[right];
      }
      maxR = Math.max(maxR, height[right]);
    }
  }

  return total;
};
```
