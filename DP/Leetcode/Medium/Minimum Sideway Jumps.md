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
### Code

```cpp
int solveTab(vector<int>& obstacles) {
    int n = obstacles.size() - 1;

    // Initialize dp table with large values (infinity-like), representing unreachable states
    vector<vector<int>> dp(4, vector<int>(n + 1, 1e9));

    // At the last position, no jumps are needed
    dp[1][n] = 0;
    dp[2][n] = 0;
    dp[3][n] = 0;

    // Traverse from the second-to-last position (n-1) to the start (0)
    for (int currPos = n - 1; currPos >= 0; currPos--) {
        for (int currLane = 1; currLane <= 3; currLane++) {
            if (obstacles[currPos + 1] != currLane) {
                // Move forward if there's no obstacle in the next position of the same lane
                dp[currLane][currPos] = dp[currLane][currPos + 1];
            } else {
                // Need to make a side jump to a different lane
                int ans = 1e9; // Initialize the answer with a large value
                for (int i = 1; i <= 3; i++) {
                    if (currLane != i && obstacles[currPos] != i) {
                        ans = min(ans, 1 + dp[i][currPos + 1]); // Consider side jumps
                    }
                }
                dp[currLane][currPos] = ans; // Update the dp table with the minimum jumps required
            }
        }
    }

    // The frog starts at lane 2, but we need to check all possible starting jumps
    return min(dp[2][0], min(1 + dp[1][0], 1 + dp[3][0]));
}
```

### Explanation
1. **Initialization:**
   - The `dp` table is initialized with values of `1e9` (representing an infinite number of jumps) because we are looking for the minimum number of side jumps.
   - At the final position (`n`), no jumps are required, so `dp[1][n] = dp[2][n] = dp[3][n] = 0` because the frog can reach the end in any lane with 0 jumps.

2. **Iterative Approach (Bottom-Up):**
   - We iterate from the second-to-last position (`n-1`) backward to the start (`0`), filling in the `dp` table as we go.
   - For each position `currPos` and lane `currLane`, we check if there is an obstacle in the next position. If there isn't, the frog can move forward in the same lane.
   - If there's an obstacle, the frog must make a side jump to another lane. We then check the possible lanes and calculate the minimum jumps needed by adding `1` for each side jump.

3. **Final Answer:**
   - The frog starts at lane 2, but since it may need to jump sideways at the start, the final answer is the minimum of:
     - Staying in lane 2 (`dp[2][0]`).
     - Jumping from lane 1 (`1 + dp[1][0]`).
     - Jumping from lane 3 (`1 + dp[3][0]`).

### Time Complexity
- **Time Complexity:** \( O(3 \times n) \), where \( n \) is the length of the `obstacles` array. For each of the `n` positions, we iterate over the 3 lanes, leading to a time complexity of \( O(n) \).

### Space Complexity
- **Space Complexity:** \( O(3 \times n) \), because we use a `dp` table of size \( 3 \times (n + 1) \) to store the minimum side jumps for each position and lane.

### Dry Run
#### Input: `obstacles = [0, 1, 2, 3, 0]`
- **Initialization:** `dp[1][n] = dp[2][n] = dp[3][n] = 0`
- **At position 3:** Lane 2 is blocked, so a side jump to lane 1 or lane 3 is considered.
  - `dp[1][3] = 1 + dp[3][4]` (side jump to lane 3 at position 4).
  - `dp[3][3] = 1 + dp[1][4]` (side jump to lane 1 at position 4).
- **At position 0:** The frog starts in lane 2, and based on the calculated `dp`, the minimum jumps needed is `2`.

# 🔥🔥Space Optimised approach
### Code

```cpp
int solveSO(vector<int>& obstacles) {
    int n = obstacles.size();
    
    // Initialize the two arrays for current and next positions, representing the lanes.
    vector<int> curr(4, 1e9);  // Tracks the minimum jumps at the current position.
    vector<int> next(4, 1e9);  // Tracks the minimum jumps at the next position.

    // At the last position (n), no jumps are needed
    next[1] = 0;
    next[2] = 0;
    next[3] = 0;

    // Iterate from the second-to-last position back to the start
    for (int pos = n - 2; pos >= 0; pos--) {
        for (int lane = 1; lane <= 3; lane++) {
            if (obstacles[pos + 1] != lane) {
                // If there is no obstacle in the next position for the current lane, move forward
                curr[lane] = next[lane];
            } else {
                // Otherwise, perform a side jump
                int ans = 1e9;
                for (int k = 1; k <= 3; k++) {
                    if (lane != k && obstacles[pos] != k) {
                        ans = min(ans, 1 + next[k]);  // Calculate the minimum jumps with a side jump
                    }
                }
                curr[lane] = ans;  // Store the result in the current position
            }
        }
        // Move the current results to the next array for the next iteration
        next = curr;
    }

    // The frog starts at lane 2, and we need to consider starting jumps from other lanes too
    return min(curr[2], min(curr[1] + 1, curr[3] + 1));
}
```

### Explanation
1. **Space Optimization:**
   - Instead of using a full 2D DP array (`dp[4][n]`), we use two 1D arrays: `curr` and `next`. These arrays store the results for the current and next positions respectively.
   - By iterating backward from `n-2` to `0`, we can update the current array based on the next position, effectively reducing the space complexity.

2. **Logic:**
   - If there’s no obstacle in the next position of the current lane, the frog can move forward (`curr[lane] = next[lane]`).
   - If there’s an obstacle, the frog needs to make a side jump. We then calculate the minimum jumps required by checking all other lanes.
   - After processing each position, we update `next` with the values in `curr`, so that the next iteration uses the updated information.

3. **Return Value:**
   - The frog starts at lane 2. The answer is the minimum of:
     - Staying in lane 2 (`curr[2]`).
     - Jumping from lane 1 (`curr[1] + 1` for a side jump).
     - Jumping from lane 3 (`curr[3] + 1` for a side jump).

### Time Complexity
- **Time Complexity:** \( O(3 \times n) \), where \( n \) is the length of the `obstacles` array. For each of the `n` positions, we iterate over 3 lanes, resulting in \( O(n) \) complexity.

### Space Complexity
- **Space Complexity:** \( O(3) \), because we only use two arrays (`curr` and `next`) of size 3 to store the minimum jumps at each position. This reduces the space requirement from \( O(3 \times n) \) to a constant size.

### Dry Run
#### Input: `obstacles = [0, 1, 2, 3, 0]`
- **Initialization:** `next[1] = next[2] = next[3] = 0`
- **At position 3:** Lane 2 is blocked, so the frog jumps to lane 1 or 3.
  - `curr[1] = 1 + next[3]`
  - `curr[3] = 1 + next[1]`
- **At position 0:** The frog starts at lane 2. Based on the calculated `curr`, the minimum jumps required is `2`.
