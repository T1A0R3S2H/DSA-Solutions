
### Code
```cpp
class Solution {
public:
    int solveRec(vector<vector<char>>& matrix, int i, int j, int& maxi){
        if(i>=matrix.size() || j>=matrix[0].size()) return 0;
        int right=solveRec(matrix, i, j+1, maxi);
        int diag=solveRec(matrix, i+1, j+1, maxi);
        int down=solveRec(matrix, i+1, j, maxi);
        if(matrix[i][j]=='1'){
            int ans=1+min(right, min(diag, down));
            maxi=max(maxi, ans);
            return ans;
        }
        else return 0;
    }

    int solveMem(vector<vector<char>>& matrix,int i,int j, int& maxi, vector<vector<int>>& dp){
        if(i>=matrix.size() || j>=matrix[0].size()) return 0;
        if(dp[i][j] != -1) return dp[i][j];
        int right=solveMem(matrix, i, j+1, maxi, dp);
        int diag=solveMem(matrix, i+1, j+1, maxi, dp);
        int down=solveMem(matrix, i+1, j, maxi, dp);
        if(matrix[i][j]=='1'){
            dp[i][j]=1+min(right, min(diag, down));
            maxi=max(maxi, dp[i][j]);
            return dp[i][j];
        }
        else {
            dp[i][j] = 0;
            return dp[i][j];
        }
    }

    int solveTab(vector<vector<char>>& matrix){
        int row=matrix.size();
        int col=matrix[0].size();
        vector<vector<int>>dp(row+1, vector<int>(col+1, 0));
        int maxi=0;
        for(int i=row-1; i>=0; i--){
            for(int j=col-1; j>=0; j--){
                int right=dp[i][j+1];
                int diag=dp[i+1][j+1];
                int down=dp[i+1][j];
                if(matrix[i][j]=='1'){
                    dp[i][j]=1+min(right, min(diag, down));
                    maxi=max(maxi, dp[i][j]);
                }
                else {
                    dp[i][j]=0;
                }
            }
        }
        return maxi * maxi;
    }

    int solveSO(vector<vector<char>>& matrix, int&maxi){
        int row=matrix.size();
        int col=matrix[0].size();
        vector<int> curr(col+1, 0);
        vector<int> next(col+1, 0);
        for(int i=row-1; i>=0; i--){
            for(int j=col-1; j>=0; j--){
                // apan row-1 and col-1 se chalu kar rahe hai par fir niche i+1 aur j+1 bhi
                // kar rahe hai, to overflow ho sakta hai, isliye row+1 and col+1 initially
                int right=curr[j+1];
                int diag=next[j+1];
                int down=next[j];
                if(matrix[i][j]=='1'){
                    curr[j]=1+min(right, min(diag, down));
                    maxi=max(maxi, curr[j]);
                }
                else {
                    curr[j]=0;
                }
            }
            next=curr;
        }
        return maxi * maxi;
    }

    int maximalSquare(vector<vector<char>>& matrix) {
        int m=matrix.size();
        int n=matrix[0].size();
        // int maxi=0;
        // solveRec(matrix, 0, 0, maxi);
        // return maxi*maxi;

        int maxi=0;
        // vector<vector<int>>dp(m, vector<int>(n, -1));
        // solveMem(matrix, 0, 0, maxi, dp);
        // return solveTab(matrix);
        return solveSO(matrix, maxi);
    }
};
```



### Explanation

#### 1. `solveRec`
- **Code**: 
  ```cpp
  int solveRec(vector<vector<char>>& matrix, int i, int j, int& maxi) {
      if (i >= matrix.size() || j >= matrix[0].size()) return 0;
      int right = solveRec(matrix, i, j + 1, maxi);
      int diag = solveRec(matrix, i + 1, j + 1, maxi);
      int down = solveRec(matrix, i + 1, j, maxi);
      if (matrix[i][j] == '1') {
          int ans = 1 + min(right, min(diag, down));
          maxi = max(maxi, ans);
          return ans;
      } else return 0;
  }
  ```
- **Explanation**: This function uses recursion to explore the matrix. It checks three directions (right, down, and diagonal) for contiguous '1's and updates the maximum size found. If the current cell is '1', it calculates the size of the square that can be formed from that cell.
- **Time Complexity**: \(O(3^{mn})\) in the worst case, where \(m\) is the number of rows and \(n\) is the number of columns.
- **Space Complexity**: \(O(m+n)\) due to the recursion stack.
- **Dry Run**: For a cell containing '1', it checks all three directions recursively until it hits boundaries or '0's, accumulating the size of the largest square found.

#### 2. `solveMem`
- **Code**: 
  ```cpp
  int solveMem(vector<vector<char>>& matrix, int i, int j, int& maxi, vector<vector<int>>& dp) {
      if (i >= matrix.size() || j >= matrix[0].size()) return 0;
      if (dp[i][j] != -1) return dp[i][j];
      int right = solveMem(matrix, i, j + 1, maxi, dp);
      int diag = solveMem(matrix, i + 1, j + 1, maxi, dp);
      int down = solveMem(matrix, i + 1, j, maxi, dp);
      if (matrix[i][j] == '1') {
          dp[i][j] = 1 + min(right, min(diag, down));
          maxi = max(maxi, dp[i][j]);
          return dp[i][j];
      } else {
          dp[i][j] = 0;
          return dp[i][j];
      }
  }
  ```
