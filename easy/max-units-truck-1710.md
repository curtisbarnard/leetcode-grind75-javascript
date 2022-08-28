# Maximum Units on a Truck

Page on leetcode: https://leetcode.com/problems/maximum-units-on-a-truck/

## Problem Statement

You are assigned to put some amount of boxes onto one truck. You are given a 2D array boxTypes, where boxTypes[i] = [numberOfBoxesi, numberOfUnitsPerBoxi]:

- numberOfBoxesi is the number of boxes of type i.
- numberOfUnitsPerBoxi is the number of units in each box of the type i.

You are also given an integer truckSize, which is the maximum number of boxes that can be put on the truck. You can choose any boxes to put on the truck as long as the number of boxes does not exceed truckSize.

Return the maximum total number of units that can be put on the truck.

### Constraints

- 1 <= boxTypes.length <= 1000
- 1 <= numberOfBoxesi, numberOfUnitsPerBoxi <= 1000
- 1 <= truckSize <= 106

### Example

```
Input: boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
Output: 8
Explanation: There are:
- 1 box of the first type that contains 3 units.
- 2 boxes of the second type that contain 2 units each.
- 3 boxes of the third type that contain 1 unit each.
You can take all the boxes of the first and second types, and one box of the third type.
The total number of units will be = (1 * 3) + (2 * 2) + (1 * 1) = 8.
```

## Solution

- Array is not sorted
- Will Truck size always be less than box available count?

### Pseudocode

1. Sort array based on units per box from largest to smallest
2. Create a max units counter
3. Create a box counter
4. Go thru array and add boxes

### Initial Solution

This solution has a time complexity O(nlogn) and space complexity of O(n) due to sorting the array. You can see an explanation of this approach here: https://www.youtube.com/watch?v=qZUokf5dwYw

```javascript
const maximumUnits = function (boxTypes, truckSize) {
  boxTypes.sort((a, b) => b[1] - a[1]);

  let boxCount = 0;
  let totalUnits = 0;

  for (let i = 0; i < boxTypes.length; i++) {
    // Truck is full, return total Units
    if (boxCount === truckSize) {
      return totalUnits;
    }
    let boxes;

    // How much room is left on the truck for current boxes
    if (boxTypes[i][0] <= truckSize - boxCount) {
      boxes = boxTypes[i][0];
    } else {
      boxes = truckSize - boxCount;
    }

    // Increase box and unit counts then move to next box
    boxCount += boxes;
    totalUnits += boxes * boxTypes[i][1];
  }

  return totalUnits;
};
```

### Optimized Solution

This alternative to using the built in sort method uses a bucket sort with a O(n) time and space complexity. You can see an explanation of bucket sort here: https://www.youtube.com/watch?v=ELrhrrCjDOA

```javascript
const maximumUnits = function (boxTypes, truckSize) {
  // Bucket sort the boxes. Buckets are the number of units per box and the value is the number of boxes.
  const bucket = [];
  for (let i = 0; i < boxTypes.length; i++) {
    const [numBoxes, numUnits] = boxTypes[i];
    if (!bucket[numUnits]) {
      bucket[numUnits] = numBoxes;
    } else {
      bucket[numUnits] += numBoxes;
    }
  }

  let totalUnits = 0;

  for (let i = bucket.length - 1; i >= 0; i--) {
    // Truck is full, return total Units
    if (truckSize === 0) {
      return totalUnits;
    }

    // Skip bucket if empty
    if (!bucket[i]) {
      continue;
    }

    // Fill truck and return units as num of boxes is larger than space left on truck
    if (bucket[i] > truckSize) {
      totalUnits += i * truckSize;
      return totalUnits;
    } else {
      // Otherwise add boxes to truck and reduce space left on truck
      totalUnits += i * bucket[i];
      truckSize -= bucket[i];
    }
  }

  return totalUnits;
};
```
