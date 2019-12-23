# Decode Ways(https://leetcode.com/problems/decode-ways/)

A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26
Given a non-empty string containing only digits, determine the total number of ways to decode it.

Example 1:

Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
Example 2:

Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

#### Solution(O(n), Runtime: 64ms)
`dp[i]`: numer of ways to parse `s.slice(0, i+1)`.
* Edge case:
```
Input: "0" Output: 0
Input: "10" Output: 1
Input: "27" Output: 1
```
* Basic Idea:

During the loop,
1) if the current charater can be encoded, `dp[i] += dp[i-1]`,
2) if the combined value of prev charater and current charater can be encoded, `dp[i] += dp[i-2]`

```javascript
var numDecodings = function(s) {
   if (!s) return 0;
  const dp = Array(s.length+1).fill(0);
  dp[0] = 1;
  // if s === '0'
  if (parseInt(s[0]) > 0) {
    dp[1] = 1;
  }
  for (let i = 2; i < s.length + 1; i++) {
    const current = parseInt(s.slice(i-1, i));
    const prev = parseInt(s.slice(i-2, i));
    if (current > 0 && current <= 9) {
      dp[i] += dp[i-1];
    }

    if (prev >= 10 && prev <= 26) {
      dp[i] += dp[i-2];
    }
  }
  return dp[s.length];
}
```