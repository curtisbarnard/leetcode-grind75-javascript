# Combination Sum

Page on leetcode: https://leetcode.com/problems/combination-sum/

## Problem Statement

Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is guaranteed that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

### Constraints

- 1 <= candidates.length <= 30
- 1 <= candidates[i] <= 200
- All elements of candidates are distinct.
- 1 <= target <= 500

### Example

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

## Solution

- can I use greedy?
- array not sorted
- iterate thru each element
- need a result array

### Pseudocode

1. Iterate thru array
2. while sum is less than target keep summing
3. iterate on second loop

### Initial Attempt

```javascript
const combinationSum = function (candidates, target) {
  for (let i = 0; i < candidates.length; i++) {
    let sum = 0;
    const possible = [];
    while (sum < target) {
      sum += candidates[i];
      possible.push(candidate[i]);
      for (let j = i + 1; j < candidates.length; j++) {}
    }
  }
};
```

### Optimized Solution

The below approach uses DFS recursion and a state space tree. The time complexity is O(2<sup>t</sup>) and space complexity is O(length of longest combo array). A discussion about time and space complexity can be found here: https://leetcode.com/problems/combination-sum/discuss/1755084/Detailed-Time-and-Space-Complecity-analysisc++javabacktracking

You can see an explanation of this solution here: https://www.youtube.com/watch?v=GBKI9VSKdGg

```javascript
const combinationSum = function (candidates, target) {
  const result = [];

  function dfs(i, combo, total) {
    if (total === target) {
      // Found a combo that adds to target, add a copy to the result so that you don't overwrite it on subsequent recursive calls.
      result.push([...combo]);
      return;
    } else if (total > target || i >= candidates.length) {
      // Can't go any further down this path
      return;
    }

    // Continue down left path which adds another occurrence of candidates[i]
    combo.push(candidates[i]);
    dfs(i, combo, total + candidates[i]);

    // Continue down the right path with no more occurrences of candidates[i]
    combo.pop();
    dfs(i + 1, combo, total);
  }

  dfs(0, [], 0);
  return result;
};
```
