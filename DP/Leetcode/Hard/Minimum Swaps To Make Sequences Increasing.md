# 🔥1. Recursive

```cpp
int solveRec(vector<int>& nums1, vector<int>& nums2, int index, bool swapped){
    if(index==nums1.size()) return 0;
    int ans=INT_MAX;
    int prev1=nums1[index-1];
    int prev2=nums2[index-1];
    //catch
    if(swapped) swap(prev1, prev2);

    //no swap
    if(nums1[index]>prev1 && nums2[index]>prev2){
        ans=solveRec(nums1, nums2, index+1, 0);
    }

    //swap
    if(nums1[index]>prev2 && nums2[index]>prev1){
        ans=min(ans, 1+solveRec(nums1, nums2, index+1, 1));
    }
    return ans;
}
```

### Explanation

The `solveRec` function uses recursion to find the minimum number of swaps required to make the sequences `nums1` and `nums2` strictly increasing. Here's a step-by-step explanation of how it works:

1. **Base Case**: If `index` reaches the length of `nums1`, this means we have processed all elements, so we return 0 as no more swaps are needed.

2. **Initialize Answer**: We set `ans` to `INT_MAX` to store the minimum swaps needed.

3. **Set Previous Values**: We use `prev1` and `prev2` to hold the previous elements of `nums1` and `nums2` respectively, which are used to check if the sequences remain strictly increasing. If `swapped` is true, we swap `prev1` and `prev2` since that would represent a state where the previous index values were swapped.

4. **No-Swap Case**: 
   - If `nums1[index] > prev1` and `nums2[index] > prev2`, the sequences remain strictly increasing without any swaps at the current `index`.
   - We call `solveRec(nums1, nums2, index + 1, 0)` recursively without incrementing the swap count.

5. **Swap Case**:
   - If `nums1[index] > prev2` and `nums2[index] > prev1`, a swap at the current index can also keep both sequences strictly increasing.
   - We call `solveRec(nums1, nums2, index + 1, 1)` recursively, adding 1 to the swap count and setting `swapped` to `1` (true).

6. **Return Minimum Swaps**: Finally, `ans` is returned, holding the minimum of the no-swap and swap cases.

### Time Complexity

- **O(2^N)**, where \( N \) is the length of `nums1` and `nums2`. This is because for each index, we have two choices (swap or no-swap), leading to a total of \( 2^N \) recursive calls in the worst case.

### Space Complexity

- **O(N)** for the recursive call stack, where \( N \) is the length of `nums1` and `nums2`.

### Dry Run

For `nums1 = [1, 3, 5, 4]` and `nums2 = [1, 2, 3, 7]`:

1. **Initial Call**: `solveRec(nums1, nums2, 1, 0)`
   - `index = 1`, `swapped = 0`, `prev1 = 1`, `prev2 = 1`.
   - **No Swap Case**: `nums1[1] > prev1` and `nums2[1] > prev2` are true (`3 > 1` and `2 > 1`), so call `solveRec(nums1, nums2, 2, 0)`.
   - **Swap Case**: `nums1[1] > prev2` and `nums2[1] > prev1` are also true (`3 > 1` and `2 > 1`), so call `solveRec(nums1, nums2, 2, 1)` with a swap (add 1 to the swap count).

2. **No Swap Path** (calling `solveRec(nums1, nums2, 2, 0)`):
   - `index = 2`, `swapped = 0`, `prev1 = 3`, `prev2 = 2`.
   - **No Swap Case**: `nums1[2] > prev1` and `nums2[2] > prev2` are true (`5 > 3` and `3 > 2`), so call `solveRec(nums1, nums2, 3, 0)`.
   - **Swap Case**: `nums1[2] > prev2` and `nums2[2] > prev1` are true (`5 > 2` and `3 > 3` is false), so skip this.

3. **No Swap Path Continuation** (calling `solveRec(nums1, nums2, 3, 0)`):
   - `index = 3`, `swapped = 0`, `prev1 = 5`, `prev2 = 3`.
   - **No Swap Case**: `nums1[3] > prev1` and `nums2[3] > prev2` are false (`4 > 5` is false), so skip this.
   - **Swap Case**: `nums1[3] > prev2` and `nums2[3] > prev1` are true (`4 > 3` and `7 > 5`), so call `solveRec(nums1, nums2, 4, 1)` with a swap (total swaps = 1).

