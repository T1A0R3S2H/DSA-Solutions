
# 1. Recursive Solution
### Code
```cpp
class Solution {
  private:
    long long solve(int M, int N, int X) {
        if (X<0) return 0;
        if (N==0 && X!=0) return 0;
        if (N!=0 && X==0) return 0;
        if (N==0 && X==0) return 1;
        long long ans=0;
        for (int i=1; i<=M; i++) {
            ans+=solve(M, N-1, X-i);
        }
        return ans;
    }
```
#### Explanation
The `solve` function recursively calculates the number of ways to get a sum \( X \) using \( N \) dice, each with \( M \) faces. It handles base cases:
- If \( X < 0 \), there are no valid combinations.
- If \( N = 0 \) and \( X \neq 0 \), no dice means no sum.
- If \( N \neq 0 \) and \( X = 0 \), it’s not possible to achieve a sum without any dice.
- If both \( N \) and \( X \) are zero, there's exactly one way (doing nothing).

For each face value from 1 to \( M \), it recursively calls itself to compute the number of ways to achieve the remaining sum with one less die.

#### Time Complexity
The time complexity is \( O(M^N) \) due to the branching factor \( M \) for each of the \( N \) dice.

#### Space Complexity
The space complexity is \( O(N) \) for the recursion stack.

#### Dry Run
- For \( M = 6, N = 3, X = 12 \):
  - Start with \( (M=6, N=3, X=12) \).
  - For \( i = 1 \): Calls \( solve(6, 2, 11) \).
  - For \( i = 2 \): Calls \( solve(6, 2, 10) \), and so on.
  - Continues until reaching base cases, accumulating counts for each valid path.

---
# 2. Recursion + Memoisation
```cpp
    long long solveMem(int M, int N, int X, vector<vector<long long>>& dp) {
        if (X<0) return 0;
        if (N==0 && X!=0) return 0;
        if (N!=0 && X==0) return 0;
        if (N==0 && X==0) return 1;
        if (dp[N][X]!=-1) {
            return dp[N][X];
        }
        long long ans=0;
        for (int i=1; i<=M; i++) {
            ans+=solveMem(M, N-1, X-i, dp);
        }
        return dp[N][X]=ans;
    }
```
#### Explanation
The `solveMem` function implements the same logic as `solve`, but uses memoization to store already computed values in the `dp` table, which reduces the number of repeated calculations. It checks if the result for the current state \( (N, X) \) has already been computed and stored in `dp`.

#### Time Complexity
The time complexity is \( O(N \times X) \) since we only compute each state once.

#### Space Complexity
The space complexity is \( O(N \times X) \) for the `dp` table.

#### Dry Run
- For \( M = 6, N = 3, X = 12 \):
  - The function first checks if \( dp[3][12] \) is computed. If not, it computes it.
  - Calls are made recursively, but results are stored in the `dp` table to avoid redundant calculations.

---
# 3. Tabulation (Bottom-Up)
```cpp
    long long solveTab(int M, int N, int X) {
        vector<vector<long long>> dp(N + 1, vector<long long>(X + 1, 0));
        dp[0][0]=1;
        for (int dice=1; dice<=N; dice++) {
            for (int target=1; target<=X; target++) {
                long long ans=0;
                for (int i=1; i<=M; i++) {
                    if (target-i>=0) {
                        ans+=dp[dice-1][target-i];
                    }
                }
                dp[dice][target]=ans;
            }
        }
        return dp[N][X];
    }
```
#### Explanation
The `solveTab` function uses a bottom-up approach (tabulation) to build the solution iteratively. It initializes a 2D `dp` array, where `dp[dice][target]` stores the number of ways to get the sum `target` using `dice` number of dice.

#### Time Complexity
The time complexity is \( O(N \times X \times M) \) since for each combination of dice and target, it loops through all possible faces.

#### Space Complexity
The space complexity is \( O(N \times X) \) for the `dp` table.

#### Dry Run
- For \( M = 6, N = 3, X = 12 \):
  - Starts filling `dp[0][0] = 1`.
  - Iteratively computes values for all possible sums and dice counts.
  - Eventually returns `dp[3][12]`.

---
# 4. Space optimised
```cpp
    long long solveSO(int M, int N, int X) {
        vector<long long> prev(X+1, 0);
        vector<long long> curr(X+1, 0);
        prev[0]=1;
        for (int dice=1; dice<=N; dice++) {
            for (int target=1; target<=X; target++) {
                long long ans=0;
                for (int i=1; i<=M; i++) {
                    if (target-i>=0) {
                        ans+=prev[target-i];
                    }
                }
                curr[target]=ans;
            }
            prev=curr;
        }
        return prev[X];
    }
```
#### Explanation
The `solveSO` function optimizes space further by using two 1D arrays, `prev` and `curr`, to store intermediate results. It only keeps track of the results of the previous dice count while iterating through the target sums.

#### Time Complexity
The time complexity is \( O(N \times X \times M) \), similar to tabulation, since it also considers all combinations of dice and target sums.

#### Space Complexity
The space complexity is \( O(X) \) for the two arrays used.

#### Dry Run
- For \( M = 6, N = 3, X = 12 \):
  - Initializes `prev[0] = 1`.
  - Fills `prev` and `curr` iteratively for each dice count.
  - Returns `prev[12]` as the final result.

---

### Public Function
```cpp
  public:
    long long noOfWays(int m, int n, int x) {
        if (x<n || x>(n*m)) return 0;
        
        // Uncomment one of the following calls to use the desired method:
        // return solve(m, n, x); // Recursion
        // vector<vector<long long>> dp(n+1, vector<long long>(x+1, -1));
        // return solveMem(m, n, x, dp); // Memoization
        // return solveTab(m, n, x); // Tabulation
        return solveSO(m, n, x); // Space Optimization
    }
```
#### Explanation
The `noOfWays` function serves as the entry point for the solution, checking if the sum \( x \) is possible with \( n \) dice of \( m \) faces. If not, it returns 0. The function can call any of the implemented methods based on the desired approach.

#### Time Complexity
The time complexity depends on the chosen method:
- Recursion: \( O(M^N) \)
- Memoization: \( O(N \times X) \)
- Tabulation: \( O(N \times X \times M) \)
- Space Optimization: \( O(N \times X \times M) \)

#### Space Complexity
The space complexity depends on the chosen method:
- Recursion: \( O(N) \)
- Memoization: \( O(N \times X) \)
- Tabulation: \( O(N \times X) \)
- Space Optimization: \( O(X) \)

#### Dry Run
- For \( m = 6, n = 3, x = 12 \):
  - It checks if 12 is within the valid range for 3 dice.
  - Calls `solveSO`, leading to a calculation of the number of ways to get the sum 12.
  - Returns 25 as the final output.
