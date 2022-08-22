# Longest Substring Without Repeating Characters

Page on leetcode: https://leetcode.com/problems/longest-substring-without-repeating-characters/

## Problem Statement

Given a string s, find the length of the longest substring without repeating characters.

### Constraints

- 0 <= s.length <= 5 \* 104
- s consists of English letters, digits, symbols and spaces.

### Example

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Solution

- Want to try and accomplish O(n)
- Only have to return an integer
- Hash map of characters?
- Variable to store max?

### Pseudocode

1. Handle 0 characters condition
2. Create max variable with initial val of 0
3. Create a new map
4. Iterate thru string
5. If char exist in map, count entries in map, take max (max, entries count)
6. clear map
7. continue iterating
8. return max

### Initial Attempt

This attempt has a time and space complexity of O(n). This doesn't work when you want to use subsequent appearances of a char.

```javascript
const lengthOfLongestSubstring = function (s) {
  let max = 0;
  if (s.length > 0) {
    let map = new Map();
    for (let i = 0; i < s.length; i++) {
      if (map.has(s[i])) {
        let count = 0;
        map.forEach((char) => (count += 1));
        max = Math.max(max, count);
        map.clear();
      } else {
        map.set(s[i], 1);
      }
    }
    // Check count of map one last time
    let count = 0;
    map.forEach((char) => (count += 1));
    max = Math.max(max, count);
  }

  return max;
};
```

### Optimized Solution

This attempt has a time and space complexity of O(n). You can watch an explanation of the sliding window approach here: https://www.youtube.com/watch?v=wiGpQwVHdE0 Fo a discussion of the javascript solution you can go here: https://leetcode.com/problems/longest-substring-without-repeating-characters/discuss/731639/JavaScript-Clean-Heavily-Commented-Solution

```javascript
const lengthOfLongestSubstring = function (s) {
  const seen = new Map();
  let start = 0;
  let max = 0;

  for (let i = 0; i < s.length; i++) {
    if (seen.has(s[i])) {
      // Taking the max below prevents start from going back to a previous index from an earlier duplicate
      start = Math.max(seen.get(s[i]) + 1, start);
    }
    seen.set(s[i], i);
    max = Math.max(max, i - start + 1);
  }

  return max;
};
```
