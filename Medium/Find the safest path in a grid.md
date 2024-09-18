
### **Code**:
```cpp
class Solution {
public:
    vector<int> rowDir={-1, 1, 0, 0};
    vector<int> colDir={0, 0, -1, 1};
    // Check if the current cell is within bounds and not visited
    bool isValid(vector<vector<bool>>& visited, int i, int j, int n) {
        return i>=0 && j>=0 && i<n && j<n && !visited[i][j];
    }
    
    // Check if there is a safe path with a given safeness factor
    bool isSafe(vector<vector<int>>& distToThief, int safeDist) {
        int n=distToThief.size();
        queue<pair<int, int>> q;
        if (distToThief[0][0]<safeDist) return false; // Start point must be safe
        q.push({0, 0});
        vector<vector<bool>> visited(n, vector<bool>(n, false));
        visited[0][0]=true;
        
        while (!q.empty()) {
            auto curr=q.front();
            q.pop();
            int currRow=curr.first;
            int currCol=curr.second;
            
            // Check if we've reached the bottom-right corner
            if (currRow==n-1 && currCol==n-1) return true;
            
            // Explore all 4 possible directions
            for (int dir=0; dir<4; dir++) {
                int newRow=currRow+rowDir[dir];
                int newCol=currCol +colDir[dir];
                
                // Ensure new cell is valid and not visited
                if (isValid(visited, newRow, newCol, n)) {
                    // Skip if the new cell is not safe enough
                    if (distToThief[newRow][newCol]<safeDist) continue;
                    
                    // Mark as visited and push to the queue for further exploration
                    visited[newRow][newCol]=true;
                    q.push({newRow, newCol});
                }
            }
        }
        return false; // No valid path found
    }
    
    // Function to calculate maximum safeness factor
    int maximumSafenessFactor(vector<vector<int>>& grid) {
        int n=grid.size();
        queue<pair<int, int>> q;
        vector<vector<bool>> visited(n, vector<bool>(n, false));
        vector<vector<int>> distToThief(n, vector<int>(n, INT_MAX)); // To store distance to nearest thief
        
        // Initialize the queue with all thief positions and set their distance to 0
        for (int i=0; i<n; i++) {
            for (int j=0; j<n; j++) {
                if (grid[i][j]==1) {
                    visited[i][j]=true;
                    q.push({i, j});
                }
            }
        }
        
        // Perform BFS from the positions of thieves to calculate distance to nearest thief
        int dist=0;
        while (!q.empty()) {
            int size=q.size();
            while (size-- > 0) {
                auto curr=q.front();
                q.pop();
                int currRow=curr.first;
                int currCol=curr.second;
                distToThief[currRow][currCol]=dist;
                
                // Explore all 4 directions from the current cell
                for (int dir=0; dir<4; dir++) {
                    int newRow=currRow+rowDir[dir];
                    int newCol=currCol+colDir[dir];
                    
                    // If the new cell is valid and not visited
                    if (isValid(visited, newRow, newCol, n)) {
                        visited[newRow][newCol]=true;
                        q.push({newRow, newCol});
                    }
                }
            }
            dist++; // Increment the distance after processing the current level
        }
        
        // Binary search to find the maximum safeness factor
        int low=0, high=dist;
        int ans=0;
        while (low<=high) {
            int mid=low+(high-low)/2;
            if (isSafe(distToThief, mid)) {
                ans=mid; // If it's safe, try to increase safeness factor
                low=mid+1;
            } 
            else {
                high=mid-1; // If not safe, decrease the safeness factor
            }
        }
        
        return ans; // Return the maximum safeness factor found
    }
};

```

### **Explanation**:
This C++ solution aims to find the maximum "safeness factor" on a grid where `1` represents a thief's position, and `0` represents an empty cell. The safeness factor is the minimum distance from any cell on a path to the nearest thief. The problem can be broken down into two steps:

1. **Step 1 - BFS to Calculate Distance to Nearest Thief:**
   - The first BFS calculates the shortest distance of each cell to the nearest thief. Starting from all thief positions, BFS fills a 2D array `distToThief` where each cell contains the shortest distance to any thief.
   
2. **Step 2 - Binary Search for Maximum Safeness Factor:**
   - A binary search is applied on the safeness factor (minimum distance from the path to the nearest thief) to find the highest possible safeness factor for a path from the top-left corner to the bottom-right corner. The BFS checks if a path exists such that all cells in the path have a distance to the nearest thief that is greater than or equal to the current safeness factor (determined by binary search).

---

### **Time Complexity**:

1. **BFS for Distance Calculation**:
   - The BFS explores every cell once, checking each of the 4 possible directions (up, down, left, right) from each cell.
   - For a grid of size `n x n`, this BFS has a time complexity of **O(n²)**.

2. **Binary Search for Maximum Safeness Factor**:
   - The binary search range for the safeness factor is based on the maximum distance calculated in the BFS. Since the distance can range from `0` to `n` (maximum possible distance on the grid), the binary search runs in **O(log n)** steps.
   - For each middle value in the binary search, another BFS is performed to check if a safe path exists. The BFS takes **O(n²)** time.

Thus, the overall time complexity is:
- **O(n² + log n * n²) ≈ O(n² log n)**.

---

### **Space Complexity**:
- **O(n²)** for the `distToThief` 2D array to store the distance from each cell to the nearest thief.
- **O(n²)** for the `visited` array to track which cells have already been processed in the BFS.
- **O(n²)** for the queue used in BFS (in the worst case, it could contain all cells).

So the overall space complexity is:
- **O(n²)**.

---

### **Dry Run**:

Let's run a **dry example** on a `3x3` grid with two thieves:

```
Grid:
0 1 0
0 0 0
0 1 0
```

**Step 1 - BFS to Calculate Distance to Nearest Thief:**

- Start BFS from thief positions `(0,1)` and `(2,1)`. Initially, the distance of these cells is set to `0`, and other cells have `∞` (or a large number):
  
```
distToThief:
∞  0  ∞
∞  ∞  ∞
∞  0  ∞
```

- First BFS iteration from `(0,1)` and `(2,1)` updates adjacent cells:

```
distToThief:
1  0  1
∞  1  ∞
1  0  1
```

- Continue BFS to fill in all cells:

```
distToThief:
1  0  1
2  1  2
1  0  1
```

**Step 2 - Binary Search for Maximum Safeness Factor:**

- We now run a binary search to find the maximum safeness factor.
  - Start with `low=0` and `high=2` (max distance).
  - Check the middle value `mid=1`.
  
**Check for safeness factor 1**:
- Start BFS from `(0,0)` and check if all cells on the path from `(0,0)` to `(2,2)` have a distance to the nearest thief `≥ 1`.
- BFS successfully finds a path that satisfies the condition: `(0,0) -> (1,1) -> (2,2)`.

Since `mid=1` is valid, try to increase safeness factor by setting `low=2`.

**Check for safeness factor 2**:
- Start BFS from `(0,0)` and check if all cells on the path have distance to the nearest thief `≥ 2`.
- No valid path exists for this safeness factor.

So the maximum safeness factor is **1**.

---

### **Conclusion**:
The algorithm efficiently calculates the safest path in a grid filled with thieves by leveraging BFS to compute the minimum distance to thieves and binary search to find the optimal safeness factor.
