### TLDR:
The approach to solving the **Shortest Common Supersequence (SCS)** problem involves first calculating the **Longest Common Subsequence (LCS)** of the two strings. The LCS helps identify the characters that are common between the two strings in the same order, which will appear only once in the final SCS. After calculating the LCS, we then merge the two strings by adding characters from both strings while ensuring that the common characters (from the LCS) appear only once. This is done by iterating through both strings and comparing characters: if they are equal, we add that character to the result; if not, we add the character from the string where the next value in the LCS matrix suggests the better choice. Finally, we append any remaining characters from either string to the result. The time complexity of this approach is dominated by the calculation of the LCS, which is O(m * n), where `m` and `n` are the lengths of the two strings.
### Code:

```cpp
class Solution {
public:
    int solveMem(string &text1, string &text2, int i, int j, vector<vector<int>> &dp) {
        if (i==text1.size() || j==text2.size()) return 0;
        if (dp[i][j]!=-1) return dp[i][j];
        if (text1[i]==text2[j])
            dp[i][j]=1+solveMem(text1, text2, i+1, j+1, dp);
        else
            dp[i][j]=max(solveMem(text1, text2, i+1, j, dp), solveMem(text1, text2, i, j+1, dp));
        return dp[i][j];
    }

    string shortestCommonSupersequence(string str1, string str2) {
        int m=str1.size(), n=str2.size();
        vector<vector<int>> dp(m+1, vector<int>(n+1, -1));
        
        // LCS nikalo
        solveMem(str1, str2, 0, 0, dp);
        
        // SCS banao
        string result="";
        int i=0, j=0;
        
        while (i<m && j<n) {
            if (str1[i]==str2[j]) {
                result+=str1[i];  //ek baar common chars add karo
                i++;
                j++;
            } 
            else if (dp[i+1][j]>dp[i][j+1]) {
                result+=str1[i];  // str1 se add karo
                i++;
            } 
            else {
                result+=str2[j];  //str2 se add karo
                j++;
            }
        }
        
        // bache hue chars from str1 ya str2 add karo
        while (i<m) {
            result+=str1[i];
            i++;
        }
        while (j<n) {
            result+=str2[j];
            j++;
        }
        return result;
    }
};

```

### Explanation:
1. **`solveMem`**: This function is used to calculate the **Longest Common Subsequence (LCS)** recursively with memoization.
   - We use a 2D memoization table `memo` to store the results of subproblems to avoid redundant calculations.
   - If the characters at `text1[i]` and `text2[j]` are the same, the value is `1 + solveMem(i+1, j+1)`.
   - If they are different, the value is the maximum of two choices: either move in `text1` or move in `text2`.

2. **`shortestCommonSupersequence`**:
   - After calculating the LCS, the function reconstructs the shortest common supersequence by iterating over both strings.
   - If the characters match, we add the common character to the result.
   - If they don't match, we add the character from the string where the next value in the memoization table is larger, indicating it's the better choice.
   - Finally, any remaining characters from either string are appended to the result.

### Time and Space Complexity:
- **Time Complexity**: O(m * n), where `m` and `n` are the lengths of `str1` and `str2`, respectively, due to the memoization table and the two passes for constructing the SCS.
- **Space Complexity**: O(m * n), for the memoization table used to store the results of subproblems.
### Example:

Let’s take the following two strings:

- `str1 = "abac"`
- `str2 = "cab"`

### Step 1: Calculate the LCS

We use the `solveMem` function with memoization to calculate the **Longest Common Subsequence** (LCS) first.

#### Initialize the DP table (memoization table):
We create a memoization table `memo` of size `[m][n]`, where `m` is the length of `str1` and `n` is the length of `str2`. Initially, all values in `memo` are set to `-1`.

```
memo:
[[-1, -1, -1], 
 [-1, -1, -1], 
 [-1, -1, -1], 
 [-1, -1, -1]]
```

#### Fill the `memo` table using the recursive `solveMem` function:
The recursive function computes the LCS by filling the table.

