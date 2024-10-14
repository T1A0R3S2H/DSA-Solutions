### 1. Recursive Approach

In this approach, we directly implement the recursive relation without any optimization like memoization or tabulation.

#### Code:

```cpp
class Solution {
public:
    const int MOD = 1e9 + 7;
    
    long long disarrange(int n) {
        // Base cases
        if (n == 1) return 0;
        if (n == 2) return 1;

        // Recursive relation
        return (n - 1) * (disarrange(n - 1) + disarrange(n - 2)) % MOD;
    }
};
```

### **Explanation**:
- **Base Case**:
  - \( D(1) = 0 \): No derangement possible with 1 element.
  - \( D(2) = 1 \): There is exactly 1 derangement for 2 elements.
  
- **Recursive Case**:
  - For \( n \geq 3 \), the number of derangements is calculated using:
    \[
    D(n) = (n - 1) \times (D(n - 1) + D(n - 2)) \mod (10^9 + 7)
    \]
  
- **Time Complexity**: \( O(2^n) \) due to overlapping recursive calls.
- **Space Complexity**: \( O(n) \) due to the recursive call stack.

---

### 2. Recursive with Memoization

Memoization is a technique to store previously computed values, thereby avoiding redundant calculations in the recursion process.

#### Code:

```cpp
class Solution {
public:
    const int MOD = 1e9 + 7;
    
    long long disarrangeHelper(int n, vector<long long>& memo) {
        // Base cases
        if (n == 1) return 0;
        if (n == 2) return 1;

        // If result is already computed, return it
        if (memo[n] != -1) return memo[n];

        // Recursive relation with memoization
        memo[n] = ((n - 1) * (disarrangeHelper(n - 1, memo) + disarrangeHelper(n - 2, memo)) % MOD) % MOD;

        return memo[n];
    }
    
    long long disarrange(int n) {
        // Memoization table initialized to -1
        vector<long long> memo(n + 1, -1);
        return disarrangeHelper(n, memo);
    }
};
```

### **Explanation**:
- **Memoization**: We use a `memo` array to store the previously calculated results for each \( n \). This ensures we don't recompute the same values multiple times.
  
- **Base Case**: 
  - \( D(1) = 0 \), \( D(2) = 1 \).
  
- **Recursive Case**: 
  - Compute \( D(n) \) only if it has not been computed already.

- **Time Complexity**: \( O(n) \) because each value is calculated once and stored.
- **Space Complexity**: \( O(n) \) due to the memoization table and recursion stack.

---

### 3. Tabulation Approach

In tabulation, we use a bottom-up dynamic programming approach to iteratively compute the results and store them in a table.

#### Code:

```cpp
class Solution {
public:
    const int MOD = 1e9 + 7;
    
    long long disarrange(int n) {
        // Base cases
        if (n == 1) return 0;
        if (n == 2) return 1;

        // dp[i] will store the number of derangements for i elements
        vector<long long> dp(n + 1);

        // Base values
        dp[0]=1;
        dp[1] = 0;
        dp[2] = 1;

        // Fill the table using the recurrence relation
        for (int i = 3; i <= n; i++) {
            dp[i] = ((i - 1) * (dp[i - 1] + dp[i - 2]) % MOD) % MOD;
        }

        return dp[n];
    }
};
```

### **Explanation**:
- **Base Case**:
  - \( dp[1] = 0 \), \( dp[2] = 1 \).
  
- **Tabulation**: 
  - We fill the `dp` array iteratively from \( 3 \) to \( n \) using the recurrence relation:
    \[
    D(n) = (n - 1) \times (D(n - 1) + D(n - 2)) \mod (10^9 + 7)
    \]

- **Time Complexity**: \( O(n) \) since we iterate once from 3 to \( n \).
- **Space Complexity**: \( O(n) \) due to the `dp` array.

---

### Summary of Approaches:
1. **Recursion**: Elegant but inefficient due to overlapping subproblems; time complexity is exponential.
2. **Recursion with Memoization**: Uses a memoization table to avoid redundant calculations; efficient and uses recursion.
3. **Tabulation**: Efficient and avoids recursion; uses a bottom-up dynamic programming approach.

All approaches return the number of derangements modulo \(10^9 + 7\).
