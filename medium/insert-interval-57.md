# Insert Interval

Page on leetcode: https://leetcode.com/problems/insert-interval/

## Problem Statement

You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

### Constraints

- 0 <= intervals.length <= 104
- intervals[i].length == 2
- 0 <= starti <= endi <= 105
- intervals is sorted by starti in ascending order.
- newInterval.length == 2
- 0 <= start <= end <= 105

### Example

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

## Solution

- Need to take care of null case (no intervals provided)
- Need to find either interval before (if no overlap) or interval with overlap of start interval
- Check for overlap of end and subsequent intervals
- Remove intervals that get merged
- If new interval is entirely contained in single existing interval, return original intervals
- Can I use two pointers?

### Pseudocode

1. Create two pointers left, right
2. Iterate thru the array
3. Check if left pointer[1] is greater than start
4. right pointer = left point + 1
5. Check if right pointer[0] is less than end, if not merge with previous
6. Check if right pointer[1] is greater than end, if true merge

### Initial Attempt

```javascript
const insert = function (intervals, newInterval) {
  // No existing intervals
  if (intervals.length === 0) {
    intervals.push(newInterval);
    return intervals;
  }

  let l = 0,
    r = 0;
  while (r < intervals.length) {
    r++;
    // New interval completely contained within a single interval
    if (intervals[l][0] <= newInterval[0] && intervals[l][1] >= newInterval[1]) {
      return intervals;
    }
    // New interval start overlaps with existing interval
    if (intervals[l][1] >= newInterval[0]) {
      r--;
      while (r < intervals.length) {
        r++;
        // new interval start overlaps on start, but not end
        if (intervals[r] === undefined || intervals[r][0] > newInterval[1]) {
          const start = intervals[l][0];
          const end = newInterval[1];
          intervals.splice(l, r - l, [start, end]);
          return intervals;
          // new interval spans and overlaps 2+ intervals, including start
        } else if (intervals[r][0] <= newInterval[1] && intervals[r][1] > newInterval[1]) {
          const start = intervals[l][0];
          const end = intervals[r][1];
          intervals.splice(l, r - l + 1, [start, end]);
          return intervals;
        }
        r++;
      }
      // No overlaps
    } else if (intervals[l][1] < newInterval[0] && intervals[r][0] > newInterval[1]) {
      intervals.splice(l + 1, 0, newInterval);
      return intervals;
    }
    // New interval end overlaps with existing interval
    if (intervals[r][0] <= newInterval[1] && intervals[r][1] > newInterval[1]) {
      const start = newInterval[0];
      const end = intervals[r][1];
      intervals.splice(r, 1, [start, end]);
      return intervals;
    }
    l++;
  }
};
```

### Optimized Solution

This solution has a time and space complexity of O(n). You can see an explanation of the solution here: https://www.youtube.com/watch?v=A8NUOmlwOlM

```javascript
const insert = function (intervals, newInterval) {
  let result = [];
  for (let i = 0; i < intervals.length; i++) {
    // Check if new interval is before element without overlap
    if (intervals[i][0] > newInterval[1]) {
      result.push(newInterval);
      return result.concat(intervals.slice(i));
      // Check if new interval is after element without overlap
    } else if (intervals[i][1] < newInterval[0]) {
      result.push(intervals[i]);
      // Update new interval based on min and max values of overlapping intervals
    } else {
      newInterval[0] = Math.min(intervals[i][0], newInterval[0]);
      newInterval[1] = Math.max(intervals[i][1], newInterval[1]);
    }
  }
  // Need to add new interval to result if the above return never fired. This accounts for cases where new interval is at the end or overlaps with the end element.
  result.push(newInterval);
  return result;
};
```