4. **Base Case Reached** (`solveRec(nums1, nums2, 4, 1)`):
   - `index = 4` is equal to `nums1.size()`, so return 0 as no more swaps are needed.

5. **Backtracking**: Return values propagate back up the recursion stack, and the minimum swap count, which is 1, is returned.

---


# 🔥2. Recur+Memoization

```cpp
int solveMem(vector<int>& nums1, vector<int>& nums2, int index, bool swapped, vector<vector<int>>& dp) {
    if (index == nums1.size()) return 0;
    if (dp[index][swapped] != -1) return dp[index][swapped];
    
    int ans = INT_MAX;
    int prev1 = nums1[index - 1];
    int prev2 = nums2[index - 1];
    
    if (swapped) swap(prev1, prev2);

    // No swap case
    if (nums1[index] > prev1 && nums2[index] > prev2) {
        ans = solveMem(nums1, nums2, index + 1, 0, dp);
    }

    // Swap case
    if (nums1[index] > prev2 && nums2[index] > prev1) {
        ans = min(ans, 1 + solveMem(nums1, nums2, index + 1, 1, dp));
    }

    return dp[index][swapped] = ans;
}
```

### Explanation

The `solveMem` function is a modified version of `solveRec`, utilizing memoization to optimize the recursive solution by storing intermediate results. This prevents redundant computations, improving performance. Here’s how it works:

1. **Base Case**: 
   - If `index` reaches `nums1.size()`, we've processed all elements, so we return 0 as no further swaps are needed.

