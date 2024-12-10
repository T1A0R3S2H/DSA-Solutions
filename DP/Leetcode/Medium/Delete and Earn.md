#### Code:
```cpp
class Solution {
public:
    int solveMem(vector<int>& points, int n, vector<int>& dp) {
        if (n <= 0) return 0; // Base case: no points can be earned
        if (dp[n] != -1) return dp[n]; // Return cached result if already computed
        
        // Take or skip
        int take = points[n] + solveMem(points, n - 2, dp); // Take current and skip one
        int skip = solveMem(points, n - 1, dp); // Skip current
        
        return dp[n] = max(take, skip); // Store and return maximum result
    }

    int deleteAndEarn(vector<int>& nums) {
        int maxNum = *max_element(nums.begin(), nums.end()); // Find max number in nums
        vector<int> points(maxNum + 1, 0); // Array to store total points for each number
        
        // Fill points array
        for (int num : nums) {
            points[num] += num;
        }

        vector<int> dp(maxNum + 1, -1); // Memoization table
        return solveMem(points, maxNum, dp);
    }
};
```
![image](https://github.com/user-attachments/assets/732df380-e75d-4c63-bfd6-93ab12a417ef)

---

### Explanation:

1. **Transformation of Input (`points` array):**
   - Instead of working directly with `nums`, convert it to a `points` array.
   - For a number `x` in `nums`, add `x` to `points[x]`. This accumulates the total points from all occurrences of each number.
   - Example: `nums = [3, 4, 2]` → `points = [0, 0, 2, 3, 4]`.

2. **Recursive Approach with Memoization:**
   - Solve the problem like the **House Robber Problem**:
     - Decide for each index `n` in `points`:
       - Either **take it** (and skip the adjacent index).
       - Or **skip it** and move to the next index.
   - Use recursion with memoization to compute results efficiently.

3. **Memoization Table (`dp`):**
   - `dp[i]` stores the maximum points that can be earned from indices `1` to `i` in the `points` array.

---

### Time Complexity:
- **O(n):** We iterate through the `points` array once to fill it, and solve the problem for each index `1` to `maxNum` exactly once due to memoization.

### Space Complexity:
- **O(n):** Space for the `points` array and the `dp` array.

---

### Dry Run:

#### Input:
`nums = [3, 4, 2]`

#### Transformation:
`points = [0, 0, 2, 3, 4]`  
`maxNum = 4`  
`dp = [-1, -1, -1, -1, -1]`

---

#### Recursive Steps:

1. **`solveMem(points, 4, dp)`**  
   - Take: `points[4] + solveMem(points, 2, dp)`  
     - `4 + solveMem(points, 2, dp)`  
   - Skip: `solveMem(points, 3, dp)`  
   - Result: `max(4 + solveMem(points, 2, dp), solveMem(points, 3, dp))`.

2. **`solveMem(points, 3, dp)`**  
   - Take: `points[3] + solveMem(points, 1, dp)`  
     - `3 + solveMem(points, 1, dp)`  
   - Skip: `solveMem(points, 2, dp)`  
   - Result: `max(3 + solveMem(points, 1, dp), solveMem(points, 2, dp))`.

3. **`solveMem(points, 2, dp)`**  
   - Take: `points[2] + solveMem(points, 0, dp)`  
     - `2 + solveMem(points, 0, dp)`  
   - Skip: `solveMem(points, 1, dp)`  
   - Result: `max(2 + solveMem(points, 0, dp), solveMem(points, 1, dp))`.

4. **`solveMem(points, 1, dp)`**  
   - Base case: `n <= 0` → `return 0`.

5. **`solveMem(points, 0, dp)`**  
   - Base case: `n <= 0` → `return 0`.

---

#### Backtracking with Results:

- **`solveMem(points, 1, dp)` → `0`**  
- **`solveMem(points, 2, dp)`**  
  - Take: `2 + 0 = 2`.  
  - Skip: `0`.  
  - Result: `max(2, 0) = 2`.  
  - `dp[2] = 2`.

- **`solveMem(points, 3, dp)`**  
  - Take: `3 + 0 = 3`.  
  - Skip: `2`.  
  - Result: `max(3, 2) = 3`.  
  - `dp[3] = 3`.

- **`solveMem(points, 4, dp)`**  
  - Take: `4 + 2 = 6`.  
  - Skip: `3`.  
  - Result: `max(6, 3) = 6`.  
  - `dp[4] = 6`.

---

#### Final Output:
- `dp[maxNum] = dp[4] = 6`.  
- **Answer: `6`**.

--- 

Let me know if you need further clarifications! 😊
