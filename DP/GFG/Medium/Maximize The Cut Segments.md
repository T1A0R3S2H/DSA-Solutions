# Tabulation (TC: O(n) and SC: O(n))🔥🔥
### **Code**:
```cpp
vector<int> dp(n+1, INT_MIN);
        dp[0]=0;
        for(int i=0;i<=n;i++){
            if(i-x>=0) dp[i]=max(dp[i], dp[i-x]+1);
            if(i-y>=0) dp[i]=max(dp[i], dp[i-y]+1);
            if(i-z>=0) dp[i]=max(dp[i], dp[i-z]+1);
        }
        if(dp[n]<0) return 0;
        return dp[n];
```
### **Explanation**:

The goal is to find the maximum number of segments you can obtain from a line segment of length `n` by repeatedly cutting the line into smaller segments of lengths `x`, `y`, or `z`. If it's impossible to make a valid cut, you return `0`.

#### Approach:
- We use **dynamic programming** to solve this problem.
- Let `dp[i]` represent the maximum number of cuts that can be obtained for a line segment of length `i`.
- The idea is to build the solution for all lengths from `0` to `n`.
  
- **Base case**: `dp[0] = 0`, since no cuts are needed if the length is `0`.
- For every possible length `i` from `0` to `n`, we check if it's possible to make a cut of length `x`, `y`, or `z`:
  - If `i - x >= 0`, then `dp[i] = max(dp[i], dp[i - x] + 1)` (i.e., if we cut a piece of length `x`, the number of cuts for the remaining segment is `dp[i - x]`, and we add 1 for the current cut).
  - Similarly, we update for `y` and `z`.
  
- After processing all lengths from `0` to `n`, `dp[n]` will store the maximum number of cuts possible. If it's not possible to make a valid cut, the value will remain negative (initialized to `INT_MIN`), and we return `0`.

### **Time Complexity**:

- **Outer loop**: The `for` loop runs for all values of `i` from `0` to `n`. So, it runs `O(n)` times.
- **Inner updates**: For each value of `i`, we make up to 3 comparisons (for `x`, `y`, and `z`), which is constant time (`O(1)`).
  
Thus, the overall time complexity is **O(n)**.

### **Space Complexity**:

- We use a `dp` array of size `n + 1` to store the results for each length from `0` to `n`. Hence, the space complexity is **O(n)**.

### **Dry Run**:

Let's dry run the code for an example to see how it works:

#### Example 1:
**Input**: `n = 4`, `x = 2`, `y = 1`, `z = 1`

- **Initialization**: 
  - `dp = [0, INT_MIN, INT_MIN, INT_MIN, INT_MIN]`
  
- **Step-by-step updates**:

  - For `i = 0`:
    - No cuts are made, so `dp[0] = 0`.
  
  - For `i = 1`:
    - We can cut a segment of length `1` (`y = 1` or `z = 1`). So, `dp[1] = max(dp[1], dp[0] + 1) = 1`.
    - `dp = [0, 1, INT_MIN, INT_MIN, INT_MIN]`
    
  - For `i = 2`:
    - We can cut a segment of length `2` (`x = 2`). So, `dp[2] = max(dp[2], dp[0] + 1) = 1`.
    - We can also cut two segments of length `1` (`y = 1` or `z = 1`), so `dp[2] = max(dp[2], dp[1] + 1) = 2`.
    - `dp = [0, 1, 2, INT_MIN, INT_MIN]`
    
  - For `i = 3`:
    - We can cut a segment of length `2` followed by a segment of length `1`, so `dp[3] = max(dp[3], dp[1] + 1) = 2`.
    - We can also cut three segments of length `1`, so `dp[3] = max(dp[3], dp[2] + 1) = 3`.
    - `dp = [0, 1, 2, 3, INT_MIN]`
    
  - For `i = 4`:
    - We can cut two segments of length `2`, so `dp[4] = max(dp[4], dp[2] + 1) = 3`.
    - We can also cut four segments of length `1`, so `dp[4] = max(dp[4], dp[3] + 1) = 4`.
    - `dp = [0, 1, 2, 3, 4]`
    
**Output**: `dp[4] = 4` (maximum 4 cuts can be made).

#### Example 2:
**Input**: `n = 5`, `x = 5`, `y = 3`, `z = 2`

- **Initialization**: 
  - `dp = [0, INT_MIN, INT_MIN, INT_MIN, INT_MIN, INT_MIN]`
  
- **Step-by-step updates**:
  - For `i = 0`:
    - `dp[0] = 0`
  
  - For `i = 1` to `i = 2`: No cuts can be made (`dp[1]` and `dp[2]` remain `INT_MIN`).
  
  - For `i = 3`:
    - We can cut a segment of length `3` (`y = 3`), so `dp[3] = dp[0] + 1 = 1`.
    - `dp = [0, INT_MIN, INT_MIN, 1, INT_MIN, INT_MIN]`
  
  - For `i = 4`: No cuts can be made.
  
  - For `i = 5`:
    - We can cut a segment of length `5` (`x = 5`), so `dp[5] = dp[0] + 1 = 1`.
    - We can also cut segments of lengths `3` and `2`, so `dp[5] = dp[3] + 1 = 2`.
    - `dp = [0, INT_MIN, INT_MIN, 1, INT_MIN, 2]`
    
**Output**: `dp[5] = 2` (maximum 2 cuts can be made).

---

This approach effectively uses dynamic programming to solve the problem in optimal time and space.
