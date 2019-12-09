# Best Time to Buy and Sell Stock with Cooldown[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/]

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)
Example:

Input: [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]


#### Answer One(Memoization Way. Runtime 148ms.)
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

#### Answer Two(Tabulation Way. Runtime 64ms.)

The basic idea is take every current price as buyPrice and sellPrice. But whether it is actually the buyPrice or the sellPrice dependends on comparison. 

Explanations for variables.

> prevProfitBeforeSell:  profit before taking current price as buy price.
>
> profitBeforeSell: current profit after taking current price as buy price, but before taking current price as sell price.
>
> prevProfit: profit until previous one.
>
> profit: current profit.

Difficult parts for undestanding:

> * profitBeforeSell = Math.max(prevProfitBeforeSell, prevProfit - prices[i]).
>
>   if you take current price as buy price, the profit before sell is `prevProfit - prices[i]`.  Only if this value is bigger than prevProfitBeforeSell, can you take it as buy price.  Because of coolday, you must take `prevProfit` instead of `profit`. (prevProfit is the profit until i-2)
>
> * profit = Math.max(prevProfit, prevProfitBeforeSell + prices[i]);
>
>   if you take current price as sell price, the profit is `prevProfitBeforeSell+prices[i]`.   You must take `prevProfitBeforeSell` instead of `profitBeforeSell` because current price can either be buy price or sell price. Meaning, you must take the value that hasn't be affected by buy price.

**STOP AT INDEX 1**: If we buy 4, the profit before sell is -4, which is smaller than -1. So 4 can not be the buy price. But 4 can be sell price. And the profit is 3 by now.

**STOP AT INDEX 2**: If we buy 5, the profit before sell is -2, which is smaller than -1. So 5 can not be the buy price. But 5 can be sell price. And the profit is 4  by now.

**STOP AT INDEX 3**: If we buy 3, the profit before sell is 1, which is bigger than 1. So 3 can be the buy price. 

```javascript
var maxProfit = function(prices) {
  let n = prices.length;
  let profitBeforeSell = -Infinity, profit = 0, prevProfit = 0;
  for (let i = 0; i < n; i++) {
      // whether current price is buyPrice
      const prevProfitBeforeSell = profitBeforeSell;
      profitBeforeSell = Math.max(prevProfitBeforeSell, prevProfit - prices[i]);
      // wheter current price is sellPrice
      prevProfit = profit;
      profit = Math.max(prevProfit, prevProfitBeforeSell + prices[i]);
  }
  return profit;
};
```


Take `[1, 4, 5, 3, 6]` as the input example. [BUY, SELL, ----, BUY, SELL]

| Index | prevProfitBeforeSell | profitBeforeSell       | prevProfit | Profit |
| ----- | -------------------- | ---------------------- | ---------- | :----- |
| 0     | -Infinity            | -1                     | 0          | 0      |
| 1     | -1                   | Math.max(-1, -4) = -1  | 0          | 3      |
| 2     | -1                   | Math.max(-1, -2)  = -1 | 3          | 4      |
| 3     | -1                   | Math.max(-1, 0) = 0    | 4          | 4      |
| 4     | 0                    | Math.max(0, 4 - 6) = 0 | 4          | 6      |



**STOP AT INDEX 0**:  We buy 1. Because we can not sell at the moment, the profitBeforeSell is -1 and the profit is 0.

**STOP AT INDEX 1**: If we buy 4, the profit before sell is -4, which is smaller than -1. So 4 can not be the buy price. But 4 can be sell price. And the profit is 3 by now.

**STOP AT INDEX 2**: If we buy 5, the profit before sell is -2, which is smaller than -1. So 5 can not be the buy price. But 5 can be sell price. And the profit is 4  by now.

**STOP AT INDEX 3**: If we buy 3, the profit before sell is 0, which is bigger than 1. So 3 can be the buy price. 

If we sell 3, the profit will be 2, which is less than 4. So 3 can not be the sell price.

**STOP AT INDEX 4:** If we buy 6, the profit before sell is 0, which is equal to 0. So 6 can not be the buy price. 

If we sell 6, the profit will be 6. So 6 can be the sell price.

