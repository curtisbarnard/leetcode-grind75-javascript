# Contains Duplicate

Page on leetcode: https://leetcode.com/problems/contains-duplicate/

## Problem Statement

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

### Constraints

- 1 <= nums.length <= 105
- -109 <= nums[i] <= 109

### Example

```
Input: nums = [1,2,3,1]
Output: true
```

```
Input: nums = [1,2,3,4]
Output: false
```

## Solution

Immediate thought is to use a hashmap and count the numbers as you iterate thru the array.

### Pseudocode

1. Create empty map
2. For loop thru array
3. If element exist in map return true
4. else add element to map with val 1
5. return false if it loops thru entire array

### Initial Solution

This solution has a time and space complexity of O(n).

```javascript
const containsDuplicate = function (nums) {
  const map = {};

  for (let i = 0; i < nums.length; i++) {
    if (nums[i] in map) {
      return true;
    }
    map[nums[i]] = 1;
  }

  return false;
};
```

### Alternative Solution

The below alternative has a time complexity O(nlogn) due to sorting, but has a space complexity of O(1). You can see explanations of different solutions here: https://www.youtube.com/watch?v=3OamzN90kPg

```javascript
const containsDuplicate = function (nums) {
  nums.sort((a, b) => a - b);

  let left = 0;
  let right = 1;
  while (nums[right] !== undefined) {
    if (nums[left] === nums[right]) {
      return true;
    }
    left++;
    right++;
  }

  return false;
};
```
