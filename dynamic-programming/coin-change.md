# Coin Change (https://leetcode.com/problems/coin-change/)
You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.
Example 1:

Input: coins = [1, 2, 5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
Example 2:

Input: coins = [2], amount = 3
Output: -1

#### Answer One (Memoization Way, 64ms)
```javascript
var coinChange = function(coins, amount) {
  coins.sort((a, b) => b - a);

  let res = Infinity;

  const find = (k, amount, count) => {
    const coin = coins[k];

    // last smallest coin
    if (k === coins.length - 1) {
      if (amount % coin === 0) {
        res = Math.min(res, count + parseInt(amount / coin));
      }
    } else {
      for (let i = parseInt(amount / coin); i >= 0 && count + i < res; i--) {
        find(k + 1, amount - coin * i, count + i);
      }
    }
  };

    find(0, amount, 0);
    return res === Infinity ? -1 : res;
}
```

* Basic Idea:
1) Descend sort the array.
2) Greedily choose the max value 5. The remainder possibilities are 1 and 6. Recursively call coin 2 to consume the remainder 1, and coin 1 to consume the remainder 6.
3)if the coin is the smallest coin, calculate the res value.

* Note
1) count variable: record the previous consumed coins.
2) for loop condition `count+i < res`. `res` is the minimum number of coins consumed. To avoid duplicate computation,`count+1 >= res` is not necessary to loop.

* Process
----k = 0, amount=11, count = 0, coin=5, res = Infinity, initial i = 2
    for(let i = 2; i >= 0 && 0 + i < Infinity>; i--)
    *find(1, 1, 2)
    ----k=1, amount=1, count=2, coin = 2, res=Infinity, initial i = 0.
        for(let i = 0; i >= 0 && 2 + i < Infinity; i --)
        find(2，1,2)
        ----k=2, amount=1, count=2, coin=1, res=Infinity. res = Math.min(Infinity, 3) = 3
    *find(1, 11 - 5*1, 1) = find(1, 6, 1)
    ----k=1,amount=6,count=1, coin=2, res=3, initial i=3.
    *find(1,11,0)
    ----k=1,amount=11,coin=2,count=0,res=3. initial i = 5;