# K Closest Points to Origin

Page on leetcode: https://leetcode.com/problems/k-closest-points-to-origin/

## Problem Statement

Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane and an integer k, return the k closest points to the origin (0, 0). The distance between two points on the X-Y plane is the Euclidean distance (i.e., √(x1 - x2)2 + (y1 - y2)2). You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

### Constraints

- 1 <= k <= points.length <= 104
- -104 < xi, yi < 104

### Example

```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].
```

## Solution

Any order so sorting isn't required. This is an optimization problem (minimizing distance to origin) so my mind goes to greedy methods. A heap might be a good data structure. I will need to look at all sets of points so the best conceivable time complexity would be O(n). I'm not sure if that is attainable though.

Edge case would be 1 point provided, with a k of 1. Or k = num of points.

### Pseudocode

1. If points length equals k then return points
2. Create helper function that returns euclidean distance
3. Heapify array to Min-Heap
4. Remove k smallest points from heap to array and return array

### Initial Attempt

```javascript
const kClosest = function (points, k) {
  if (points.length === k) {
    return points;
  }

  function dist(point) {
    return Math.sqrt(point[0] ** 2 + point[1] ** 2);
  }

  function heapify(arr) {
    for (let i = points.length - 1; i >= 0; i--) {
      const right = 2 * i + 2;
      const left = 2 * i + 1;
      if (arr[right]) {
        if (dist(arr[right]) < dist(arr[i])) {
          const temp = arr[i];
          arr[i] = arr[right];
          arr[right] = temp;
        }
      }
      if (arr[left]) {
        if (dist(arr[left]) < dist(arr[i])) {
          const temp = arr[i];
          arr[i] = arr[left];
          arr[left] = temp;
        }
      }
    }
    return arr;
  }

  function deleteFromHeap(arr) {
    const first = arr.shift();
    const last = arr.pop();
    arr.unshift(last);
    bubbleDown(arr, 0);
    return first;
  }

  function bubbleDown(arr, point) {
    if (!arr[point]) {
      return;
    }
    const right = 2 * i + 2;
    const left = 2 * i + 1;
    if (arr[left] < arr[right] && arr[left] < arr[point]) {
      arr[point] = arr[left];
      arr[left] = bubbleDown(arr, left);
    } else if (arr[right] < arr[left] && arr[right] < arr[point]) {
      arr[point] = arr[right];
      arr[right] = bubbleDown(arr, right);
    }
    return;
  }

  let result = [];
  for (let i = 0; i < k; i++) {
    result.push(deleteFromHeap(points));
  }
  return result;
};
```

### Second Attempt

I've used the max heap approach which has a time complexity of O(nlogk) and a space complexity of O(k).

```javascript
const kClosest = function (points, k) {
  if (points.length === k) {
    return points;
  }

  const maxHeap = [];

  // Add points to the heap
  for (let i = 0; i < points.length; i++) {
    if (maxHeap.length >= k && dist(points[i]) > dist(maxHeap[0])) {
      continue;
    }

    addToHeap(points[i]);
    if (maxHeap.length > k) {
      removeFromHeap();
    }
  }

  // Helper Functions
  function dist(point) {
    return point[0] ** 2 + point[1] ** 2;
  }

  function addToHeap(point) {
    maxHeap.push(point);
    heapifyUp(maxHeap, maxHeap.length - 1);
  }

  function heapifyUp(arr, node) {
    const parentNode = Math.floor((node - 1) / 2);
    if (parentNode < 0) {
      return;
    }
    if (dist(arr[node]) > dist(arr[parentNode])) {
      const temp = arr[parentNode];
      arr[parentNode] = arr[node];
      arr[node] = temp;
      heapifyUp(arr, parentNode);
    }
    return;
  }

  function removeFromHeap() {
    maxHeap.shift();
    maxHeap.unshift(maxHeap.pop());
    heapifyDown(maxHeap, 0);
  }

  function heapifyDown(arr, node) {
    const right = 2 * node + 2;
    const left = 2 * node + 1;
    if (right <= arr.length - 1) {
      if (dist(arr[right]) > dist(arr[node])) {
        const temp = arr[node];
        arr[node] = arr[right];
        arr[right] = temp;
        heapifyDown(arr, right);
      }
    }
    if (left <= arr.length - 1) {
      if (dist(arr[left]) > dist(arr[node])) {
        const temp = arr[node];
        arr[node] = arr[left];
        arr[left] = temp;
        heapifyDown(arr, left);
      }
    }
    return;
  }

  return maxHeap;
};
```

### Optimized Solution

There are a few different ways to solve this problem. Good examples and discussion can be seen here: https://leetcode.com/problems/k-closest-points-to-origin/discuss/762781/javascript-sort-minHeap-and-maxHeap-solutions

I would say the min heap approach is the most ideal.

```javascript
var kClosest = function (points, k) {
  // we can build the heap in place
  let p = Math.floor((points.length - 2) / 2); // last parent
  for (let i = p; i >= 0; i--) {
    heapifyDown(points, i, distance);
  }

  // now we need to remove the smallest (points[0]) k times
  let solution = [];
  for (let i = 0; i < k; i++) {
    solution.push(remove(points, distance));
  }

  return solution;

  // read 0, replace 0 with last position, heapifyDown
  function remove(heap, weightFunction) {
    let val = heap[0];
    heap[0] = heap.pop();
    heapifyDown(heap, 0, weightFunction);
    return val;
  }

  // compare with children, swap with smallest, repeat
  function heapifyDown(heap, idx, weightFunction) {
    let left = 2 * idx + 1;
    let right = 2 * idx + 2;
    let smallest = left;

    if (left >= heap.length) return;

    if (right < heap.length && weightFunction(heap[left]) > weightFunction(heap[right])) {
      smallest = right;
    }

    if (weightFunction(heap[idx]) > weightFunction(heap[smallest])) {
      [heap[idx], heap[smallest]] = [heap[smallest], heap[idx]];
      heapifyDown(heap, smallest, weightFunction);
    }
  }

  function distance(point) {
    return point[0] * point[0] + point[1] * point[1];
  }
};
```
