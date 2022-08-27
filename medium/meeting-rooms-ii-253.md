# Meeting Rooms II

Page on leetcode: https://leetcode.com/problems/meeting-rooms-ii/

## Problem Statement

Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), find the minimum number of conference rooms required.)

### Constraints

### Example

```
Input: intervals = [(0,30),(5,10),(15,20)]
Output: 2
Explanation:
We need two meeting rooms
room1: (0,30)
room2: (5,10),(15,20)
```

## Solution

### Optimized Solution

This problem can be approached using sorting and two pointers or using a heap. The below is with a heap which has a time complexity of O(nlogn) and space complexity of O(n). You can see an explanation of this approach here: https://www.youtube.com/watch?v=qx7Akat3xrM

```javascript
const minMeetingRooms(intervals) {
  // Sort meetings by start times
  intervals.sort((a,b) => a[0] - b[0])

  // Create initial heap
  const usedRooms = [intervals[0][1]]

  // iterate thru meetings and compare start times to heap
  for (let i = 1; i < intervals.length; i++){
    // If current meeting has no overlap with the minheap meeting, then remove the minheap meeting. We won't need an additional meeting room.
    if (intervals[i][0] >= usedRooms[0]) {
      heapifyDown()
    }
    // Add current meeting to the heap
    heapifyUp(intervals[i][1])
  }

  return usedRooms.length

  // Heapify Up helper
  function heapifyUp(endTime) {

  }

  // Heapify Down helper
  function heapifyDown(endTime) {

  }

}
```
