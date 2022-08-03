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

The optimized solution has a time complexity of O(n).You can watch an explanation of the solution at: https://youtu.be/jJXJ16kPFWg as well as see the discussion of the Javascript solution at: https://leetcode.com/problems/valid-palindrome/discuss/40238/128ms-100-JavaScript-solution-super-easy-to-understand

```javascript
const isPalindrome = function (s) {
  // Initial pointer values
  let left = 0;
  let right = s.length - 1;

  while (left < right) {
    let leftCode = s.charCodeAt(left);
    let rightCode = s.charCodeAt(right);

    // Passing over non-AlphaNum characters
    if (!checkAlphaNum(leftCode)) {
      left++;
      continue;
    }
    if (!checkAlphaNum(rightCode)) {
      right--;
      continue;
    }

    // If both characters are alphanumeric check that they are the same as lowercase
    if (s[left].toLowerCase() !== s[right].toLowerCase()) {
      return false;
    }

    left++;
    right--;
  }
  return true;

  // Helper function for checking is a character is alphanumeric
  function checkAlphaNum(charCode) {
    if (97 <= charCode && charCode <= 122) {
      return true;
    } else if (65 <= charCode && charCode <= 90) {
      return true;
    } else if (48 <= charCode && charCode <= 57) {
      return true;
    } else {
      return false;
    }
  }
};
```
