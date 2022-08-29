# Coin Change

Page on leetcode: https://leetcode.com/problems/coin-change/

## Problem Statement

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

### Constraints

- 1 <= coins.length <= 12
- 1 <= coins[i] <= 231 - 1
- 0 <= amount <= 104

### Example

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

## Solution

- Greedy algorithm
- Is the coin array sorted? No

### Pseudocode

1. Sort coins largest to smallest
2. Create counter variable
3. Greedy take largest coin until not divisible into remaining amount
4. Move to next amount and repeat until amount is reached
5. If at end of coins and can't reach amount return -1

### Initial Attempt

Greedy doesn't work as it takes a large coin and may return -1 even when you could find a solution with the second largest coin.

```javascript
const coinChange = function (coins, amount) {
  coins.sort((a, b) => b - a);
  let count = 0;
  for (let i = 0; i < coins.length; i++) {
    if (amount === 0) {
      return count;
    }
    const numDivisible = Math.floor(amount / coins[i]);
    amount -= numDivisible * coins[i];
    count += numDivisible;
  }
  if (amount > 0) {
    return -1;
  }
  return count;
};
```

### Optimized Solution

This problem can be solved using bottom up dynamic programming. Time complexity for this solution is O(n\*m) and space complexity is O(n), where n is amount and m is coins length. You can see an explanation of the solution here: https://www.youtube.com/watch?v=H9bfqozjoqs

```javascript
// Dynamic Programming
const coinChange = function (coins, amount) {
  //  Create array and fill with a max value. This will help later for determining if a solution was found.
  const dpCounts = Array(amount + 1).fill(amount + 1);
  // Base case
  dpCounts[0] = 0;

  // Fill out all cache slots with the minimum amount of coins
  for (let i = 1; i < dpCounts.length; i++) {
    for (const coin of coins) {
      // Make sure that coin can actual make up amount i
      if (i - coin >= 0) {
        dpCounts[i] = Math.min(dpCounts[i], 1 + dpCounts[i - coin]);
      }
    }
  }
  // Make sure a solution was found
  if (dpCounts[amount] < amount + 1) {
    return dpCounts[amount];
  } else {
    return -1;
  }
};

// Breadth First Search
const coinChange = function (coins, amount) {
  // If amount is zero, always returns 0
  if (amount === 0) {
    return 0;
  }
  //  Create array and fill with a temp value. This will help later for determining if a solution was found.
  const counts = Array(amount + 1).fill(-1);

  // Create queue for BFS and numOfCoins to track the "depth" of the search
  const queue = [0];
  let numOfCoins = 0;

  while (queue.length > 0) {
    numOfCoins++;
    // Checking nodes for this level
    let levelNodes = queue.length;
    while (levelNodes > 0) {
      const current = queue.shift();
      // Check current amount plus each coin
      for (const coin of coins) {
        if (current + coin === amount) {
          // Found a match at this level. Since it is BFS this will be the lowest level possible.
          return numOfCoins;
        }
        // Skip if the total is more than the amount or if it has already been calculated
        if (current + coin < counts.length && counts[current + coin] === -1) {
          // Mark amount as calculated and add to queue to be process with next set of coins
          counts[current + coin] = numOfCoins;
          queue.push(current + coin);
        }
      }
      levelNodes--;
    }
  }
  // If BFS completes and numOFCoins doesn't return then no solution exist
  return -1;
};
```
