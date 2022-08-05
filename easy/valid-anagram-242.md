# Valid Anagram

Page on leetcode: https://leetcode.com/problems/valid-anagram/

## Problem Statement

Given two strings s and t, return true if t is an anagram of s, and false otherwise. An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once

### Constraints

- 1 <= s.length, t.length <= 5 \* 10<sup>4</sup>
- s and t consist of lowercase English letters.

### Example

```
Input: s = "anagram", t = "nagaram"
Output: true
```

```
Input: s = "rat", t = "car"
Output: false
```

## Solution

### Pseudocode

1. Check that strings are equal length
2. Sort both strings
3. Check that strings are equal

### Initial Solution

This solution has a time complexity of O(n<sup>3</sup>)

```javascript
const isAnagram = function (s, t) {
  if (s.length !== t.length) {
    return false;
  }
  sSorted = s.split('').sort().join('');
  tSorted = t.split('').sort().join('');
  return sSorted === tSorted;
};
```

### Optimized Solution

This solution has a time complexity of O(n). Discussion of the below solution can be seen here: https://leetcode.com/problems/valid-anagram/discuss/66527/A-few-JavaScript-solutions You can also see a video explanation of this approach here: https://www.youtube.com/watch?v=9UtInBqnCgA

```javascript
const isAnagram = function (s, t) {
  if (s.length !== t.length) {
    return false;
  }
  const charCounts = {};
  for (let char of s) {
    // Add character to map or increment count by 1
    charCounts[char] = (charCounts[char] || 0) + 1;
  }
  for (let char of t) {
    // If character doesn't exist or is already at zero return false
    if (!charCounts[char]) {
      return false;
    }
    charCounts[char]--;
  }
  return true;
};
```
