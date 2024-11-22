### Code
```cpp
class Solution {
  public:
    int solveMem(vector<vector<int>>& arr, int n, int last, vector<vector<int>>& dp) {
        if (n==0) {
            if (last==0) return arr[0][0];
            if (last==1) return arr[0][1];
            if (last==2) return arr[0][2];
        }
        if (dp[n][last]!=-1) return dp[n][last];
        if (last==0) {
            dp[n][last]=arr[n][0]+max(solveMem(arr, n-1, 1, dp), solveMem(arr, n-1, 2, dp));
        }
        else if (last==1) {
            dp[n][last]=arr[n][1]+max(solveMem(arr, n-1, 0, dp), solveMem(arr, n-1, 2, dp));
        }
        else {
            dp[n][last]=arr[n][2]+max(solveMem(arr, n-1, 0, dp), solveMem(arr, n-1, 1, dp));
        }
        return dp[n][last];
    }

    int maximumPoints(vector<vector<int>>& arr, int n) {
        vector<vector<int>> dp(n, vector<int>(3, -1));
        return max(solveMem(arr, n-1, 0, dp), max(solveMem(arr, n-1, 1, dp), solveMem(arr, n-1, 2, dp)));
    }
};
```

### Explanation

This problem involves maximizing points by choosing one activity per day such that no activity is repeated on two consecutive days. The given approach solves the problem using **recursion with memoization**. Here’s the detailed breakdown:

1. **Recursive Functionality**:
   - The function `solveMem(arr, n, last, dp)` calculates the maximum points for the first `n` days where the last activity performed on day `n-1` is represented by `last` (0 for running, 1 for fighting, 2 for learning practice).  
   - Base case:
     - For day `0`, we directly return the points of the activity not performed on the previous day (`last`).
   - Recursive step:
     - Depending on the last activity, compute the maximum points from the two remaining activities using recursion. Add the current day's points for the chosen activity to this value.
   - Memoization:
     - The results of subproblems are stored in a 2D DP array `dp` to avoid redundant calculations.

2. **Final Calculation**:
   - At the end, the solution considers the maximum points achievable by choosing any activity on the last day (`n-1`).

---

### Time Complexity

1. **Recursive Calls**:
   - For each day, we calculate the maximum points for all possible `3` activities. This results in approximately `3 * n` recursive calls.
2. **Memoization Optimization**:
   - Since the DP array `dp` has dimensions `n x 3`, and each state is calculated only once, the overall complexity is reduced to **O(3 * n)**.

Thus, the time complexity is **O(n)** (ignoring the constant multiplier `3`).

---

### Space Complexity

1. **DP Array**:
   - The memoization table `dp` requires **O(3 * n)** space.
2. **Recursive Stack**:
   - The recursion depth is `n`, so the stack space is **O(n)**.

Thus, the total space complexity is **O(3 * n)**.

---

### Dry Run

**Input**: `arr = [[1, 2, 5], [3, 1, 1], [3, 3, 3]]`, `n = 3`

1. **Initialization**:
   - `dp` is initialized as a 2D array of size `3 x 3` filled with `-1`.

2. **Day 0 (Base Case)**:
   - If `last = 0`: Return `max(arr[0][1], arr[0][2]) = 5`.
   - If `last = 1`: Return `max(arr[0][0], arr[0][2]) = 5`.
   - If `last = 2`: Return `max(arr[0][0], arr[0][1]) = 2`.

3. **Day 1 (Recursive Step)**:
   - If `last = 0`: Choose from fighting (1 point) or learning (5 points).
     - `dp[1][0] = 3 + max(5, 2) = 8`.
   - If `last = 1`: Choose from running (1 point) or learning (5 points).
     - `dp[1][1] = 1 + max(1, 5) = 6`.
   - If `last = 2`: Choose from running (1 point) or fighting (1 point).
     - `dp[1][2] = 1 + max(1, 1) = 4`.

4. **Day 2 (Final Calculation)**:
   - If `last = 0`: Choose from fighting (3 points) or learning (3 points).
     - `dp[2][0] = 3 + max(6, 4) = 9`.
   - If `last = 1`: Choose from running (3 points) or learning (3 points).
     - `dp[2][1] = 3 + max(8, 4) = 11`.
   - If `last = 2`: Choose from running (3 points) or fighting (3 points).
     - `dp[2][2] = 3 + max(8, 6) = 11`.

**Result**: The maximum value is `max(9, 11, 11) = 11`.

---

This solution efficiently computes the maximum merit points for Geek while adhering to the constraints.
