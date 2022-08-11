# First Bad Version

Page on leetcode: https://leetcode.com/problems/first-bad-version/

## Problem Statement

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

### Constraints

- 1 <= bad <= n <= 231 - 1

### Example

```
Input: n = 5, bad = 4
Output: 4
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.
```

## Solution

### Pseudocode

1. Check midpoint of n
2. if true

- is n-1 false, if so return n
- if true recursive check left side

3. if false recursive check right side

- is n+1 true, if so return n
- if false recursive check right side

### Initial Solution

This solution has a time complexity O(logn) with a space complexity of O(logn) due to the call stack size.

```javascript
const solution = function (isBadVersion) {
  /**
   * @param {integer} n Total versions
   * @return {integer} The first bad version
   */
  return function (n) {
    function binary(l, r) {
      const mid = Math.floor((r + l) / 2);

      // Check left side
      if (isBadVersion(mid)) {
        if (!isBadVersion(mid - 1)) {
          return mid;
        }
        return binary(l, mid);
      }

      // Check right side
      if (!isBadVersion(mid)) {
        if (isBadVersion(mid + 1)) {
          return mid + 1;
        }
        return binary(mid, r);
      }
    }

    return binary(1, n);
  };
};
```

### Optimized Solution

The below is still a binary search O(logn) solution, but is cleaner and has a space complexity of O(1). You can see discussion of this solution here: https://leetcode.com/problems/first-bad-version/discuss/71344/JavaScript-solution-keysubtle-difference-from-other-languages

```javascript
const solution = function (isBadVersion) {
  /**
   * @param {integer} n Total versions
   * @return {integer} The first bad version
   */
  return function (n) {
    let start = 1;
    let end = n;

    while (start < end) {
      const mid = Math.floor(start + (end - start) / 2);
      if (isBadVersion(mid)) {
        end = mid;
      } else {
        start = mid + 1;
      }
    }
    return end;
  };
};
```
