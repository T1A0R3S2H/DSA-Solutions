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
        int m = matrix.size();
        int n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        int maxi = 0;
        for(int i = m - 1; i >= 0; i--){
            for(int j = n - 1; j >= 0; j--){
                if(matrix[i][j] == '1'){
                    if(i < m - 1 && j < n - 1)
                        dp[i][j] = 1 + min(dp[i+1][j], min(dp[i][j+1], dp[i+1][j+1]));
                    else
                        dp[i][j] = 1; // Base case for boundary cells
                    maxi = max(maxi, dp[i][j]);
                }
            }
        }
        return maxi * maxi;
    }

    int maximalSquare(vector<vector<char>>& matrix) {
        int m=matrix.size();
        int n=matrix[0].size();
        int maxi=0;
        vector<vector<int>>dp(m, vector<int>(n, -1));
        solveMem(matrix, 0, 0, maxi, dp);
        return maxi*maxi;
    }
};
```

### Explanation

The problem aims to find the largest square containing only '1's in a given binary matrix and return its area.

- **`solveRec`**: This function recursively explores every cell in the matrix to find the size of the largest square. It checks three possible directions: right, diagonal, and down. If the current cell is '1', it calculates the minimum square size that can be formed using those three directions. The largest square size (`maxi`) is updated during the recursion. Base cases occur when the indices go out of bounds.
  
- **`solveMem`**: This is a dynamic programming version of the recursive solution using memoization (`dp` table). It avoids recalculating the results of subproblems by storing them in the `dp` table. Like `solveRec`, it calculates the size of the square for each cell but checks the memoized result before recursion.
  
- **`solveTab`**: This function uses a bottom-up approach with tabulation to fill the `dp` table. For each cell containing '1', it calculates the size of the square that can be formed at that point, considering the right, down, and diagonal cells. The `maxi` variable stores the largest square size found during the traversal. The area is then returned by squaring `maxi`.
  
- **`maximalSquare`**: This function initiates the solution, either by calling `solveMem` or `solveRec`. It returns the area of the largest square, calculated as `maxi * maxi`.

### Time Complexity

- **Recursive Solution (`solveRec`)**: The time complexity of the recursive solution is exponential due to overlapping subproblems. Each cell can potentially lead to multiple recursive calls.
  - **Time Complexity**: \(O(3^{m \times n})\), where \(m\) is the number of rows and \(n\) is the number of columns.
  
- **Memoization Solution (`solveMem`)**: With memoization, the overlapping subproblems are avoided by storing the results of each subproblem in the `dp` table.
  - **Time Complexity**: \(O(m \times n)\) because every cell is computed only once.

- **Tabulation Solution (`solveTab`)**: The bottom-up approach iterates over all the cells in the matrix, calculating the square size for each cell once.
  - **Time Complexity**: \(O(m \times n)\), where \(m\) is the number of rows and \(n\) is the number of columns.

### Space Complexity

- **Recursive Solution (`solveRec`)**: The recursive solution uses stack space proportional to the number of recursive calls. There is no extra storage besides that.
  - **Space Complexity**: \(O(m + n)\) due to recursion stack depth.

- **Memoization Solution (`solveMem`)**: The memoization solution requires additional space for the `dp` table, which is of size \(m \times n\).
  - **Space Complexity**: \(O(m \times n)\) for the `dp` table.

- **Tabulation Solution (`solveTab`)**: The tabulation method also requires a `dp` table of size \(m \times n\).
  - **Space Complexity**: \(O(m \times n)\) for the `dp` table.

### Dry Run

Let’s consider the following example:

#### Input:
```cpp
matrix = [["1","0","1","0","0"],
          ["1","0","1","1","1"],
          ["1","1","1","1","1"],
          ["1","0","0","1","0"]]
```

#### Dry Run of `solveTab`:

- Start at cell (3,0): `matrix[3][0] == '1'`, so `dp[3][0] = 1`.
- Continue moving left to right, updating the `dp` table. For example, `dp[2][2] = 2`, meaning a 2x2 square can be formed there.
- After processing all the cells, `maxi = 2`, indicating the largest square is 2x2.
  
#### Output:
```cpp
4
```
