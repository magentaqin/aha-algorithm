# Best Time to Buy and Sell Stock with Cooldown[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/]

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)
Example:

Input: [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]


#### Answer One(Memoization Way)
To recursively solve this problem, you have three choices.
If current prices is less than or equal to previous one, buy it.
if current price is more than previous one: 1.sell it and skip one item. 2.skip current sell. look for bigger profit.
```javascript
var maxProfit = function(prices) {
  const transactions = {};

  const calculate = (currentIndex, buyIndex) => {
    const transactionIndex = [currentIndex, buyIndex];
    const currentPrice = prices[currentIndex];
    const buyPrice = prices[buyIndex];

    if (currentIndex > prices.length - 1) return 0;

    if (transactions[transactionIndex]) return transactions[transactionIndex];

    // set profit to -Infinity on every recursion.
    let profit = -Infinity;

    if (currentPrice <= buyPrice) {
      // buy current.
      profit = Math.max(profit, calculate(currentIndex+1, currentIndex))
    } else {
      const nextBuyIndex = currentIndex + 2;
      const nextSellIndex = currentIndex + 3;
      // sell current and move to nextBuyIndex.
      profit = Math.max(profit, currentPrice - buyPrice + calculate(nextSellIndex, nextBuyIndex))

      // move on to next index, sell.
      profit = Math.max(profit, calculate(currentIndex+1, buyIndex))
    }
    transactions[transactionIndex] = profit;
    return profit;
   }

  return calculate(1, 0);
};
```

Let's see the whole algorithm process. And take input [1, 2, 4] as an example.

- 1st Round call `calculate`. currentIndex 1 and buyIndex 0.
SELL CURRENT => profit = Math.max(-Infinity, 2 - 1 + calculate(4, 3))
  - 2nd Round call `calculate`. currentIndex 4, buyIndex 3. POP `0`.
profit = Math.max(-Infinity, 2 - 1 + 0) = 1;
MOVE ON TO NEXT INDEX TO SELL => profit = Math.max(1, calculate(2, 0))
  - 3rd Round call `calculate`. currentIndex 2, buyIndex 0.
  SELL CURRENT => profit = Math.max(-Infinity, 4 - 1 + calculate(5, 4))
    - 4th Round call `calculate`. currentIndex 5, buyIndex 4. POP `0`.
  profit = Math.max(-Infinity, 3+0) = 3;
  MOVE ON TO NEXT INDEX TO SELL => profit = Math.max(3, calculate(3, 0));
    - 5th Round call `calculate`. currentIndex 3, buyIndex 0. POP `0`.
  profit = Math.max(3, 0) = 3;
  transactions["2,0"] = 3;
  POP `3`;
profit = Math.max(1, 3) = 3;
transactions["1,0"] = 3;
POP `3`;


