### Code
```class Solution {
public:
    void dfs(vector<vector<int>>& a, int i, int j, int c, int val) {
        if (i<0 || i>=a.size() || j<0 || j>=a[0].size() || a[i][j]==c || a[i][j]!=val){
                return;
            }
        a[i][j]=c;
        dfs(a, i-1, j, c, val);
        dfs(a, i, j-1, c, val);
        dfs(a, i+1, j, c, val);
        dfs(a, i, j+1, c, val);
    }
    vector<vector<int>> floodFill(vector<vector<int>>& a, int sr, int sc, int c) {
        dfs(a, sr, sc, c, a[sr][sc]);
        return a;
    }
};
```
### Explanation:

The problem you're solving is a classic depth-first search (DFS) problem. The goal is to fill all connected cells (4-directionally) that have the same starting pixel value with a new color. Here's how the algorithm works:

1. **Input:** You are given a 2D grid `a` of size `m x n`, starting coordinates `(sr, sc)`, and a target color `c`. You need to flood-fill all connected pixels that share the same color as the starting pixel `a[sr][sc]` and replace them with the new color `c`.

2. **DFS:** You use DFS to traverse the grid from the starting pixel. At each step, if a neighboring pixel has the same color as the starting pixel and hasn't already been filled with the new color, the DFS will continue to that neighbor. The base case stops the recursion if the current pixel is out of bounds or doesn't meet the conditions.

### Algorithm Breakdown:
- **dfs function**: This function is responsible for the recursive traversal. It checks boundary conditions (out of bounds), whether the pixel has already been filled, and whether the current pixel has the same value as the starting pixel.
- **floodFill function**: It initiates the DFS with the starting pixel and returns the modified image.

### Dry Run:

Let's dry-run the solution with the following example:

#### Input:
```
image = [[1, 1, 1], 
         [1, 1, 0], 
         [1, 0, 1]], 
sr = 1, sc = 1, color = 2
```

- The starting pixel is `(1, 1)` and has the color `1`.
- The flood fill should replace all connected `1` pixels with `2`.

#### Step-by-step:
1. The initial pixel is `(1, 1)`. The color is `1`, so we replace it with `2`. The grid becomes:
   ```
   [[1, 1, 1], 
    [1, 2, 0], 
    [1, 0, 1]]
   ```
2. We apply DFS to the top neighbor `(0, 1)`. It is also `1`, so we replace it with `2`:
   ```
   [[1, 2, 1], 
    [1, 2, 0], 
    [1, 0, 1]]
   ```
3. From `(0, 1)`, check `(0, 0)`. It is `1`, so we replace it with `2`:
   ```
   [[2, 2, 1], 
    [1, 2, 0], 
    [1, 0, 1]]
   ```
4. Now check the other neighbors of `(0, 1)`, but they either are out of bounds or already filled.
5. Continue checking all connected pixels from the remaining DFS calls, including `(1, 0)` and `(2, 0)`, eventually filling the entire connected region.

Final result:
```
[[2, 2, 2], 
 [2, 2, 0], 
 [2, 0, 1]]
```

### Time Complexity:

- The algorithm performs a DFS traversal of the image, visiting each pixel at most once. In the worst case, all pixels are the same color as the starting pixel, so we visit each pixel once.
- The time complexity is **O(m * n)**, where `m` is the number of rows and `n` is the number of columns in the image.

### Space Complexity:

- The space complexity is determined by the recursion depth, which in the worst case is equal to the total number of pixels in the image.
- The space complexity is **O(m * n)** for the recursion stack if the entire grid is one connected component.
- Additionally, the input image is modified in-place, so no extra space is used aside from the recursion stack.

### Summary:

- **Time Complexity:** O(m * n)
- **Space Complexity:** O(m * n)
