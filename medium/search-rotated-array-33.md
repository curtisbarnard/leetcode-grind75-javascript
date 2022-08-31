# Search in Rotated Sorted Array

Page on leetcode: https://leetcode.com/problems/search-in-rotated-sorted-array/

## Problem Statement

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

### Constraints

- 1 <= nums.length <= 5000
- -104 <= nums[i] <= 104
- All values of nums are unique.
- nums is an ascending array that is possibly rotated.
- -104 <= target <= 104

### Example

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

## Solution

- return index of target
- -1 if no target found
- Must be O(logn)
- [15, 20, 23, 40, 78]
- [40, 78, 15, 20, 23]

### Pseudocode

1. Check if end is less than start, if so it has been rotated
2. If not rotated use binary search (find mid, check mid, iteratively)
3. If rotated check if target is less than end, if true then check in second part of array, if not check in first part
4. Check second part, move start to mid point, check if mid is less than target, if less then proceed with binary search
5. If greater than, move start to midpoint between 0-index and start

### Initial Attempt

```javascript
const search = function (nums, target) {
  let start = 0;
  let end = nums.length - 1;

  if (nums[end] > sums[start]) {
    return binarySearch(start, end);
  }

  // Find which part of rotated array the target is in
  if (target < nums[end]) {
    start = start + (end - start) / 2;
    return findSortedArray(start, end);
  } else {
    end = start + (end - start) / 2;
    findSortedArray(start, end);
  }

  function findSortedArray(l, r) {
    // Proceed with binary search
    if (l < target && r > target) {
      return binarySearch(l, r);
    }

    if (l > target && l !== 0) {
      l = l / 2;
    } else if (r < target && r !== nums.length - 1) {
      r = r / 2;
    }
  }

  // binary search helper function
  function binarySearch(l, r) {
    while (l < r) {
      // update mid each loop
      const mid = l + (r - l) / 2;
      if (nums[mid] > target) {
        r = mid - 1;
      } else if (nums[mid] < target) {
        l = mid + 1;
      } else {
        return mid;
      }
    }
    // target not found
    return -1;
  }
};
```

### Optimized Solution

This solution has a time complexity of O(logn) and a space complexity of O(1). You can see an explanation of the solution here: https://www.youtube.com/watch?v=U8XENwh8Oy8

```javascript
const search = function (nums, target) {
  let l = 0;
  let r = nums.length - 1;

  while (l <= r) {
    // update mid each loop
    const mid = Math.floor((l + r) / 2);

    // target found
    if (nums[mid] === target) {
      return mid;
    }

    // mid point is on the left sorted portion of the array
    if (nums[l] <= nums[mid]) {
      if (target > nums[mid] || target < nums[l]) {
        l = mid + 1;
      } else {
        r = mid - 1;
      }
    } else {
      // mid points is on the right sorted portion of the array
      if (target < nums[mid] || target > nums[r]) {
        r = mid - 1;
      } else {
        l = mid + 1;
      }
    }
  }

  // target not found
  return -1;
};
```
