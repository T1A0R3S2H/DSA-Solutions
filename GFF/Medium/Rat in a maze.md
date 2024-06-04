## Approach
- The approach used in this solution is a backtracking algorithm, which is a general algorithmic technique that considers searching every possible combination to solve a computational problem. The problem at hand is to find all possible paths from the top-left corner (0, 0) to the bottom-right corner (n-1, n-1) of a given n x n maze, where each cell is either blocked (0) or unblocked (1).

- The solution starts by initializing an empty vector `res` to store all the valid paths. If the starting cell (0, 0) is blocked, the function returns an empty vector since no path is possible. Otherwise, it calls a recursive helper function `backtrack` with the starting cell coordinates (0, 0), an empty string `""` representing the current path, and the `res` vector to store the valid paths.

- The `backtrack` function follows a depth-first search approach to explore all possible paths. At each cell, it checks if the current cell is the destination cell (n-1, n-1). If so, it adds the current path string `s` to the `res` vector and returns. If not, it marks the current cell as visited by setting its value to 0 in the maze matrix `m`.

- Then, the function recursively explores all valid neighboring cells (up, right, down, left) by calling itself with updated coordinates and an updated path string `s` appended with the respective direction ("U" for up, "R" for right, "D" for down, and "L" for left). After exploring all possible paths from the current cell, it restores the original value of the cell by setting `m[i][j]` back to 1, effectively unmarking the cell.

- Once the `backtrack` function finishes exploring all possible paths from the starting cell, the `res` vector contains all valid paths. The solution then sorts the `res` vector in lexicographical order using `std::sort` and returns it.

- The time complexity of this solution is O(3^(n^2)), where n is the size of the maze. In the worst case, where all cells are unblocked, the backtracking algorithm needs to explore all possible paths, which can be up to 3^(n^2) (three choices for each cell: right, down, or backtrack). However, in practice, the time complexity is often less than the worst case due to pruning of invalid paths and early termination.

- The space complexity of the solution is O(n^2), which is the maximum depth of the recursion stack. In the worst case, when all cells are unblocked, the recursion stack can grow up to n^2 levels, representing the maximum length of a path in the maze.

- It's important to note that this solution assumes that the rat can move only in the right or down direction at any given point, and it finds all possible paths satisfying this constraint. If the problem requires finding the shortest path or handling additional constraints, the solution may need to be modified accordingly.
##
```bash
#include <vector>
#include <string>
#include <algorithm>

class Solution {
public:
        vector<string> findPath(vector<vector<int>>& m, int n) {
        vector<string> res;
        if (m[0][0] == 0) return res;

        backtrack(m, n, 0, 0, "", res);
        sort(res.begin(), res.end());

        return res;
    }

private:
    void backtrack(vector<vector<int>>& m, int n, int i, int j, string s, vector<string>& res) {
        
        // base case
        if (i == n - 1 && j == n - 1) {
            res.push_back(s);
            return;
        }

        // visited matrix
        m[i][j] = 0;
        
        if (i > 0 && m[i - 1][j] == 1) {
            backtrack(m, n, i - 1, j, s + "U", res);
        }
        if (j < n - 1 && m[i][j + 1] == 1) {
            backtrack(m, n, i, j + 1, s + "R", res);
        }
        if (i < n - 1 && m[i + 1][j] == 1) {
            backtrack(m, n, i + 1, j, s + "D", res);
        }
        if (j > 0 && m[i][j - 1] == 1) {
            backtrack(m, n, i, j - 1, s + "L", res);
        }
        
        
        // once all possibilities are done
        m[i][j] = 1;
    }
};
```


