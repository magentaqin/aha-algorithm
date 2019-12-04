# [Best time to buy and sell stock (II)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

Example 1:

Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.

#### Answer One(O(n), run time 64ms)
The problem flattens to this: sell immediately as you can.
```javascript
var maxProfit = function(prices) {
  return prices.reduce((profit, price, i, prices) => {
    let sum = profit;
    if (i > 0) {
      if (price > prices[i-1]) {
        sum += price - prices[i-1];
      }
    }
    return sum;
  }, 0)
};
```