1. **i = 3, j = 2**: `text1[3] = "c"`, `text2[2] = "b"`, not equal.
   - We take the maximum of `solveMem(i+1, j)` and `solveMem(i, j+1)` which will be `max(0, 1) = 1`. 
   - So, `memo[3][2] = 1`.

2. **i = 2, j = 1**: `text1[2] = "a"`, `text2[1] = "a"`, equal.
   - We add 1 to the result of `solveMem(i+1, j+1)`. This will be `1 + solveMem(3, 2) = 1 + 1 = 2`.
   - So, `memo[2][1] = 2`.

3. **i = 2, j = 0**: `text1[2] = "a"`, `text2[0] = "c"`, not equal.
   - We take the maximum of `solveMem(i+1, j)` and `solveMem(i, j+1)`, which will be `max(1, 2) = 2`.
   - So, `memo[2][0] = 2`.

4. **i = 1, j = 1**: `text1[1] = "b"`, `text2[1] = "a"`, not equal.
   - We take the maximum of `solveMem(i+1, j)` and `solveMem(i, j+1)`, which will be `max(2, 2) = 2`.
   - So, `memo[1][1] = 2`.

5. **i = 1, j = 0**: `text1[1] = "b"`, `text2[0] = "c"`, not equal.
   - We take the maximum of `solveMem(i+1, j)` and `solveMem(i, j+1)`, which will be `max(2, 2) = 2`.
   - So, `memo[1][0] = 2`.

6. **i = 0, j = 1**: `text1[0] = "a"`, `text2[1] = "a"`, equal.
   - We add 1 to the result of `solveMem(i+1, j+1)`. This will be `1 + solveMem(1, 2) = 1 + 2 = 3`.
   - So, `memo[0][1] = 3`.

7. **i = 0, j = 0**: `text1[0] = "a"`, `text2[0] = "c"`, not equal.
   - We take the maximum of `solveMem(i+1, j)` and `solveMem(i, j+1)`, which will be `max(2, 3) = 3`.
   - So, `memo[0][0] = 3`.

Now the final `memo` table looks like this:

```
memo:
[[3, 3, 3], 
 [2, 2, 2], 
 [2, 2, 1], 
 [1, 1, 1]]
```

The LCS length is `memo[0][0] = 3`.

The LCS itself is `"ab"`, as that is the longest common subsequence.

### Step 2: Construct the Shortest Common Supersequence (SCS)

Now, we will reconstruct the shortest common supersequence from the `memo` table.

1. Start from `i = 0`, `j = 0`. Initialize an empty result string: `result = ""`.

2. **i = 0, j = 0**: `text1[0] = "a"`, `text2[0] = "c"`, not equal.
   - Since `memo[i+1][j] = 2` is greater than `memo[i][j+1] = 3`, we add `text2[0] = "c"` to the result.
   - Now, `result = "c"`, and move `j = 1`.

3. **i = 0, j = 1**: `text1[0] = "a"`, `text2[1] = "a"`, equal.
   - Add `text1[0] = "a"` to the result.
   - Now, `result = "ca"`, and move `i = 1` and `j = 2`.

4. **i = 1, j = 2**: `text1[1] = "b"`, `text2[2] = "b"`, equal.
   - Add `text1[1] = "b"` to the result.
   - Now, `result = "cab"`, and move `i = 2` and `j = 3`.

5. **i = 2, j = 3**: `j = 3` is out of bounds (end of `text2`), so add the remaining characters from `text1`.
   - Add `text1[2] = "a"` to the result.
   - Now, `result = "caba"`, and move `i = 3`.

6. **i = 3, j = 3**: `i = 3` is out of bounds (end of `text1`), so add the remaining characters from `text2` (no characters left to add).

Final SCS: `"cabac"`

### Final Output:
The shortest common supersequence for the input `str1 = "abac"` and `str2 = "cab"` is `"cabac"`.

### Summary:
- **LCS** is `"ab"`.
- **SCS** is `"cabac"`.
