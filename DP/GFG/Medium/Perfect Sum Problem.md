### **CETSD for Perfect Sum Problem**

---

### **Code**

```cpp
class Solution {
public:
    static const int MOD = 1e9 + 7;

    int solveMem(vector<int>& arr, int target, int index, vector<vector<int>>& dp) {
        // Base cases
        if (index == arr.size()) {
            return target == 0 ? 1 : 0; // If we've processed all elements, check if the target is met
        }

        // Return cached result
        if (dp[index][target] != -1) return dp[index][target];

        // Exclude the current element
        int notTake = solveMem(arr, target, index + 1, dp);

        // Include the current element
        int take = 0;
        if (arr[index] <= target) {
            take = solveMem(arr, target - arr[index], index + 1, dp);
        }

        // Store result in dp table
        dp[index][target] = (take + notTake) % MOD;
        return dp[index][target];
    }

    int perfectSum(vector<int>& arr, int target) {
        int n = arr.size();
        vector<vector<int>> dp(n, vector<int>(target + 1, -1));
        return solveMem(arr, target, 0, dp);
    }
};
```

---

### **Explanation**

1. **Function Definitions**:
   - `solveMem`: Recursive function that solves the problem using memoization.
   - `perfectSum`: Initializes the `dp` table and starts the recursive process from `index = 0`.

2. **Logic**:
   - The goal is to count subsets of the array `arr` that sum up to the given `target`.
   - At each element in the array (`index`), two choices are considered:
     - **Exclude the current element** (`notTake`): Continue with the next element while keeping the `target` unchanged.
     - **Include the current element** (`take`): Reduce the `target` by the value of the current element and move to the next element.
   - The sum of these two choices gives the number of subsets that satisfy the condition.

3. **Base Case**:
   - If `index == arr.size()`, check if the `target` is 0:
     - Return `1` if `target == 0` (a valid subset is found).
     - Return `0` otherwise (no valid subset).

4. **Memoization**:
   - Use a 2D `dp` table to store results of subproblems:
     - `dp[index][target]` represents the count of subsets from `arr[index..n-1]` that sum to `target`.
   - If a subproblem has already been solved (`dp[index][target] != -1`), return the cached result.

5. **Modulo Operation**:
   - Results are computed modulo \(10^9 + 7\) to handle large numbers.

---

### **Time Complexity**

- **Recursive Calls**: Each state (`index`, `target`) is visited only once, leading to \(O(n \times \text{target})\) calls.
- **Memoization Table**: Filling a table of size \(O(n \times \text{target})\).

**Overall Time Complexity**: \(O(n \times \text{target})\).

---

### **Space Complexity**

- **DP Table**: \(O(n \times \text{target})\).
- **Recursive Stack**: \(O(n)\) due to recursion.

**Overall Space Complexity**: \(O(n \times \text{target})\).

---

### **Dry Run**

#### **Input**
`arr = [5, 2, 3, 10]`, `target = 10`.

#### **Steps**
1. **Initialization**:
   - `n = 4`, `dp = vector<vector<int>>(4, vector<int>(11, -1))`.

2. **Recursive Calls**:
   - Start at `solveMem(arr, 10, 0, dp)`:
     - Exclude `arr[0] = 5`: Call `solveMem(arr, 10, 1, dp)`.
     - Include `arr[0] = 5`: Call `solveMem(arr, 5, 1, dp)`.

   - At `solveMem(arr, 10, 1, dp)`:
     - Exclude `arr[1] = 2`: Call `solveMem(arr, 10, 2, dp)`.
     - Include `arr[1] = 2`: Call `solveMem(arr, 8, 2, dp)`.

   - Continue exploring until `index == 4` and `target == 0` or other base cases are hit.

3. **DP Table** (final state):
   After all recursive calls, the result is stored in `dp[0][10]`.

#### **Output**
The number of subsets that sum up to `10` is `3`.

---

### **TL;DR**

- The code counts subsets of an array `arr` that sum to a given `target` using recursion and memoization.
- **Key Idea**: At each step, either include or exclude the current element and compute the count recursively.
- Optimized using a `dp` table to avoid redundant computations, with results taken modulo \(10^9 + 7\).
