
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

    int maximalSquare(vector<vector<char>>& matrix) {
        int m=matrix.size();
        int n=matrix[0].size();
        int maxi=0;
        return solveTab(matrix);
    }
};
```

### Explanation
The solution employs three methods: recursive (`solveRec`), memoized (`solveMem`), and tabulated (`solveTab`). The main goal is to find the largest square containing only '1's in a binary matrix and return its area.

- **Recursive Method (`solveRec`)**: This method explores the matrix recursively. For each cell that contains '1', it checks the size of squares that can be formed by moving right, diagonally, and downward. The maximum size found is updated in the `maxi` variable.

- **Memoization Method (`solveMem`)**: This optimizes the recursive approach by storing the results of subproblems in a `dp` array to avoid recomputation. It works similarly to the recursive method but checks the `dp` array before proceeding with calculations.

- **Tabulation Method (`solveTab`)**: This is the bottom-up dynamic programming approach. It uses a 2D array (`dp`) where `dp[i][j]` stores the size of the largest square whose bottom-right corner is at cell `(i, j)`. The size is determined by the minimum size of squares formed by the adjacent right, down, and diagonal cells.

### Time Complexity
- The time complexity for the tabulated approach is **O(m * n)**, where `m` is the number of rows and `n` is the number of columns in the matrix. This is because we traverse each cell once.

### Space Complexity
- The space complexity is **O(m * n)** due to the additional `dp` array used for storing intermediate results in the tabulated approach. If using only the recursive or memoization approach, it would be **O(m * n)** for the memoization array but could be optimized to **O(n)** if only the last row of results is stored.

### Dry Run
Let’s dry run the code with an example:

**Input**:
```
matrix = [["1","0","1","0","0"],
          ["1","0","1","1","1"],
          ["1","1","1","1","1"],
          ["1","0","0","1","0"]]
```

1. **Tabulation Execution**:
   - Start from the bottom-right corner of the matrix.
   - Fill the `dp` array according to the conditions.
   - For cell `(3,4)`: It's '0', so `dp[3][4] = 0`.
   - For cell `(3,3)`: It's '1', hence `dp[3][3] = 1` (minimum of right, down, and diagonal is 0).
   - Continue this way until reaching the top-left cell.
   - The maximum value in the `dp` array is updated as we fill it.

2. The final `dp` array would look something like this:
```
   dp = [[0, 0, 1, 0, 0],
         [1, 0, 1, 1, 1],
         [1, 1, 2, 2, 2],
         [1, 0, 0, 1, 0]]
```
   - The maximum square size is `2`, so the output will be `2 * 2 = 4`.

This process successfully computes the area of the largest square containing '1's.
