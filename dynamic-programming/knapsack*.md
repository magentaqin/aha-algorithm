# Knapsack
Give items that have weights and values, and an integer W that denotes the size of a knapsack。
Find the biggest values you can get to fill this knapsack.


#### Solution One
```javascript
// i: index of items to be packed
// j: iteration index from 0 to W
function knapsack(weights, values, W) {
  const n = weights.length;
  const dp = new Array(n);
  dp[-1] = new Array(W+1).fill(0);
  for (let i = 0; i < n; i++) {
    dp[i] = new Array(W).fill(0);
    for (let j = 0; j <= W; j++) {
      if (j < weights[i]) {
        dp[i][j] = dp[i-1][j]
      } else {
        const prevMax = dp[i-1][j];
        const sum = values[i] + dp[i-1][j-weights[i]];
        dp[i][j] = Math.max(prevMax, sum);
      }
    }
  }
  return dp[n-1][W];
}

const a = knapsack([2, 2,6, 5, 4], [6, 3, 5, 4, 6], 10)
console.log(a) // 15
```

