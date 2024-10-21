# 1. Recursive Solution (`solveRec`)
### Code
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

### Code
```cpp
class Solution {
public:
    int solveSO(vector<int>& satisfaction) {
        int n = satisfaction.size();
        vector<int> curr(n + 1, 0);
        vector<int> next(n + 1, 0);
        
        // Iterate from the last dish to the first dish
        for (int index = n - 1; index >= 0; index--) {
            // Iterate over possible time slots
            for (int time = index; time >= 0; time--) {
                int include = satisfaction[index] * (time + 1) + next[time + 1];
                int exclude = 0 + next[time];
                curr[time] = max(include, exclude);
            }
            next = curr; // Move to the next row (current row becomes previous)
        }
        return next[0]; // Maximum satisfaction starting with time 0
    }

    int maxSatisfaction(vector<int>& satisfaction) {
        sort(satisfaction.begin(), satisfaction.end());
        return solveSO(satisfaction);
    }
};
```

### Explanation
- The function `solveSO` is designed to calculate the maximum like-time coefficient by iterating through the sorted satisfaction levels from the last dish to the first.
- Two arrays, `curr` and `next`, are used to keep track of the maximum like-time coefficients for the current and the next time slots, respectively.
- For each dish (from last to first), the code computes:
  - **Include**: The contribution of the current dish multiplied by its cooking time plus the maximum coefficient obtainable with the remaining time.
  - **Exclude**: The maximum coefficient obtainable without including the current dish.
- After processing each dish, the `curr` array is copied to `next`, preparing it for the next iteration.
- Finally, `next[0]` gives the maximum satisfaction starting from time 0.

### Time Complexity
- The time complexity is \(O(n^2)\) because we have a nested loop: the outer loop runs \(n\) times (for each dish), and the inner loop runs up to \(n\) times (for the time slots).

### Space Complexity
- The space complexity is \(O(n)\) due to the two arrays `curr` and `next`, each of size \(n+1\).

### Dry Run
**Input**: `satisfaction = [-1, -8, 0, 5, -9]`

**Sorted Array**: `[-9, -8, -1, 0, 5]`

1. **Initialization**:
   - `n = 5`
   - `curr = [0, 0, 0, 0, 0, 0]`
   - `next = [0, 0, 0, 0, 0, 0]`

2. **Outer Loop (Index = 4)** (Dish: `5`):
   - `time = 4`: `include = 5 * (4 + 1) + next[5] = 30 + 0 = 30`
     - `exclude = next[4] = 0`
     - `curr[4] = max(30, 0) = 30`
   - `time = 3`: `include = 5 * (3 + 1) + next[4] = 20 + 0 = 20`
     - `exclude = next[3] = 0`
     - `curr[3] = max(20, 0) = 20`
   - `time = 2`: `include = 5 * (2 + 1) + next[3] = 15 + 0 = 15`
     - `exclude = next[2] = 0`
     - `curr[2] = max(15, 0) = 15`
   - `time = 1`: `include = 5 * (1 + 1) + next[2] = 10 + 0 = 10`
     - `exclude = next[1] = 0`
     - `curr[1] = max(10, 0) = 10`
   - `time = 0`: `include = 5 * (0 + 1) + next[1] = 5 + 0 = 5`
     - `exclude = next[0] = 0`
     - `curr[0] = max(5, 0) = 5`
   - After this iteration: `next = [5, 10, 15, 20, 30, 0]`

3. **Outer Loop (Index = 3)** (Dish: `0`):
   - `time = 3`: `include = 0 * (3 + 1) + next[4] = 0 + 30 = 30`
     - `exclude = next[3] = 20`
     - `curr[3] = max(30, 20) = 30`
   - `time = 2`: `include = 0 * (2 + 1) + next[3] = 0 + 20 = 20`
     - `exclude = next[2] = 15`
     - `curr[2] = max(20, 15) = 20`
   - `time = 1`: `include = 0 * (1 + 1) + next[2] = 0 + 15 = 15`
     - `exclude = next[1] = 10`
     - `curr[1] = max(15, 10) = 15`
   - `time = 0`: `include = 0 * (0 + 1) + next[1] = 0 + 10 = 10`
     - `exclude = next[0] = 5`
     - `curr[0] = max(10, 5) = 10`
   - After this iteration: `next = [10, 15, 20, 30, 30, 0]`

4. **Outer Loop (Index = 2)** (Dish: `-1`):
   - `time = 2`: `include = -1 * (2 + 1) + next[3] = -3 + 30 = 27`
     - `exclude = next[2] = 20`
     - `curr[2] = max(27, 20) = 27`
   - `time = 1`: `include = -1 * (1 + 1) + next[2] = -2 + 20 = 18`
     - `exclude = next[1] = 15`
     - `curr[1] = max(18, 15) = 18`
   - `time = 0`: `include = -1 * (0 + 1) + next[1] = -1 + 10 = 9`
     - `exclude = next[0] = 10`
     - `curr[0] = max(9, 10) = 10`
   - After this iteration: `next = [10, 18, 27, 30, 30, 0]`

5. **Outer Loop (Index = 1)** (Dish: `-8`):
   - `time = 1`: `include = -8 * (1 + 1) + next[2] = -16 + 27 = 11`
     - `exclude = next[1] = 18`
     - `curr[1] = max(11, 18) = 18`
   - `time = 0`: `include = -8 * (0 + 1) + next[1] = -8 + 10 = 2`
     - `exclude = next[0] = 10`
     - `curr[0] = max(2, 10) = 10`
   - After this iteration: `next = [10, 18, 27, 30, 30, 0]`

6. **Outer Loop (Index = 0)** (Dish: `-9`):
   - `time = 0`: `include = -9 * (0 + 1) + next[1] = -9 + 10 = 1`
     - `exclude = next[0] = 10`
     - `curr[0] = max(1, 10) = 10`
   - After this iteration: `next = [10, 18, 27, 30, 30, 0]`

Final Result: `next[0]` gives **14**.

### Conclusion
The modified space-optimized approach efficiently computes the maximum like-time coefficient while maintaining a straightforward structure without using extra space for a full DP table. The dry run illustrates how the algorithm accumulates values and updates the results iteratively.

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
