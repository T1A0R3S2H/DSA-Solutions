# 🔥🔥The most `observation-based` question related to DP seen so far!


### **Problem Summary:**
You are given a `2 x n` board and need to find the number of ways to tile the board using **dominoes (2x1)** and **trominoes (L-shaped, covering 3 squares)**. The answer should be returned modulo `10^9 + 7`.

### **Recurrence Relation:**
We use the following recurrence relation to solve the problem:

\[
dp[i] = (2 \times dp[i-1] + dp[i-3]) \mod 1000000007
\]

Where:
- `dp[i]` represents the number of ways to tile a `2 x i` board.
- `dp[i-1]` accounts for the ways to tile the board when the last column is covered by a vertical domino.
- `dp[i-3]` accounts for the ways to tile the board when the last part is covered by a tromino (L-shaped).

### **Detailed Explanation:**

1. **Base Cases:**
   We need to initialize the base cases for smaller board sizes:
   - `dp[0] = 1`: There is exactly one way to tile an empty `2 x 0` board, which is doing nothing.
   - `dp[1] = 1`: There is exactly one way to tile a `2 x 1` board, using a vertical domino.
   - `dp[2] = 2`: There are two ways to tile a `2 x 2` board: either two vertical dominoes or two horizontal dominoes.
   - `dp[3] = 5`: This can be derived from earlier subproblems and calculations.

2. **Recurrence Breakdown:**
   The recurrence relation consists of two main components:
   - `2 * dp[i - 1]`: This accounts for the cases where the last column is tiled using a vertical domino. This is because for a `2 x (i - 1)` board, you can extend it by placing a vertical domino on the last column, and there are two ways to place the domino.
   - `dp[i - 3]`: This represents the case where the last part of the board is covered by a tromino, which reduces the problem size by 3 columns. The tromino is L-shaped and covers three cells in a `2 x 3` area.

3. **Modulo Operation:**
   Since the result can be very large, every time we calculate `dp[i]`, we take the result modulo `10^9 + 7` to ensure that the answer fits within the limits.

### **Time Complexity:**

- **Time Complexity:**  
  The time complexity of the solution is **O(n)**, where `n` is the size of the board. We only need to iterate through the array `dp` from `0` to `n`, and for each index, we do constant-time operations (multiplying and adding).
  
  The overall time complexity is linear in terms of `n`.

- **Space Complexity:**  
  The space complexity is **O(n)** due to the storage required for the `dp` array, which holds the number of ways to tile boards of size `2 x i` for all `i` from `0` to `n`.

### **Dry Run (Example: n = 4)**

We will now perform a dry run for `n = 4` to see how the recurrence relation and base cases are used to fill the `dp` array.

#### Initializing base cases:
- `dp[0] = 1` (1 way to tile an empty board)
- `dp[1] = 1` (1 way to tile a `2 x 1` board)
- `dp[2] = 2` (2 ways to tile a `2 x 2` board)

#### Computing `dp[3]`:
Using the recurrence:
\[
dp[3] = (2 \times dp[2] + dp[0]) \mod 1000000007
\]
\[
dp[3] = (2 \times 2 + 1) \mod 1000000007 = (4 + 1) \mod 1000000007 = 5
\]
So, `dp[3] = 5`.

#### Computing `dp[4]`:
Now compute `dp[4]`:
\[
dp[4] = (2 \times dp[3] + dp[1]) \mod 1000000007
\]
\[
dp[4] = (2 \times 5 + 1) \mod 1000000007 = (10 + 1) \mod 1000000007 = 11
\]

#### Final `dp` array for `n = 4`:
\[
dp = [1, 1, 2, 5, 11]
\]
Thus, the number of ways to tile a `2 x 4` board is `dp[4] = 11`.

### **Summary of the Approach:**

- We are using **dynamic programming** to solve the problem by reducing it to smaller subproblems and building up the solution incrementally.
- The recurrence relation takes into account two main cases: one for vertical dominoes and one for trominoes.
- Base cases are initialized for small boards (up to `2 x 2`).
- The final result is stored in `dp[n]`, where `n` is the size of the board.

### **Code Implementation:**

```cpp
class Solution {
public:
    int numTilings(int n) {
        // Define the dp array to store the number of ways to tile a 2 x i board.
        unsigned int dp[n + 3];  // Extra space to handle the dp[i - 3] case.
        
        // Initialize base cases.
        dp[0] = 1; 
        dp[1] = 1; 
        dp[2] = 2;
        
        // Fill the dp array using the recurrence relation.
        for (int i = 3; i <= n; i++) {
            dp[i] = (2 * dp[i - 1] + dp[i - 3]) % 1000000007;
        }
        
        // Return the number of ways to tile a 2 x n board.
        return dp[n];
    }
};
```

### **Conclusion:**

- The recurrence relation `dp[i] = (2 * dp[i-1] + dp[i-3]) % 1000000007` effectively models the ways to tile the `2 x n` board by combining smaller subproblems (tiling a `2 x (i-1)` board and a `2 x (i-3)` board).
- The space complexity is **O(n)**, and the time complexity is **O(n)**, making the solution efficient even for larger values of `n`.

---

The reason for using `unsigned int` in the solution is to handle potential **integer overflow**. Here's why:

### **1. Modulo Operation:**
In this problem, we are frequently using the modulo `10^9 + 7` to keep the results within the range of standard integer types. However, before applying the modulo operation, intermediate results might exceed the range of a signed integer.

- In C++, a signed `int` has a range of **-2,147,483,648 to 2,147,483,647**.
- If the result exceeds this range, it leads to **integer overflow**, which causes undefined behavior or incorrect results.

### **2. Large Numbers:**
The recurrence relation in the problem involves multiplying numbers and summing them. For example:
- `dp[i] = (2 * dp[i-1] + dp[i-3]) % 1000000007;`
- If `dp[i-1]` and `dp[i-3]` are large enough, the multiplication and addition can result in values that are larger than the maximum value a signed `int` can hold.

### **3. Unsigned Int:**
An **unsigned int** has a range of **0 to 4,294,967,295**, which is larger than the maximum value of a signed `int`. Using `unsigned int` ensures that the intermediate results are not negatively affected by overflow and can safely store larger values before the modulo operation is applied.

- Even after performing the modulo operation (`% 1000000007`), the number is guaranteed to fit within the range of `unsigned int`.

### **4. Modulo Operation Guarantees:**
When you take modulo `1000000007`, the result will always be a positive integer between 0 and `1000000006`, which fits comfortably within the range of an `unsigned int`.

### **Summary:**
- **Using `unsigned int`** ensures that intermediate results do not overflow when performing arithmetic operations before applying the modulo.
- Since the final answer is always within the range of a 32-bit unsigned integer, using `unsigned int` is safe and avoids issues with overflow.

Thus, the decision to use `unsigned int` is made to handle large numbers properly and ensure the program behaves as expected without overflow errors.
