# 1. Recursive Solution (`solveRec`)

```cpp
int solveRec(vector<int>& satisfaction, int index, int time){
    int n=satisfaction.size();
    if(index>=satisfaction.size()) return 0;
    int include=satisfaction[index]*(time+1) + solveRec(satisfaction, index+1, time+1);
    int exclude=0+solveRec(satisfaction, index+1, time);
    return max(include, exclude);
}
```

#### Explanation
- This function uses recursion to explore two choices for each dish: **include** the dish or **exclude** it.
- The `include` case calculates the like-time coefficient for the current dish and recursively processes the next dish with increased time.
- The `exclude` case skips the current dish and processes the next dish without increasing time.
- The maximum of the two cases is returned.

#### Time Complexity
- \(O(2^n)\): In the worst case, each dish can either be included or excluded, leading to an exponential number of possibilities.

#### Space Complexity
- \(O(n)\): The maximum depth of the recursion stack can be \(n\), where \(n\) is the number of dishes.

#### Dry Run
For `satisfaction = [-1, -8, 0, 5, -9]`:
- Starting from index 0, the function will explore including and excluding each dish.
- Eventually, it will consider all combinations of dishes and calculate the maximum like-time coefficient, which will yield 14.

---

# 2. Memoized Solution (`solveMem`)

```cpp
int solveMem(vector<int>& satisfaction, int index, int time, vector<vector<int>>& dp){
    int n=satisfaction.size();
    if (index == n) return 0;
    if(dp[index][time]!=-1) return dp[index][time];
    int include=satisfaction[index]*(time+1)+solveMem(satisfaction, index+1, time+1, dp);
    int exclude=0+solveMem(satisfaction, index+1, time, dp);
    int ans=max(include, exclude);
    return dp[index][time]=ans;
}
```

#### Explanation
- This function is similar to `solveRec` but uses memoization to avoid recomputation.
- A 2D vector `dp` stores the results of previously computed states.
- If the result for a particular state `(index, time)` is already computed (i.e., not -1), it returns the stored value.
- Otherwise, it computes the result as in the recursive function and stores it in `dp`.

#### Time Complexity
- \(O(n^2)\): Due to the memoization and the nested structure of the recursive calls.

#### Space Complexity
- \(O(n^2)\): The 2D `dp` table stores results for all combinations of `index` and `time`.

#### Dry Run
For `satisfaction = [-1, -8, 0, 5, -9]`:
- As the function recurses, it checks the memoization table before making further recursive calls.
- This significantly reduces the number of calculations compared to the naive recursive approach.
- Ultimately, it will also yield a maximum like-time coefficient of 14.

---

# 3. Tabulated Solution (`solveTab`)

```cpp
int solveTab(vector<int>& satisfaction) {
        int n=satisfaction.size();
        sort(satisfaction.begin(), satisfaction.end());
        vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));

        // n to 0 aana chahiye tha, but n ka case to handle ho gaya (index==n) return 0
        for (int index=n-1; index>=0; index--) {
            for (int time=index; time>=0; time--) {
                int include=satisfaction[index]* (time+1) + dp[index+1][time+1];
                int exclude=0+dp[index+1][time];
                dp[index][time]=max(include, exclude);
            }
        }
        // anser hai: all dishes starting with time 0
        return dp[0][0];
    }
```

#### Explanation
- This function uses dynamic programming with a bottom-up approach.
- It sorts the `satisfaction` array to allow for optimal dish preparation order.
- A 2D DP table (`dp`) is constructed to store maximum like-time coefficients for combinations of dishes and time.
- It fills the table in reverse order, calculating the maximum for each index and time.

#### Time Complexity
- \(O(n^2)\): Due to the nested loops iterating through dishes and time values.

#### Space Complexity
- \(O(n^2)\): The DP table of size \(n \times n\).

#### Dry Run
For `satisfaction = [-1, -8, 0, 5, -9]`:
- After sorting: `[-9, -8, -1, 0, 5]`
- The DP table is filled iteratively, and the maximum like-time coefficient of 14 is calculated and returned from `dp[0][0]`.

---

# 4. Space-Optimized Solution (`solveSO`)

```cpp
int solveSO(vector<int>& satisfaction) {
    int n = satisfaction.size();
    sort(satisfaction.begin(), satisfaction.end());
    int maxi = 0;
    int currSum = 0;
    int time = 0;
    for (int index = n - 1; index >= 0; --index) {
        currSum += satisfaction[index];
        time += currSum;
        if (time > maxi) {
            maxi = time;
        }
    }
    return maxi;
}
```

#### Explanation
- This function also sorts the `satisfaction` array.
- It maintains only a few variables to track the current sum, time, and maximum like-time coefficient.
- It iterates backward through the sorted array, continuously updating the accumulated sums.

#### Time Complexity
- \(O(n \log n)\): The sorting step dominates the complexity.

#### Space Complexity
- \(O(1)\): Only a constant amount of additional space is used for variables.

#### Dry Run
For `satisfaction = [-1, -8, 0, 5, -9]`:
- After sorting: `[-9, -8, -1, 0, 5]`
- The function iterates from the last index to the first, computing cumulative values and ultimately determines the maximum like-time coefficient, which is 14.

