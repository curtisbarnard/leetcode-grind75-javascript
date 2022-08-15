# Majority Element

Page on leetcode: https://leetcode.com/problems/majority-element/

## Problem Statement

Given an array nums of size n, return the majority element. The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

Follow-up: Could you solve the problem in linear time and in O(1) space?

### Constraints

- n == nums.length
- 1 <= n <= 5 \* 10<sup>4</sup>
- -10<sup>9</sup> <= nums[i] <= 10<sup>9</sup>

### Example

```
Input: nums = [3,2,3]
Output: 3
```

```
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

## Solution

### Pseudocode

1. Create hashmap
2. Iterate through nums
3. Add num to hash map
4. If any num has count > n/2 return that num

### Initial Solution

This solution has a time complexity O(n) with a space complexity of O(n). Technically space complexity would be O(n/2) as at least half the elements need to be one number.

```javascript
const majorityElement = function (nums) {
  if (nums.length === 1) {
    return nums[0];
  }

  const threshold = nums.length / 2;
  const map = {};

  for (let num of nums) {
    if (num in map) {
      map[num]++;
      if (map[num] > threshold) {
        return num;
      }
    } else {
      map[num] = 1;
    }
  }
};
```

### Optimized Solution

This revised solution uses the [Boyer-Moore Voting Algorithm](https://www.geeksforgeeks.org/boyer-moore-majority-voting-algorithm/). Time complexity is O(n) as you have to check every element in the array, but space complexity is only O(1). You can see a good explanation here: https://www.youtube.com/watch?v=7pnhv842keE

```javascript
const majorityElement = function (nums) {
  let major = nums[0];
  let count = 1;

  for (let i = 1; i < nums.length; i++) {
    // increment/decrement the count based on if character matches current major character
    if (nums[i] === major) {
      count++;
    } else {
      count--;
    }

    // Set new major character if the count ever gets down to zero
    if (count === 0) {
      major = nums[i];
      count = 1;
    }
  }

  return major;
};
```
