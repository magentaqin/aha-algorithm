# House Robber(https://leetcode.com/problems/house-robber/)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

#### Solution
`maxMoney[n-1]` stands for max amount of money when you robbed `nth` house.
```javascript
var rob = function(nums) {
  if (!nums.length) return 0;
  const maxMoney = Array(nums.length).fill(0);
  for (i = 0; i < nums.length; i++) {
    const current = nums[i];
    if (i === 0) {
      maxMoney[i] = nums[i];
      continue;
    }
    if (i === 1) {
      maxMoney[i] = Math.max(current, nums[0]);
      continue;
    }
    maxMoney[i] = Math.max(maxMoney[i-1], maxMoney[i-2]+current)
  }
  return maxMoney[nums.length - 1];
};
```