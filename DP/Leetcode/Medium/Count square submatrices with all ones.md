### Code
```cpp
class Solution {
public:
    int countSquares(vector<vector<int>>& matrix) {
        int n = matrix.size();
        int m = matrix[0].size();
        vector<vector<int>> dp(n, vector<int>(m, 0));

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 1) {
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1; 
                    } 
                    else {
                        dp[i][j] = 1 + min({dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]});
                    }
                }
            }
        }

        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                ans += dp[i][j];
            }
        }
        return ans;
    }
};
```

### Explanation
1. **Goal**: The task is to count all square submatrices filled with ones. Each submatrix must end at cell `(i, j)` and have only ones.
   
2. **Dynamic Programming (DP) Definition**:
   - Define `dp[i][j]` as the side length of the largest square submatrix ending at cell `(i, j)` with all `1`s.
   
3. **DP Transition**:
   - For cells with `matrix[i][j] == 1`, if the cell is on the top row (`i == 0`) or the left column (`j == 0`), it can only form a `1x1` square. So, `dp[i][j] = 1`.
   - Otherwise, `dp[i][j]` is `1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])`. This formula ensures that:
     - We extend the smallest square possible from one of the three adjacent cells (`top`, `left`, or `top-left`).
   - For cells where `matrix[i][j] == 0`, `dp[i][j] = 0`.

4. **Summing Up**:
   - The total count of squares is the sum of all values in `dp`, where each `dp[i][j]` represents the count of square submatrices that can end at that cell.

### Time Complexity
- **O(n * m)**: We iterate over each cell in the matrix twice—once to fill the `dp` array and once to sum up its values.

### Space Complexity
- **O(n * m)**: A `dp` array of the same size as `matrix` is used to store the maximum square lengths.

### Dry Run

#### Example Matrix:
Given matrix:
\[
\text{matrix} = 
\begin{bmatrix}
0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 \\
0 & 1 & 1 & 1 \\
\end{bmatrix}
\]

#### Step-by-Step Calculation of DP Array:
1. **Initialization**:
   - **`dp` array** initially as a `3x4` matrix filled with zeroes.

2. **Filling `dp` Array**:
   - **Row 0**:
     - `(0,0)`: `matrix[0][0] = 0`, so `dp[0][0] = 0`.
     - `(0,1)`: `matrix[0][1] = 1`, so `dp[0][1] = 1`.
     - `(0,2)`: `matrix[0][2] = 1`, so `dp[0][2] = 1`.
     - `(0,3)`: `matrix[0][3] = 1`, so `dp[0][3] = 1`.

     Result after row 0:
     \[
     \text{dp} = 
     \begin{bmatrix}
     0 & 1 & 1 & 1 \\
     0 & 0 & 0 & 0 \\
     0 & 0 & 0 & 0 \\
     \end{bmatrix}
     \]

   - **Row 1**:
     - `(1,0)`: `matrix[1][0] = 1`, so `dp[1][0] = 1`.
     - `(1,1)`: `matrix[1][1] = 1`, `dp[1][1] = 1 + min(dp[0][1], dp[1][0], dp[0][0]) = 1 + min(1, 1, 0) = 1`.
     - `(1,2)`: `matrix[1][2] = 1`, `dp[1][2] = 1 + min(dp[0][2], dp[1][1], dp[0][1]) = 1 + min(1, 1, 1) = 2`.
     - `(1,3)`: `matrix[1][3] = 1`, `dp[1][3] = 1 + min(dp[0][3], dp[1][2], dp[0][2]) = 1 + min(1, 2, 1) = 2`.

     Result after row 1:
     \[
     \text{dp} = 
     \begin{bmatrix}
     0 & 1 & 1 & 1 \\
     1 & 1 & 2 & 2 \\
     0 & 0 & 0 & 0 \\
     \end{bmatrix}
     \]

   - **Row 2**:
     - `(2,0)`: `matrix[2][0] = 0`, so `dp[2][0] = 0`.
     - `(2,1)`: `matrix[2][1] = 1`, `dp[2][1] = 1 + min(dp[1][1], dp[2][0], dp[1][0]) = 1 + min(1, 0, 1) = 1`.
     - `(2,2)`: `matrix[2][2] = 1`, `dp[2][2] = 1 + min(dp[1][2], dp[2][1], dp[1][1]) = 1 + min(2, 1, 1) = 2`.
     - `(2,3)`: `matrix[2][3] = 1`, `dp[2][3] = 1 + min(dp[1][3], dp[2][2], dp[1][2]) = 1 + min(2, 2, 2) = 3`.

     Final `dp` array:
     \[
     \text{dp} = 
     \begin{bmatrix}
     0 & 1 & 1 & 1 \\
     1 & 1 & 2 & 2 \\
     0 & 1 & 2 & 3 \\
     \end{bmatrix}
     \]

3. **Summing Up**:
   - Total count of squares is the sum of all elements in `dp`:
     \[
     0 + 1 + 1 + 1 + 1 + 1 + 2 + 2 + 0 + 1 + 2 + 3 = 15
     \]

### Final Answer
The total number of square submatrices with all ones is **15**.
