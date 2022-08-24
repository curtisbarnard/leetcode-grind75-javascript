# Merge Intervals

Page on leetcode: https://leetcode.com/problems/merge-intervals/

## Problem Statement

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

### Constraints

- 1 <= intervals.length <= 104
- intervals[i].length == 2
- 0 <= starti <= endi <= 104

### Example

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

## Solution

- Is intervals always sorted? We will assume yes for now

### Pseudocode

1. Create empty array result
2. Create curInterval and set to intervals[0]
3. Iterate thru intervals
4. ith interval [0] is less than or equal currInterval[1], set curr[0] to min of ith and curr and set curr[1] to max of ith and curr.
5. Else push curr to result
6. Set curr equal to ith
7. Push curr to result
8. Return result

### Initial Solution

This solution has a time complexity of O(nlogn) due to sorting and a space complexity of O(n) due returning a result array. You can see an explanation of the approach to this problem here: https://www.youtube.com/watch?v=44H3cEC2fFM

```javascript
const merge = function (intervals) {
  // In order for below to work need a sorted array. Sort lowest to highest by the 0-index of the interval
  intervals.sort((a, b) => a[0] - b[0]);

  const result = [];
  let curr = intervals[0];

  for (let i = 1; i < intervals.length; i++) {
    if (intervals[i][0] <= curr[1]) {
      // If there is overlap, update current interval to span the overlapping intervals
      curr[0] = Math.min(curr[0], intervals[i][0]);
      curr[1] = Math.max(curr[1], intervals[i][1]);
    } else {
      // If no overlap then add current interval to the result and update current interval to the just checked interval
      result.push(curr);
      curr = intervals[i];
    }
  }
  result.push(curr);

  return result;
};
```
