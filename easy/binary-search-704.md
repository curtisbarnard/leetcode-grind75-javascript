# Binary Search

Page on leetcode:https://leetcode.com/problems/binary-search/

## Problem Statement

Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1. You must write an algorithm with O(log n) runtime complexity.

### Constraints

- 1 <= nums.length <= 104
- -104 < nums[i], target < 104
- All the integers in nums are unique.
- nums is sorted in ascending order.

### Example

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```

```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```

## Solution

### Pseudocode

1. Check if the number a position nums/2 rounded down is equal to the target
2. If greater than target check position numsLeft/2, if not recurse
3. If less than target check position numsRight/2, if not recurse
4. Return target index if found, else return -1

### Initial Attempt

This solution has a time complexity O(logn), however it doesn't find the position of the target in the original array.

```javascript
const search = function (nums, target) {
  let index = Math.floor(nums.length / 2);

  if (nums[index] === target) {
    return index;
  } else if (nums.length === 1) {
    return -1;
  } else if (nums[index] > target) {
    return search(nums.slice(0, index), target);
  } else if (nums[index] < target) {
    return search(nums.slice(index), target);
  }
};
```

### Optimized Solution

This solution has time complexity of O(logn) and space complexity of O(1). You can see a video explanation here: https://www.youtube.com/watch?v=s4DPM8ct1pI

```javascript
const search = function (nums, target) {
  let left = 0,
    right = nums.length - 1;
  while (left <= right) {
    const middle = Math.floor((left + right) / 2);
    if (nums[middle] > target) {
      right = middle - 1;
    } else if (nums[middle] < target) {
      left = middle + 1;
    } else {
      return middle;
    }
  }
  return -1;
};
```
