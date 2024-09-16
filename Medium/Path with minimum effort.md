## **Code:**
```c
class Solution {
public:
    // Directions array to move up, down, left, and right
    vector<pair<int, int>> directions={{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    bool dfs(vector<vector<int>>& heights, vector<vector<bool>>& visited, int maxEffort, int x, int y) {
        int m=heights.size();
        int n=heights[0].size();
        // If we've reached the bottom-right corner, return true
        if (x==m-1 && y==n-1) return true;
        visited[x][y]=true;
        // Traverse all four directions
        for (auto& dir:directions) {
            int newX=x+dir.first;
            int newY=y+dir.second;
            
            // Check if the new coordinates are within bounds
            if (newX>=0 && newX<m && newY>=0 && newY<n && !visited[newX][newY]) {
                int diff=abs(heights[newX][newY]-heights[x][y]);
                // current height difference is within the maxEffort limit, continue the DFS
                if (diff<=maxEffort) {
                    if (dfs(heights, visited, maxEffort, newX, newY)) return true;
                }
            }
        }
        return false;
    }
    
    int minimumEffortPath(vector<vector<int>>& heights) {
        int m=heights.size();
        int n=heights[0].size();
        // binary search based approach

        // define the range
        int left=0;
        int right=1e6;
        
        while (left<right) {
            int mid=(left+right)/2;
            vector<vector<bool>> visited(m, vector<bool>(n, false));
            // If DFS finds a path with the current maximum effort, decrease the right boundary
            if (dfs(heights, visited, mid, 0, 0)) {
                right=mid;
            } 
            else {
                left=mid+1;
            }
        }
        return left;
    }
};

```

## **Explanation**

### **Problem Statement**
You are given a 2D grid `heights`, where each cell `(i, j)` represents the height of that location. You need to find a path from the top-left corner `(0, 0)` to the bottom-right corner `(m-1, n-1)` such that the **maximum absolute height difference** between consecutive cells in the path is minimized.

### **Approach Overview**
We solve the problem by using **binary search on the effort** (maximum height difference) and **DFS** to check the feasibility of a path for a given effort.

### **Steps**:

1. **Effort as Search Space**:
   - The **effort** is defined as the maximum absolute difference between the heights of two adjacent cells on the path.
   - The effort can range from `0` (if all cells have the same height) to `1e6` (based on the problem constraints where the maximum height can be `1e6`).

2. **Binary Search on Effort**:
   - We aim to find the **minimum possible effort** using binary search.
   - We define the search space as `left = 0` and `right = 1e6`. For each mid-point in the binary search, we check if there exists a path from `(0, 0)` to `(m-1, n-1)` where the maximum height difference does not exceed the mid-point value (`mid`).

3. **DFS for Path Validation**:
   - For each value of `mid` (effort) during the binary search, we use **DFS** to explore the grid from the top-left corner to the bottom-right corner.
   - In the DFS, we can only move to adjacent cells if the height difference between the current cell and the adjacent cell is less than or equal to the current effort (`mid`).
   - If the DFS successfully reaches the bottom-right corner, it means that a path exists with the current effort, and we attempt to reduce the effort further by adjusting the binary search range.
   - If DFS fails to find a valid path, we increase the effort by adjusting the binary search range.

4. **Termination**:
   - The binary search continues until the `left` and `right` pointers converge, at which point `left` will contain the minimum possible effort.


5. 
![image](https://github.com/user-attachments/assets/65d36618-8ddc-4141-b716-df3cc1be52a6)

---

## **Time Complexity**

1. **DFS Complexity**:
   - In the worst case, the DFS explores every cell of the grid exactly once during each binary search iteration.
   - Since there are `m * n` cells in total, a single DFS takes `O(m * n)`.

2. **Binary Search Complexity**:
   - The binary search is conducted on the possible values of the effort. The effort can range from `0` to `1e6`, so the binary search will perform `O(log(1e6))` iterations, which simplifies to `O(20)`.

3. **Total Time Complexity**:
   - In each binary search iteration, we perform a DFS. Therefore, the total time complexity is:
     \[
     O(m \times n \times \log(1e6)) = O(m \times n \times 20) = O(m \times n)
     \]
   - This is a highly efficient approach given that we are checking all cells using binary search and DFS combined.

---

## **Space Complexity**

1. **Visited Array**:
   - During each DFS call, we maintain a `visited` array of size `m x n` to track which cells have been visited. This takes `O(m \times n)` space.

2. **DFS Call Stack**:
   - DFS is implemented recursively, and in the worst case, the recursion depth can reach the total number of cells, which is `m * n`. Hence, the call stack can take up to `O(m \times n)` space.

3. **Total Space Complexity**:
   - The total space used by the visited array and the DFS call stack is `O(m \times n)`.

---

## **Dry Run**

### Input:
```cpp
heights = [[1, 2, 2],
           [3, 8, 2],
           [5, 3, 5]]
```

### Binary Search Initialization:
- **Left**: `0`, **Right**: `1e6`, **Mid**: `500`

### Step 1: Test with Mid-Point Effort (500)

1. Start DFS at `(0, 0)` (height `1`).
2. Move right to `(0, 1)` (height `2`), effort `|2 - 1| = 1`, which is less than `500`. Continue.
3. Move right to `(0, 2)` (height `2`), effort `|2 - 2| = 0`. Continue.
4. Move down to `(1, 2)` (height `2`), effort `|2 - 2| = 0`. Continue.
5. Move down to `(2, 2)` (height `5`), effort `|5 - 2| = 3`, which is less than `500`. We have reached the destination `(2, 2)`.
6. Since DFS succeeded, a valid path exists with effort `500`. Update the binary search range: **Right** becomes `500`.

### Step 2: Test with Mid-Point Effort (250)

1. Perform DFS with `mid = 250`. The path `(0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2)` works again, as the maximum effort (3) is still less than `250`.
2. A valid path exists with effort `250`. Update the binary search range: **Right** becomes `250`.

### Step 3: Further Binary Search

1. The binary search continues, halving the search space, and testing mid-points (e.g., 125, 62, etc.).
2. Eventually, the binary search narrows down to an effort of **2**, which is the minimum effort required to reach the destination.
3. The final path has an effort of 2.

### Final Output:
The minimum effort is `2`.

---

## **Summary**

- **Explanation**: We perform a binary search on the effort and use DFS to check the feasibility of paths with each mid-point effort.
- **Time Complexity**: `O(m * n)` due to `O(m * n)` DFS calls within `O(log(1e6))` binary search iterations.
- **Space Complexity**: `O(m * n)` for the visited array and the DFS call stack.
- **Dry Run**: Shows how binary search progressively narrows the effort and how DFS validates paths for each mid-point effort.

This detailed ETSD outlines how your approach works efficiently for the problem.
