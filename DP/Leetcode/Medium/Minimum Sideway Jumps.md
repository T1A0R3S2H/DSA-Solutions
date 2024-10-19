# 🔥🔥Recursion
```cpp
class Solution {
public:
    int solveRec(vector<int>& obstacles, int currLane, int currPos) {
        int n = obstacles.size() - 1; // because obs ki size is (n+1)
        
        // Base case: If we reached the last position
        if (currPos == n) return 0;
        
        // If there is no obstacle in the next position in the same lane, move forward
        if (obstacles[currPos + 1] != currLane) {
            return solveRec(obstacles, currLane, currPos + 1);
        }
        else {
            // Need to perform a side jump
            int ans = INT_MAX;
            for (int i = 1; i <= 3; i++) {
                if (currLane != i && obstacles[currPos] != i) {
                    ans = min(ans, 1 + solveRec(obstacles, i, currPos)); // 1 side jump
                }
            }
            return ans;
        }
    }

    int minSideJumps(vector<int>& obstacles) {
        return solveRec(obstacles, 2, 0); // Start from lane 2 at position 0
    }
};

```
# 🔥🔥Recursion + Memoisation (Top-down)
### Code

```cpp
class Solution {
public:
    int solveMem(vector<int>& obstacles, int currLane, int currPos, vector<vector<int>>& dp) {
        int n = obstacles.size() - 1; // because obs ki size is (n+1)
        if (currPos == n) return 0;
        if (dp[currLane][currPos] != -1) return dp[currLane][currPos];
        
        // Move forward in the same lane if there is no obstacle in the next position
        if (obstacles[currPos + 1] != currLane) {
            return solveMem(obstacles, currLane, currPos + 1, dp);
        }
        else {
            // If obstacle found, try switching lanes
            int ans = INT_MAX;
            // there are 3 lanes
            for (int i = 1; i <= 3; i++) {
                if (currLane != i && obstacles[currPos] != i) {
                    ans = min(ans, 1 + solveMem(obstacles, i, currPos, dp)); // 1 side jump
                }
            }
            dp[currLane][currPos] = ans;
            return dp[currLane][currPos];
        }
    }

    int minSideJumps(vector<int>& obstacles) {
        int n = obstacles.size();
        vector<vector<int>> dp(4, vector<int>(obstacles.size(), -1));
        return solveMem(obstacles, 2, 0, dp); // Start from lane 2 at position 0
    }
};
```

### Explanation
1. **Function `solveMem`:** This is a recursive function with memoization to calculate the minimum number of side jumps required for the frog to reach the end (`n`) starting from lane 2.
   - **Base Case:** If the frog reaches the last position (`currPos == n`), no more jumps are required, so the function returns `0`.
   - **Memoization Check:** If the value for the current lane and position is already calculated and stored in `dp`, the function directly returns that stored value.
   - **Moving Forward:** If there is no obstacle in the next position of the current lane, the function calls itself to move the frog forward to the next position (`currPos + 1`).
   - **Side Jump:** If there is an obstacle in the next position, the function tries all possible lanes and calculates the minimum number of jumps by adding 1 for each side jump.

2. **Function `minSideJumps`:** This function initializes the `dp` table with `-1` and calls the `solveMem` function starting from lane 2 and position 0.
3. The reason why the `dp` array has a size of `4` in the code (`vector<vector<int>> dp(4, vector<int>(obstacles.size(), -1));`) is that there are **3 lanes** (lanes 1, 2, and 3). The lanes are indexed from `1` to `3`, so we use a size of `4` to conveniently access the lanes using their respective index numbers.

Here's the breakdown:
- **Lane 1** corresponds to index `1` in the `dp` array.
- **Lane 2** corresponds to index `2` in the `dp` array.
- **Lane 3** corresponds to index `3` in the `dp` array.

By using a size of `4` for the first dimension of the `dp` array, you can directly use `1`, `2`, or `3` as the lane index without any offset, which simplifies the code and avoids confusion. Index `0` is not used but allocated to make the array 1-based for easier lane management.

So the extra row (`dp[0]`) is never accessed, but it simplifies indexing since lanes start from 1 instead of 0.

####  Example:
- If the frog is on **lane 2**, `dp[2][currPos]` stores the result for lane 2 at the current position.


### Time Complexity
- **Time Complexity:** \( O(3 \times n) \), where \( n \) is the number of positions (length of the `obstacles` array minus 1). For each position, the function checks all three lanes, leading to a linear complexity in terms of the number of positions.

### Space Complexity
- **Space Complexity:** \( O(3 \times n) \) for the `dp` table, which stores the results of the subproblems for each of the 3 lanes at every position. The recursion stack also takes space proportional to \( O(n) \).

### Dry Run
#### Input: `obstacles = [0, 1, 2, 3, 0]`
- The frog starts at lane 2, position 0.
- At position 1, lane 2 is clear, so it moves to position 2.
- At position 2, lane 2 is blocked by an obstacle, so it makes a side jump to lane 1.
- At position 3, lane 1 is blocked, so it jumps to lane 3.
- At position 4, lane 3 is clear, so it moves forward.
- Total side jumps = 2.

#### Input: `obstacles = [0, 1, 1, 3, 3, 0]`
- Starting at lane 2, there are no obstacles in lane 2 for the entire path, so the frog doesn’t need any side jumps.
- Total side jumps = 0.

# 🔥🔥Tabulation (Bottom Up)
