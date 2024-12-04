### Code

```cpp
class Solution {
public:
    int solveMem(int n, vector<int>& dp) {
        // Base case
        if (n==1) return 1;
        if (dp[n]!=-1) return dp[n];

        int maxProduct=0;
        // Try breaking n into two parts i and (n-i)
        for (int i=1; i<n; i++) {
            // Either take i and multiply it with (n-i)
            // Or take i and multiply it with the recursive solution of (n-i)
            maxProduct=max(maxProduct, i*max(n-i, solveMem(n-i, dp)));
        }
        return dp[n]=maxProduct;
    }

    int integerBreak(int n) {
        vector<int> dp(n+1, -1);
        return solveMem(n, dp);
    }
};
```

---

### Explanation

1. **Purpose**:
   - Break the integer `n` into at least two positive integers such that their product is maximized.
   
2. **Approach**:
   - Use recursion with memoization to compute the maximum product for each integer `n`.
   - At each step, split the integer `n` into two parts: `i` and `n - i`. Calculate:
     - The product of `i` and `(n - i)`.
     - The product of `i` and the recursive result of breaking `n - i` further.
   - Store the result for each `n` in a `dp` array to avoid redundant calculations.

3. **Base Case**:
   - If `n == 1`, return 1 because 1 cannot be split further.

4. **Recursive Formula**:
   - \( \text{dp}[n] = \max(\text{dp}[n], i \times \max(n - i, \text{solveMem}(n - i))) \), for all \( i \in [1, n-1] \).

---

### Time Complexity

- **Recursive calls**: The loop iterates \( n \) times for each recursive call.
- **Memoization**: Each subproblem is solved only once.
- **Overall Complexity**: \( O(n^2) \).

---

### Space Complexity

- **Space for Memoization**: \( O(n) \).
- **Recursive Stack Space**: \( O(n) \).
- **Overall Complexity**: \( O(n) \).

---

### Dry Run

**Input**: `n = 10`

#### Initialization
- `dp = [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1]`.

#### Step-by-Step Execution

1. **Call `integerBreak(10)`**:
   - Calls `solveMem(10)`.

2. **`solveMem(10)`**:
   - Loop through `i = 1 to 9`:
     - **`i = 1`**: Split `(1, 9) → 1 * max(9, solveMem(9))`. Calls `solveMem(9)`.
     - **`i = 2`**: Split `(2, 8) → 2 * max(8, solveMem(8))`. Calls `solveMem(8)`.
     - ...

3. **Base Cases Reached**:
   - `solveMem(1) = 1`.

4. **Memoization Updates**:
   - `dp[2] = 1` (2 = 1 + 1 → \( 1 \times 1 = 1 \)).
   - `dp[3] = 2` (3 = 1 + 2 → \( 1 \times 2 = 2 \)).
   - `dp[4] = 4` (4 = 2 + 2 → \( 2 \times 2 = 4 \)).
   - `dp[5] = 6` (5 = 2 + 3 → \( 2 \times 3 = 6 \)).
   - `dp[6] = 9` (6 = 3 + 3 → \( 3 \times 3 = 9 \)).
   - `dp[7] = 12` (7 = 3 + 4 → \( 3 \times 4 = 12 \)).
   - `dp[8] = 18` (8 = 3 + 3 + 2 → \( 3 \times 3 \times 2 = 18 \)).
   - `dp[9] = 27` (9 = 3 + 3 + 3 → \( 3 \times 3 \times 3 = 27 \)).

5. **Final Calculation**:
   - For `n = 10`:
     - **`i = 1`**: \( 1 \times \max(9, dp[9]) = 1 \times 27 = 27 \).
     - **`i = 2`**: \( 2 \times \max(8, dp[8]) = 2 \times 18 = 36 \).
     - **`i = 3`**: \( 3 \times \max(7, dp[7]) = 3 \times 12 = 36 \).
     - ...

6. **Final Result**:
   - `dp[10] = 36`.

---

**Output**: `36`
