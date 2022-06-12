# Two Sum

Page on leetcode: https://leetcode.com/problems/two-sum/

## Problem Statement

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.
**Follow-up:** Can you come up with an algorithm that is less than O(n2) time complexity?

### Constraints

- 2 <= nums.length <= 104
- -109 <= nums[i] <= 109
- -109 <= target <= 109
- Only one valid answer exists.

### Example

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

## Solution

### Pseudocode

1. Shift first element from array store in var
2. loop thru array
3. check var + num[i] === target
4. If not successful shift next element and repeat

### Initial Solution

This solution has a time complexity O(n<sup>2</sup>)

```javascript
var twoSum = function (nums, target) {
  const initialLength = nums.length;
  for (let i = 0; i < initialLength; i++) {
    let testNum = nums.shift();
    for (let j = 0; j < nums.length; j++) {
      if (testNum + nums[j] === target) {
        return [i, i + j + 1];
      }
    }
  }
};
```

### Optimized Solution

Using a map (hash map) reduces the time complexity to O(n). Here is a good video with an explanation: https://www.youtube.com/watch?v=KLlXCFG5TnA

```javascript
var twoSum = function (nums, target) {
  const map = new Map();
  for (let i = 0; i < nums.length; i++) {
    const n = nums[i];
    if (map.has(target - n)) {
      return [map.get(target - n), i];
    } else {
      map.set(n, i);
    }
  }
  return [];
};
```
