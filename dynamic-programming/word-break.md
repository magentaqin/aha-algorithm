# Word Break(https://leetcode.com/problems/word-break/)

Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

Note:

The same word in the dictionary may be reused multiple times in the segmentation.
You may assume the dictionary does not contain duplicate words.
Example 1:

Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
Example 2:

Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
Example 3:

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false

#### Solution(Runtime 52ms)
`dp[i+1]`: whether it can be segemented into dictionary words when you stopped at index `i` of `s`.

```javascript
var wordBreak = function(s, wordDict) {
  const dp = Array(s.length+1).fill(false);
  dp[0] = true;
  for (let i = 1; i < s.length+1; i++) {
    for (let j = 0; j < i; j++) {
      const word = s.slice(j, i);
      if (dp[j] === true && wordDict.includes(word)) {
        dp[i] = true;
        break;
      }
    }
  }
  return dp[s.length];
}
```