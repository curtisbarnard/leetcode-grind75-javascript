# Valid Palindrome

Page on leetcode: https://leetcode.com/problems/valid-palindrome/

## Problem Statement

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers. Given a string s, return true if it is a palindrome, or false otherwise.

### Constraints

- 1 <= s.length <= 2 \* 10<sup>5</sup>
- s consists only of printable ASCII characters.

### Example

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

## Solution

### Pseudocode

1. String to lowercase
2. Remove non alphanumeric characters
3. Reverse string
4. Return conditional result of lowercaseString === reverseString

### Initial Solution

This solution has a time complexity O(n<sup>6</sup>) 😬

```javascript
const isPalindrome = function (s) {
  const regex = /[A-Za-z0-9]/;
  const cleanedArray = s
    .toLowerCase()
    .split('')
    .filter((char) => regex.test(char));
  const reversedString = [...cleanedArray].reverse().join('');
  return cleanedArray.join('') === reversedString;
};
```

### Optimized Solution

```javascript

```
