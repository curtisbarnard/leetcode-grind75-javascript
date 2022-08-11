# Longest Palindrome

Page on leetcode: https://leetcode.com/problems/longest-palindrome/

## Problem Statement

Given a string s which consists of lowercase or uppercase letters, return the length of the longest palindrome that can be built with those letters. Letters are case sensitive, for example, "Aa" is not considered a palindrome here.

### Constraints

- 1 <= s.length <= 2000
- s consists of lowercase and/or uppercase English letters only.

### Example

```
Input: s = "abccccdd"
Output: 7
Explanation: One longest palindrome that can be built is "dccaccd", whose length is 7.
```

## Solution

### Pseudocode

- Can have a single character in the middle if odd length
- Other characters would need even amounts to be able to split in half

1. Add count of all characters to hash map
2. Create two arrays
3. iterate thru hash map
4. Divide char count by two, rounding down and push char to one array and unshift to the other. decrement count
5. If remainder count is one add to middle variable
6. join array plus middle plus other array
7. Return length of final array

### Initial Solution

This solution has a time complexity of O(n) and a space complexity of O(n).

```javascript
const longestPalindrome = function (s) {
  const chars = {};
  const leftSide = [];
  const rightSide = [];
  let middle = '';

  for (let char of s) {
    if (char in chars) {
      chars[char]++;
    } else {
      chars[char] = 1;
    }
  }

  for (char in chars) {
    if (chars[char] > 1) {
      const count = Math.floor(chars[char] / 2);
      leftSide.push(char.repeat(count));
      rightSide.unshift(char.repeat(count));
      chars[char] -= 2 * count;
    }
    if (chars[char] === 1) {
      middle = char;
    }
  }

  const finalString = leftSide.concat(middle, rightSide).join('');

  return finalString.length;
};
```

### Optimized Solution

The below refactored solution also has a time complexity of O(n) and a space complexity of O(n). You can see an explanation of the approach here: https://www.youtube.com/watch?v=a_3XDW9P47E

```javascript
const longestPalindrome = function (s) {
  const chars = {};
  let result = 0;

  for (let char of s) {
    if (char in chars) {
      chars[char]++;
    } else {
      chars[char] = 1;
    }
  }

  for (char in chars) {
    if (chars[char] > 1) {
      const count = Math.floor(chars[char] / 2) * 2;
      result += count;
    }
    if (result % 2 === 0 && chars[char] % 2 === 1) {
      result += 1;
    }
  }

  return result;
};
```
