### Code
```cpp
class Solution {
public:
    // Recursive function (not optimized)
    int solveRec(int n, int m, vector<vector<int>> &mat, int i, int j) {
        if (i < 0 || j < 0) return 0;
        if (mat[i][j] == 0) return 0;
        
        int left = solveRec(n, m, mat, i, j - 1);
        int up = solveRec(n, m, mat, i - 1, j);
        int diag = solveRec(n, m, mat, i - 1, j - 1);
        
        return 1 + min({left, up, diag});
    }
    
    // Memoization function
    int solveMem(int n, int m, vector<vector<int>> &mat, int i, int j, vector<vector<int>> &dp) {
        if (i < 0 || j < 0) return 0;
        if (mat[i][j] == 0) return 0;
        if (dp[i][j] != -1) return dp[i][j];
        
        int left = solveMem(n, m, mat, i, j - 1, dp);
        int up = solveMem(n, m, mat, i - 1, j, dp);
        int diag = solveMem(n, m, mat, i - 1, j - 1, dp);
        
        return dp[i][j] = 1 + min({left, up, diag});
    }

    // Tabulation function
    int solveTab(int n, int m, vector<vector<int>> &mat) {
        vector<vector<int>> dp(n, vector<int>(m, 0));
        int maxi = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (mat[i][j] == 0) {
                    dp[i][j] = 0;
                } else {
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1; // first row or column
                    } else {
                        dp[i][j] = 1 + min({dp[i][j - 1], dp[i - 1][j - 1], dp[i - 1][j]});
                    }
                }
                maxi = max(maxi, dp[i][j]);
            }
        }
        return maxi;
    }

    // Space-optimized function
    int solveSO(int n, int m, vector<vector<int>> &mat) {
        vector<int> prev(m, 0), curr(m, 0);
        int maxi = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (mat[i][j] == 0) {
                    curr[j] = 0;
                } else {
                    if (i == 0 || j == 0) {
                        curr[j] = 1; // first row or column
                    } else {
                        curr[j] = 1 + min({curr[j - 1], prev[j - 1], prev[j]});
                    }
                }
                maxi = max(maxi, curr[j]);
            }
            prev = curr; // update prev for next iteration
        }
        return maxi;
    }

    int maxSquare(int n, int m, vector<vector<int>> mat) {
        return solveTab(n, m, mat); // Call to the tabulation function
        // Uncomment below lines to test other implementations
        // vector<vector<int>> dp(n, vector<int>(m, -1));
        // return solveMem(n, m, mat, n - 1, m - 1, dp); // Memoization
        // return solveRec(n, m, mat, n - 1, m - 1); // Recursive
        // return solveSO(n, m, mat); // Space Optimized
    }
};
```

### Space Complexity - O(1)
```cpp
int space_optimization1(int m ,int n, vector<vector<int>>&mat){
        
        int maxi = 0;
        for(int i=n-1; i>=0; i--){
            for(int j=m-1; j>=0; j--){
                int down = i<n-1?mat[i+1][j]:0;
                int right = j<m-1? mat[i][j+1]:0;
                int diag = i<n-1 and j<m-1? mat[i+1][j+1]:0;
                // if(dp[i][j] == 1)
                if(mat[i][j] == 1){
                    mat[i][j] = 1 + min(down,min(right, diag));
                    maxi = max(mat[i][j],maxi);
                }
            }
        }
        return maxi;
    }
```

### Explanation

1. **solveRec**: A recursive approach to find the maximum square size. It's not optimized for large inputs due to repeated calculations.

2. **solveMem**: Uses memoization to store intermediate results, optimizing the recursive approach and improving performance.

3. **solveTab**: Implements dynamic programming with a bottom-up approach using tabulation, storing results in a 2D array.

4. **solveSO**: A space-optimized version of the tabulation approach that uses only two rows to store results.

5. **maxSquare**: The main function that currently calls the tabulation method. You can uncomment other lines to test the other implementations.
