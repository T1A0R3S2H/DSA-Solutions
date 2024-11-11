
#### Code:

```cpp
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n=nums.size();
        nums.insert(nums.begin(), 1);
        nums.push_back(1);
        vector<vector<int>> dp(n+2, vector<int>(n+2, -1));
        return solveMem(1, n, nums, dp);
    }
    
private:
    int solveMem(int left, int right, vector<int>& nums, vector<vector<int>>& dp) {
        if (left>right) return 0;
        if (dp[left][right]!=-1) return dp[left][right];
        int maxi=0;
        // Try bursting each balloon in the range [left, right - 1]
        for (int i=left; i<=right; ++i) {
            int coins=nums[left-1]*nums[i]*nums[right+1];
            coins+=solveMem(left, i-1, nums, dp);  // [left, i)
            coins+=solveMem(i+1, right, nums, dp);  // (i, right)
            maxi=max(maxi, coins);
        }
        dp[left][right]=maxi;
        return maxi;
    }
};

```

---

#### **Explanation:**

1. **Initialization of `nums` array:**
   - `nums.insert(nums.begin(), 1)` and `nums.push_back(1)` add virtual balloons with a value of `1` at the beginning and end of the array. This allows for easier boundary management (no need to handle edge cases like accessing `nums[left-1]` or `nums[right+1]` out of bounds).

2. **Dynamic Programming Table (`dp`):**
   - `vector<vector<int>> dp(n + 2, vector<int>(n + 2, -1));` creates a 2D DP table with dimensions `(n+2) x (n+2)` to store the results for subproblems, where each entry `dp[left][right]` stores the maximum coins that can be obtained by bursting all balloons between indices `left` and `right` inclusively. The table is initialized with `-1` indicating that a value has not yet been computed.

3. **Function `solveMem`:**
   - The function is a recursive helper function that computes the maximum coins for a subarray of balloons between indices `left` and `right` using memoization.

---

#### **ETSD:**

1. **Explanation of Base Conditions:**

   - **`if (left > right) return 0;`:**
     This is the base case. If `left > right`, it means that there are no balloons left to burst, so the result is `0`.

   - **`if (dp[left][right] != -1) return dp[left][right];`:**
     This condition checks if the subproblem `(left, right)` has already been computed (i.e., `dp[left][right]` is not `-1`). If it has been computed, we return the stored result from `dp[left][right]` to avoid redundant calculations, ensuring efficiency.

2. **Recursive Loop:**

   - **`for (int i = left; i <= right; ++i)`:**
     This loop iterates over all possible balloons to burst between `left` and `right` inclusively. The reason we use `<=` instead of `<` is that the `right` boundary is inclusive (as the problem statement treats both ends as valid indices to burst). This ensures that we consider all possible ways of bursting balloons in the subarray.

   - **`int coins = nums[left - 1] * nums[i] * nums[right + 1];`:**
     When bursting balloon `i`, the coins gained are calculated by multiplying the value of the virtual balloons at indices `left - 1`, `i`, and `right + 1` (the adjacent balloons to `i` after the burst). The virtual `1` balloons at the boundaries ensure valid multiplication even for the first and last balloons.
     - `nums[left - 1]` refers to the value before the first balloon in the subarray (or 1 if at the boundary).
     - `nums[i]` is the value of the balloon being burst.
     - `nums[right + 1]` refers to the value after the last balloon in the subarray (or 1 if at the boundary).

   - **`coins += solveMem(left, i - 1, nums, dp);`:**
     This recursive call solves the subproblem for the left part of the array (`[left, i-1]`), which represents the balloons remaining after bursting the `i`-th balloon.

   - **`coins += solveMem(i + 1, right, nums, dp);`:**
     Similarly, this recursive call solves the subproblem for the right part of the array (`[i+1, right]`), representing the balloons remaining after bursting the `i`-th balloon.

   - **`maxi = max(maxi, coins);`:**
     After calculating the coins for bursting the `i`-th balloon, we update the maximum result `maxi` by taking the maximum of the current maximum and the newly computed coins. This ensures that we pick the burst sequence that maximizes the total coins.

3. **Memoization:**
   - **`dp[left][right] = maxi;`:**
     After computing the result for the subproblem `(left, right)`, we store it in the `dp` table to avoid recomputing it in future recursive calls.

---

#### **Time Complexity:**

- **Time complexity of the recursive function `solveMem`:**
  The function calls itself recursively for subarrays in the range `[left, right]`. For each subarray, it loops over all possible balloons to burst (which are between `left` and `right`), leading to a time complexity of `O(n^2)` for filling the `dp` table.
  
  Specifically, the total number of subproblems is `O(n^2)` (since we are calculating the result for all pairs of `left` and `right`), and for each subproblem, we loop over `O(n)` balloons.
  
  Therefore, the overall time complexity is **O(n^3)**, where `n` is the number of balloons.

#### **Space Complexity:**

- **Space Complexity:**
  We store the `dp` table, which has dimensions `(n+2) x (n+2)`. Therefore, the space complexity is **O(n^2)** due to the DP table. Additionally, there is a recursive call stack that could go as deep as `O(n)`, but this does not increase the overall space complexity beyond `O(n^2)`.

---

#### **Dry Run (Example: `nums = [3, 1, 5, 8]`)**

1. **Initial setup:**
   - `nums = [1, 3, 1, 5, 8, 1]` (after adding 1 at the beginning and end)
   - DP table: `dp[0][0] = -1`, `dp[1][1] = -1`, ..., `dp[4][4] = -1`

2. **Recursive breakdown:**
   - Start with `solveMem(1, 4)`, where we try bursting each balloon from index 1 to 4. For each balloon `i`:
     - For `i = 1`, burst balloon 1 and calculate coins for left (`solveMem(1, 0)`) and right (`solveMem(2, 4)`) subarrays.
     - For `i = 2`, burst balloon 2 and calculate coins for left (`solveMem(1, 1)`) and right (`solveMem(3, 4)`) subarrays.
     - Continue this process recursively, memoizing the results.

---

This approach ensures that we efficiently calculate the maximum coins, avoiding redundant calculations with memoization and using dynamic programming principles.
