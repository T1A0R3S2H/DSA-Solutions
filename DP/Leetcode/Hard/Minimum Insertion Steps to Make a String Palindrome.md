### Code:
```cpp
class Solution {
public:
    // Tabulation function (solveTab)
    int solveTab(string &text1, string &text2) {
        int m=text1.size(), n = text2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i= m-1; i>=0; i--) {
            for (int j=n-1; j >= 0; j--) {
                if (text1[i]==text2[j]) dp[i][j]=1+dp[i+1][j+1];
                else dp[i][j]=max(dp[i+1][j], dp[i][j+1]);
            }
        }
        return dp[0][0];
    }

    int minInsertions(string s) {
        int n=s.length();
        string revS=s;
        reverse(revS.begin(), revS.end());
        int ans=solveTab(s, revS);
        return n-ans;
    }
};
```
**Gist:**
- If we know the longest palindromic sub-sequence is x and the length of the string is n then, what is the answer to this problem? It is n - x as we need n - x insertions to make the remaining characters also palindrome.

**Explanation:**
1. **Problem Understanding**: 
   - The task is to determine the minimum number of insertions required to make a string palindrome. 
   - A palindrome reads the same forward and backward. 
   - The minimum number of insertions can be derived by finding the longest palindromic subsequence in the string. 
   - If we subtract the length of the longest palindromic subsequence from the total string length, the result gives the minimum number of insertions needed.

2. **Approach**:
   - The key insight is that the number of insertions required to make a string a palindrome is related to its longest palindromic subsequence.
   - The longest palindromic subsequence can be found by comparing the string with its reverse. The longest common subsequence (LCS) of the string and its reverse is the longest palindromic subsequence.
   - After finding the longest palindromic subsequence, we subtract its length from the original string length to get the result.

3. **Time Complexity**:
   - Time complexity is O(n^2), where n is the length of the string. This is because the dynamic programming approach computes the LCS using a 2D DP table.

4. **Space Complexity**:
   - Space complexity is O(n^2) due to the 2D DP table used to store the LCS results.

**Dry Run:**
For the input `s = "mbadm"`:
- The reversed string is `revS = "mdabm"`.
- The longest palindromic subsequence between `s` and `revS` is "madam" with length 3.
- The minimum insertions will be `5 - 3 = 2`. Thus, the answer is 2.
