# Best Time to Buy and Sell Stock

Page on leetcode:https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

## Problem Statement

You are given an array prices where prices[i] is the price of a given stock on the ith day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

### Constraints

- 1 <= prices.length <= 105
- 0 <= prices[i] <= 104

### Example

With Profit:

> Input: prices = [7,1,5,3,6,4]
> Output: 5
> Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
> Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

Without Profit:

> Input: prices = [7,6,4,3,1]
> Output: 0
> Explanation: In this case, no transactions are done and the max profit = 0.

## Solution

### Pseudocode

1. If array is length 1 return 0
2. Declare lowest val and profit variables
3. loop thru array
4. Store lowest value a i = 1
5. check if i + 1 minus lowest is positive, if so store profit, else continue
6. next i store as lowest value if less than lowest value
7. repeat 5

### Initial Solution

This solution has a time complexity O(n)

```javascript
const maxProfit = function (prices) {
  let lowVal = prices[0];
  let profit = 0;
  for (i = 0; i < prices.length; i++) {
    const nextVal = prices[i + 1] || 0;
    if (prices[i] < lowVal) {
      lowVal = prices[i];
    }
    if (nextVal - lowVal > profit) {
      profit = nextVal - lowVal;
    }
  }
  return profit;
};
```

### Optimized Solution

Essentially the same solution with modified references using "Two-Pointers" It has the same time and space complexity as the initial solution. See this video for an explanation: https://www.youtube.com/watch?v=1pkOgXD63yU

```javascript
const maxProfit = function (prices) {
  let l = 0,
    r = 1,
    maxProfit = 0;
  while (r < prices.length) {
    if (prices[l] < prices[r]) {
      const profit = prices[r] - prices[l];
      maxProfit = Math.max(profit, maxProfit);
    } else {
      l = r;
    }
    r++;
  }
  return maxProfit;
};
```