---

### Summary
- Each function implements a different approach to the problem, varying in complexity and efficiency.
- The recursive approach is the simplest but least efficient, while the space-optimized version is the most efficient in terms of space usage.


---
# Dry Run (all 4 approaches)🔥✅
### 1. Recursive Solution (`solveRec`)

**Initial Input**: `satisfaction = [-1, -8, 0, 5, -9]`

**Sorted Array**: `[-9, -8, -1, 0, 5]`

**Function Call Stack**:
- The function starts with `index = 0` and `time = 0`.

**Dry Run Steps**:
1. **Index 0** (`satisfaction[0] = -9`):
   - Include: `-9 * (0 + 1) + solveRec(1, 1)` → `-9 + solveRec(1, 1)`
   - Exclude: `solveRec(1, 0)`
   
2. **Index 1** (`satisfaction[1] = -8`):
   - Include: `-8 * (1 + 1) + solveRec(2, 2)` → `-16 + solveRec(2, 2)`
   - Exclude: `solveRec(2, 1)`
   
3. **Index 2** (`satisfaction[2] = -1`):
   - Include: `-1 * (2 + 1) + solveRec(3, 3)` → `-3 + solveRec(3, 3)`
   - Exclude: `solveRec(3, 2)`

4. **Index 3** (`satisfaction[3] = 0`):
   - Include: `0 * (3 + 1) + solveRec(4, 4)` → `0 + solveRec(4, 4)`
   - Exclude: `solveRec(4, 3)`

5. **Index 4** (`satisfaction[4] = 5`):
   - Include: `5 * (4 + 1) + solveRec(5, 5)` → `25 + 0`
   - Exclude: `0`

Calculating values:
- **Max without any dishes**: 0
- **Max including dish 4**: 25
- **Max including dish 3**: 25
- **Max including dish 2**: `-3 + 25 = 22`
- **Max including dish 1**: `-16 + 22 = 6`
- **Max including dish 0**: `-9 + 6 = -3`

The maximum like-time coefficient calculated is **14** through different combinations recursively.

### 2. Memoized Solution (`solveMem`)

**Initial Input**: `satisfaction = [-1, -8, 0, 5, -9]`

**Sorted Array**: `[-9, -8, -1, 0, 5]`

**Memoization Table**: Initialized with `-1`.

**Dry Run Steps**:
1. Start with `index = 0` and `time = 0`.
2. The function will first check if the value in `dp[index][time]` is already calculated.
3. Follow similar steps as the recursive solution, but store the computed values in the `dp` table.
4. Each time a state is computed, store the result, e.g., for `dp[0][0]`, `dp[1][1]`, etc.
5. When reaching a previously computed state, return the value immediately.

Example states and calculations will be the same as the recursive version but will not redo calculations for the same `(index, time)` pairs.

Final computed `maxSatisfaction` will yield **14**.

### 3. Tabulated Solution (`solveTab`)

**Initial Input**: `satisfaction = [-1, -8, 0, 5, -9]`

**Sorted Array**: `[-9, -8, -1, 0, 5]`

**DP Table Initialization**: A 2D DP table of size `(n+1) x (n+1)` initialized to 0.

**Dry Run Steps**:
1. Fill the DP table in reverse order.
2. For each `index` from `n-1` to `0` and for each `time` from `0` to `n-1`, compute:
   - `include = satisfaction[index] * (time + 1) + dp[index + 1][time + 1]`
   - `exclude = dp[index + 1][time]`
   - Store `max(include, exclude)` in `dp[index][time]`.
   
**Calculations in the DP Table**:
- For `index = 4`, `time = 0 to 4`: Only include dish 4 with value 5.
- For `index = 3`, `time = 0 to 4`: Include dish 3 with 0; maximum would be from dish 4.
- Continue filling until `index = 0`.

Final value `dp[0][0]` yields **14**.

### 4. Space-Optimized Solution (`solveSO`)

**Initial Input**: `satisfaction = [-1, -8, 0, 5, -9]`

**Sorted Array**: `[-9, -8, -1, 0, 5]`

**Variables**: `maxi = 0`, `currSum = 0`, `time = 0`.

**Dry Run Steps**:
1. Iterate backward through the sorted `satisfaction` array.
2. Update `currSum` and calculate `time`:
   - **Index 4**: `currSum = 5`, `time = 5`, `maxi = max(0, 5) = 5`
   - **Index 3**: `currSum = 5`, `time = 10`, `maxi = max(5, 10) = 10`
   - **Index 2**: `currSum = 4`, `time = 14`, `maxi = max(10, 14) = 14`
   - **Index 1**: `currSum = -4`, `time = 10`, `maxi = max(14, 10) = 14`
   - **Index 0**: `currSum = -13`, `time = -3`, `maxi = max(14, -3) = 14`

Final result from the space-optimized approach gives a maximum like-time coefficient of **14**.

### Summary of Results
- All four approaches yield the maximum like-time coefficient of **14**.
- The recursive solution provides a clear path but with exponential complexity.
- The memoized and tabulated approaches reduce redundancy but use additional space.
- The space-optimized approach efficiently computes the result using linear space, demonstrating the power of cumulative calculations and sorted order.