- **Explanation**: This function uses memoization to optimize the recursive approach. It caches the results in a `dp` table to avoid redundant calculations, significantly reducing the time complexity.
- **Time Complexity**: \(O(mn)\) due to the memoization of each cell.
- **Space Complexity**: \(O(mn)\) for the `dp` table and \(O(m+n)\) for the recursion stack.
- **Dry Run**: For each cell, it checks if the value is already computed; if not, it performs the same checks as in `solveRec`, storing results in the `dp` array.

#### 3. `solveTab`
- **Code**: 
  ```cpp
  int solveTab(vector<vector<char>>& matrix) {
      int row = matrix.size();
      int col = matrix[0].size();
      vector<vector<int>> dp(row + 1, vector<int>(col + 1, 0));
      int maxi = 0;
      for (int i = row - 1; i >= 0; i--) {
          for (int j = col - 1; j >= 0; j--) {
              int right = dp[i][j + 1];
              int diag = dp[i + 1][j + 1];
              int down = dp[i + 1][j];
              if (matrix[i][j] == '1') {
                  dp[i][j] = 1 + min(right, min(diag, down));
                  maxi = max(maxi, dp[i][j]);
              } else {
                  dp[i][j] = 0;
              }
          }
      }
      return maxi * maxi;
  }
  ```
- **Explanation**: This function employs a tabulation approach, filling up a 2D array from bottom-right to top-left, calculating the largest square that can be formed at each cell.
- **Time Complexity**: \(O(mn)\) since each cell is processed once.
- **Space Complexity**: \(O(mn)\) for the `dp` table.
- **Dry Run**: Iterating from the last row and column, it calculates potential square sizes based on the precomputed values of adjacent cells, updating the maximum square size.

#### 4. `solveSO`
- **Code**: 
  ```cpp
  int solveSO(vector<vector<char>>& matrix, int& maxi) {
      int row = matrix.size();
      int col = matrix[0].size();
      vector<int> curr(col + 1, 0);
      vector<int> next(col + 1, 0);
      for (int i = row - 1; i >= 0; i--) {
          for (int j = col - 1; j >= 0; j--) {
              int right = curr[j + 1];
              int diag = next[j + 1];
              int down = next[j];
              if (matrix[i][j] == '1') {
                  curr[j] = 1 + min(right, min(diag, down));
                  maxi = max(maxi, curr[j]);
              } else {
                  curr[j] = 0;
              }
          }
          next = curr;
      }
      return maxi * maxi;
  }
  ```
- **Explanation**: This function uses space optimization by only keeping track of the current and next rows, reducing the space complexity to \(O(n)\).
- **Time Complexity**: \(O(mn)\), as each cell is processed once.
- **Space Complexity**: \(O(n)\) for the two 1D arrays (`curr` and `next`).
- **Dry Run**: Similar to `solveTab`, but uses two arrays to save space. It calculates the potential square size for each '1', storing only necessary values for calculations.

#### 5. `maximalSquare`
- **Code**: 
  ```cpp
  int maximalSquare(vector<vector<char>>& matrix) {
      int m = matrix.size();
      int n = matrix[0].size();
      int maxi = 0;
      return solveSO(matrix, maxi);
  }
  ```
- **Explanation**: This is the main function that initializes the necessary variables and calls the optimized space solution function.
- **Time Complexity**: Depends on the called function; here it’s \(O(mn)\).
- **Space Complexity**: Space is determined by the called function, specifically \(O(n)\) in this case.
- **Dry Run**: Initializes `maxi` and passes control to `solveSO` to compute the maximal square area.

  ---
### Time Complexity Conclusion
- The overall time complexity for the algorithm is:
  \[
  O(n \times m)
  \]
This applies to all versions of the function:
- **Recursive Version**: While the recursive version is typically \( O(2^{(n+m)}) \) due to overlapping subproblems, it's generally less efficient than the others and not directly comparable here.
- **Memoization Version**: This is also \( O(n \times m) \) as each cell is computed once and stored.
- **Tabulation Version**: The same \( O(n \times m) \) as it fills up a DP table.
- **Space Optimized Version**: This maintains the same \( O(n \times m) \) time complexity for the same reasons.

---
# Further space optimisation🔥🔥
```cpp
int space_optimization1(int m, int n, vector<vector<int>>& mat) {
    int maxi = 0;
    for (int i = n - 1; i >= 0; i--) {
        for (int j = m - 1; j >= 0; j--) {
            int down = 0;
            int right = 0;
            int diag = 0;

            if (i < n - 1) {
                down = mat[i + 1][j]; // Value directly below
            }
            if (j < m - 1) {
                right = mat[i][j + 1]; // Value directly to the right
            }
            if (i < n - 1 && j < m - 1) {
                diag = mat[i + 1][j + 1]; // Value diagonally down-right
            }

            if (mat[i][j] == 1) { // If the current cell is 1
                // Update the current cell with the size of the square that can be formed
                mat[i][j] = 1 + min(down, min(right, diag));
                maxi = max(mat[i][j], maxi); // Update the maximum size found
            }
        }
    }
    return maxi;
}
```

### Changes Made:
- **Removed Ternary Operators**: I replaced the ternary conditions with `if` statements. Each variable (`down`, `right`, `diag`) is initialized to `0`, and its value is updated if the condition for its corresponding index is met.

### Explanation of the Modified Code:
- The logic and flow remain unchanged. Each variable is set to `0` by default, and if the indices are valid (i.e., within bounds), their values are updated accordingly.
- This way, the code avoids using ternary operators while keeping the same functionality and efficiency.

This modification makes the code more explicit, which may improve readability for some developers.
