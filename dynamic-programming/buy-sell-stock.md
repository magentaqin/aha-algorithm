# [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

Example
```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

#### Answer One(O(n^2)Runtime 460ms)
The basic idea is about finding the max value and the min value.

Restricted condition is that the index of the max value is above the index of the min value.

Max Profit is dynamically decided by comparing the current profit with the previous profit.

```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
  let maxProfit = 0;

  for (let x = 0; x < prices.length - 1; x++) {
    const buyPrice = prices[x];

    let y = prices.length - 1;
    while(y > x) {
      const sellPrice = prices[y];
      const currentProfit = sellPrice - buyPrice;
      if (currentProfit > maxProfit) {
        maxProfit = currentProfit
      }
      y--;
    }
  }
  return maxProfit;
};
```


#### Answer Two(O(n), Run Time 64ms)
This algorithm saves the loop by looking for the maxProfit as well as looking for the min value.

The trick thing is that it supposes the min value is the biggest integer. Imperatively, the first element is supposedly to be the min value.
```javascript
var maxProfit = function(prices) {
  let min = Number.MAX_SAFE_INTEGER;
  let maxProfit = 0;
    for (var i = 0; i < prices.length; i++) {
        min = Math.min(min, prices[i]);
        maxProfit = Math.max(maxProfit, prices[i] - min);
    }
  return maxProfit;
};
```
