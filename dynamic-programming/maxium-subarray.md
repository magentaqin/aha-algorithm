# [Maxium Subarray](https://leetcode.com/problems/maximum-subarray/)
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Example:

Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
Follow up:

If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

#### Solution One(DP, Runtime 64ms)
`dp[i]` indicates the max value you can get when moves to `i` index of nums array.

The tricky part is

```javascript
var maxSubArray = function(nums) {
  const dp = Array(nums.length).fill(-Infinity);
  dp[0] = nums[0];
  let currentSum = nums[0]
  for (let i = 1; i < nums.length; i++) {
    const current = nums[i];
    currentSum = Math.max(current, currentSum + current);
    dp[i] = Math.max(currentSum, dp[i-1]);
  }
  return dp[nums.length-1];
};
```