2. **Memoization Check**:
   - If `dp[index][swapped]` has already been computed (i.e., it's not -1), we return the stored result to avoid redundant calculations.

3. **Set Previous Values**:
   - `prev1` and `prev2` represent the previous elements of `nums1` and `nums2` respectively. If `swapped` is `true`, we swap `prev1` and `prev2`, as this accounts for the last swap that could have occurred in the previous call.

4. **No-Swap Case**:
   - If `nums1[index] > prev1` and `nums2[index] > prev2`, it means the sequences remain strictly increasing without any swap. We recursively call `solveMem(nums1, nums2, index + 1, 0, dp)`.

5. **Swap Case**:
   - If `nums1[index] > prev2` and `nums2[index] > prev1`, a swap at the current index keeps both sequences strictly increasing. We call `solveMem(nums1, nums2, index + 1, 1, dp)` and add 1 to the swap count.

6. **Store Result in dp Array**:
   - After computing the minimum swaps needed for the current index, we store it in `dp[index][swapped]`. This ensures that any future calls with the same parameters will return the precomputed value, reducing redundant recursive calls.

7. **Return Minimum Swaps**:
   - Finally, the function returns `dp[index][swapped]`, which holds the minimum of the no-swap and swap cases.

### Time Complexity

- **O(N)**, where \( N \) is the length of `nums1` and `nums2`, because the memoization reduces redundant calls. There are only two states for each index (swap or no-swap), resulting in \( O(N \times 2) \) or \( O(N) \).

### Space Complexity

- **O(N)** for the `dp` array and **O(N)** for the recursive call stack, totaling **O(N)** space complexity.

### Dry Run

For `nums1 = [1, 3, 5, 4]` and `nums2 = [1, 2, 3, 7]`:

1. **Initial Call**: `solveMem(nums1, nums2, 1, 0, dp)`
   - `index = 1`, `swapped = 0`, `prev1 = 1`, `prev2 = 1`.
   - Since `dp[1][0] == -1`, we proceed with calculations.

2. **No-Swap Case**: 
   - `nums1[1] > prev1` and `nums2[1] > prev2` are true (`3 > 1` and `2 > 1`), so call `solveMem(nums1, nums2, 2, 0, dp)`.

3. **Swap Case**:
   - `nums1[1] > prev2` and `nums2[1] > prev1` are also true (`3 > 1` and `2 > 1`), so call `solveMem(nums1, nums2, 2, 1, dp)` with a swap (add 1 to swap count).

4. **No-Swap Path** (calling `solveMem(nums1, nums2, 2, 0, dp)`):
   - `index = 2`, `swapped = 0`, `prev1 = 3`, `prev2 = 2`.
   - Since `dp[2][0] == -1`, we continue.
   - **No-Swap Case**: `nums1[2] > prev1` and `nums2[2] > prev2` are true (`5 > 3` and `3 > 2`), so call `solveMem(nums1, nums2, 3, 0, dp)`.

5. **No-Swap Path Continuation** (calling `solveMem(nums1, nums2, 3, 0, dp)`):
   - `index = 3`, `swapped = 0`, `prev1 = 5`, `prev2 = 3`.
   - Since `dp[3][0] == -1`, we continue.
   - **No-Swap Case**: `nums1[3] > prev1` and `nums2[3] > prev2` are false (`4 > 5` is false), so skip this.
   - **Swap Case**: `nums1[3] > prev2` and `nums2[3] > prev1` are true (`4 > 3` and `7 > 5`), so call `solveMem(nums1, nums2, 4, 1, dp)` with a swap (total swaps = 1).

6. **Base Case Reached** (`solveMem(nums1, nums2, 4, 1, dp)`):
   - `index = 4` is equal to `nums1.size()`, so return 0 as no more swaps are needed.

7. **Backtracking and Memoization**:
   - As we backtrack, each recursive result is stored in `dp` to prevent redundant calculations.
   - Final result: the minimum swap count is 1, and `dp` has stored this result, ensuring efficiency for future calls.

---


# 🔥3. Tabulation (Bottom-Up)

```cpp
int solveTab(vector<int>& nums1, vector<int>& nums2) {
    int n = nums1.size();
    vector<vector<int>> dp(n + 1, vector<int>(2, 0));

    for (int index = n - 1; index >= 1; index--) {
        for (int swapped = 1; swapped >= 0; swapped--) {
            int ans = INT_MAX;
            int prev1 = nums1[index - 1];
            int prev2 = nums2[index - 1];
            
            // Apply previous swap state
            if (swapped) swap(prev1, prev2);
            
            // No swap case
            if (nums1[index] > prev1 && nums2[index] > prev2) {
                ans = dp[index + 1][0];
            }
            
            // Swap case
            if (nums1[index] > prev2 && nums2[index] > prev1) {
                ans = min(ans, 1 + dp[index + 1][1]);
            }
            
            dp[index][swapped] = ans;
        }
    }
    return dp[1][0];
}
```

### Explanation

The `solveTab` function uses a bottom-up tabulation approach with dynamic programming to eliminate recursion and achieve the same result more efficiently. Here’s how it works:

1. **Initialization**:
   - We create a 2D DP array `dp` with dimensions `(n+1) x 2`, where `n` is the size of `nums1` and `nums2`. `dp[index][swapped]` represents the minimum number of swaps required to make both arrays strictly increasing starting from `index`, with `swapped` indicating whether the previous index was swapped (1) or not (0).

2. **Bottom-Up Calculation**:
   - We iterate from the last index (`n-1`) back to the first index (`1`). Starting from `index = n-1` and working backwards ensures that we have computed the results for subsequent elements before using them.

3. **Set Previous Values**:
   - For each index, `prev1` and `prev2` represent the values from the previous position of `nums1` and `nums2`. If `swapped` is true, we swap `prev1` and `prev2` to simulate the effect of a previous swap.

4. **No-Swap Case**:
   - If `nums1[index] > prev1` and `nums2[index] > prev2`, it means the sequences are strictly increasing without any swap at the current index.
   - We set `ans = dp[index + 1][0]`, which is the result of the next index without swapping.

5. **Swap Case**:
   - If `nums1[index] > prev2` and `nums2[index] > prev1`, a swap at the current index keeps both sequences strictly increasing.
   - We set `ans = min(ans, 1 + dp[index + 1][1])` by adding 1 to the swap count and using the result from the next index with a swap.

6. **Store Result**:
   - We store the minimum swaps required in `dp[index][swapped]`, and then move to the previous index.

7. **Return Result**:
   - After completing all iterations, the answer is stored in `dp[1][0]`, representing the minimum swaps needed for both arrays starting at index 1 without any initial swap.

### Time Complexity

- **O(N)**, where \( N \) is the length of `nums1` and `nums2`, as each index and state (swap or no-swap) is processed once.

### Space Complexity

- **O(N)**, for the `dp` array used to store the results of each subproblem.

### Dry Run

For `nums1 = [1, 3, 5, 4]` and `nums2 = [1, 2, 3, 7]`:

1. **Initialization**:
   - `dp` is initialized as a `(5 x 2)` matrix filled with 0s.
   
2. **Processing from Last Index**:
   - `index = 3`, `swapped = 0`: 
      - `prev1 = nums1[2] = 5`, `prev2 = nums2[2] = 3`.
      - No-swap case: `nums1[3] = 4` and `nums2[3] = 7` don’t satisfy the condition (`4 > 5` is false), so this path is skipped.
      - Swap case: `nums1[3] > prev2` and `nums2[3] > prev1` (`4 > 3` and `7 > 5`) is true. Therefore, `ans = min(INT_MAX, 1 + dp[4][1]) = 1`.

3. **Back to `index = 2`, `swapped = 0`**:
   - `prev1 = 3`, `prev2 = 2`.
   - No-swap case: `nums1[2] > prev1` and `nums2[2] > prev2` (`5 > 3` and `3 > 2`) is true, so `ans = dp[3][0] = 1`.
   - Swap case: `nums1[2] > prev2` and `nums2[2] > prev1` (`5 > 2` and `3 > 3` is false), so it is skipped.
   - Store `dp[2][0] = 1`.

4. **Back to `index = 1`, `swapped = 0`**:
   - `prev1 = 1`, `prev2 = 1`.
   - Both no-swap and swap cases satisfy the conditions, with both paths leading to `dp[2][0]` or `dp[2][1]`, both of which result in a minimum swap count of 1. Store `dp[1][0] = 1`.

5. **Result**: 
   - The answer is found in `dp[1][0]`, which is 1, indicating the minimum number of swaps needed to make both sequences strictly increasing is 1.


---


# 🔥4. Space Optimized

```cpp
int spaceOptimized(vector<int>& nums1, vector<int>& nums2) {
    vector<int> dp(2, 0);
    
    for (int idx = nums1.size() - 1; idx >= 0; idx--) {
        vector<int> temp(2);
        
        for (int swapped = 1; swapped >= 0; swapped--) {
            int ans = INT_MAX;
            
            // Swap elements if needed
            if (swapped) {
                swap(nums1[idx], nums2[idx]);
            }
            
            // No previous index to compare for the first element
            if (idx == 0 || (nums1[idx] > nums1[idx - 1] && nums2[idx] > nums2[idx - 1])) {
                ans = dp[0];
            }
            
            // Previous index condition for swapped elements
            if (idx == 0 || (nums1[idx] > nums2[idx - 1] && nums2[idx] > nums1[idx - 1])) {
                ans = min(ans, 1 + dp[1]);
            }
            
            // Store the answer in the temporary array
            temp[swapped] = ans;   
        }
        
        // Update the dp array for the next iteration
        dp = temp;
    }
    
    return dp[0];
}
```

### Explanation

The `spaceOptimized` function implements a dynamic programming solution to minimize space usage by only maintaining the necessary information for the current and previous iterations. Here’s how it works:

1. **Initialization**:
   - A 1D array `dp` of size 2 is initialized to store the minimum swaps needed to make the sequences strictly increasing. `dp[0]` represents the state without a swap, and `dp[1]` represents the state with a swap.

2. **Iterate Backwards**:
   - We iterate through the indices of `nums1` and `nums2` from the last index down to the first (`idx = size - 1` to `0`). This ensures that we are calculating results for future indices before using them.

3. **Temporary Array**:
   - A temporary array `temp` of size 2 is used to hold the results for the current index, which will be copied to `dp` after processing both swap states.

4. **Swap Handling**:
   - Inside the inner loop, we check for the current state (swapped or not). If `swapped` is true, we swap `nums1[idx]` and `nums2[idx]`.

5. **Condition Checks**:
   - For the first index (`idx == 0`), we can directly return the values in `dp` since there's no previous index to compare against.
   - For non-first indices, we check if the current elements (with and without swap) maintain a strictly increasing sequence:
     - **No Swap**: Check if both `nums1[idx] > nums1[idx - 1]` and `nums2[idx] > nums2[idx - 1]`. If true, set `ans = dp[0]`.
     - **Swap**: Check if `nums1[idx] > nums2[idx - 1]` and `nums2[idx] > nums1[idx - 1]`. If true, compute `ans = min(ans, 1 + dp[1])`.

6. **Store Result**:
   - The computed minimum swaps for the current index and swap state are stored in the `temp` array.

7. **Update DP Array**:
   - After processing both states for the current index, we update `dp` with the values from `temp`.

8. **Return Result**:
   - The final result is returned from `dp[0]`, which represents the minimum number of swaps needed for the sequences starting from the first index without any initial swap.

### Time Complexity

- **O(N)**, where \( N \) is the length of `nums1` and `nums2`, since each index is processed once.

### Space Complexity

- **O(1)**, as only a fixed amount of additional space is used (the `dp` and `temp` arrays of size 2), irrespective of the input size.

### Dry Run

For `nums1 = [1, 3, 5, 4]` and `nums2 = [1, 2, 3, 7]`:

1. **Initialization**:
   - `dp` is initialized to `[0, 0]`.

2. **Processing from Last Index**:
   - **For `idx = 3`**:
     - `temp` is initialized to `[0, 0]`.
     - `swapped = 1`: Swap the elements to get `nums1 = [1, 3, 5, 7]` and `nums2 = [1, 2, 3, 4]`. Since `idx = 0` is false, we check the previous conditions:
       - No swap case: False.
       - Swap case: True (resulting in `1 + 0 = 1`).
     - `temp[1] = 1`.
     - `swapped = 0`: Elements are not swapped. Check the previous conditions:
       - No swap case: False.
       - Swap case: False (no valid sequences).
     - `temp[0] = INT_MAX`.
   - After processing `idx = 3`, `dp` becomes `[INT_MAX, 1]`.

3. **Back to `idx = 2`**:
   - `temp` is reset to `[0, 0]`.
   - **For `swapped = 1`**: Swap to get `nums1[2] = 7`, and `nums2[2] = 5` (valid sequence). Check previous:
       - No swap case: True (`5 > 3` and `3 > 2`).
       - Swap case: False.
     - `temp[1] = 1`.
   - **For `swapped = 0`**: No swap. Check previous:
       - No swap case: True (`5 > 3` and `3 > 2`).
       - Swap case: False.
     - `temp[0] = 1`.
   - After processing `idx = 2`, `dp` becomes `[1, 1]`.

4. **Back to `idx = 1`**:
   - `temp` is reset to `[0, 0]`.
   - **For `swapped = 1`**: Swap to get `nums1[1] = 2`, and `nums2[1] = 3` (valid sequence). Check previous:
       - No swap case: True (`3 > 1` and `2 > 1`).
       - Swap case: False.
     - `temp[1] = 0`.
   - **For `swapped = 0`**: No swap. Check previous:
       - No swap case: True (`3 > 1` and `1 > 1`).
       - Swap case: True (`2 > 1` and `1 > 1`).
     - `temp[0] = 1`.
   - After processing `idx = 1`, `dp` becomes `[0, 1]`.

5. **Back to `idx = 0`**:
   - `temp` is reset to `[0, 0]`.
   - **For both states**: As there are no elements left to compare with, the final answer is determined based on prior indices stored in `dp`, giving `temp` as `[0, 1]`.

6. **Result**: 
   - The final answer is found in `dp[0]`, which is `1`, indicating that a minimum of 1 swap is required to make both sequences strictly increasing.
