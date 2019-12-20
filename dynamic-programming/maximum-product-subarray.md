# Maximum Product Subarray(https://leetcode.com/problems/maximum-product-subarray/)

Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

Example 1:

Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
Example 2:

Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.

#### Solution(60ms)
Assumption: You have to put the current num into the subarray in every iteration.

`max_dp[i]` means the largest product when you reached `i`. You have to compare these three values:
- max_dp[i-1] * nums[i]: prev max value * current value (e.g. 2*8)
- min_dp[i-1] * nums[i]: prev min value * current value (e.g. -2 * -8 )
- nums[i]

`min_dp[i]` means the smallest product when you reached `i`.

The reason why we should have this variable is that `min_dp[i]` and `nums[i]` can both be the negative num. And their product can be the largest one.
```javascript
var maxProduct = function(nums) {
  const max_dp = Array(nums.length).fill(0);
  const min_dp = Array(nums.length).fill(0);
  max_dp[0] = nums[0];
  min_dp[0] = nums[0];
  for (let i = 1; i < nums.length; i++) {
    max_dp[i] = Math.max(max_dp[i-1]*nums[i], min_dp[i-1]*nums[i], nums[i]);
    min_dp[i] = Math.min(max_dp[i-1]*nums[i], min_dp[i-1]*nums[i], nums[i]);
  }

  let max = 0;
  max_dp.forEach((item) => {
   max = Math.max(item, max)
  })
  return max;
};
```