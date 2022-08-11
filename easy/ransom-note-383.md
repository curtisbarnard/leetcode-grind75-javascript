# Ransom Note

Page on leetcode: https://leetcode.com/problems/ransom-note/

## Problem Statement

Given two strings ransomNote and magazine, return true if ransomNote can be constructed by using the letters from magazine and false otherwise. Each letter in magazine can only be used once in ransomNote.

### Constraints

- 1 <= ransomNote.length, magazine.length <= 105
- ransomNote and magazine consist of lowercase English letters.

### Example

```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```

```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```

## Solution

### Pseudocode

1. Add magazine characters to map
2. Decrement ransom note characters from map
3. If doesn't exist or goes below zero return false

### Initial Solution

This solution has a time complexity O(n+m) and space complexity O(n+m). You can see an explanation of this approach here: https://www.youtube.com/watch?v=bYzvVCfbwXM

```javascript
const canConstruct = function (ransomNote, magazine) {
  const map = {};
  for (let char of magazine) {
    if (map[char]) {
      map[char]++;
    } else {
      map[char] = 1;
    }
  }

  for (let char of ransomNote) {
    if (!map[char]) {
      return false;
    } else {
      map[char]--;
    }
  }

  return true;
};
```
