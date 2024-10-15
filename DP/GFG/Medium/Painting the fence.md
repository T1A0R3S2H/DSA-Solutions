ALL 4 APPROACHES - EASY TO UNDERSTAND
1) RECURSIVE

2) RECURSIVE + MEMOIZATION

3) TABULATION

4) SPACE OPTIMIZATION

---

### **Code**
```cpp
class Solution{
    public:
    long long mod = 1e9+7;
    long long mul(long long a, long long b){
        return ((a%mod)*(b%mod))%mod;
    }
    
    long long add(long long a, long long b){
        return ((a%mod)+(b%mod))%mod;
    }
    
    long long solve(int n, int k){
        if(n==1) return k;
        if(n==2) return mul(k,k);
        long long first = mul(solve(n-2,k),k-1);
        long long second = mul(solve(n-1,k),k-1);
        return add(first,second);
    }
    
    long long solveMem(int n, int k, vector<long long>&dp){
        if(n==1) return (long long)k;
        if(n==2) return (long long)mul(k,k);
        if(dp[n]!=-1) return dp[n]%mod;
        long long first = mul(solveMem(n-2,k,dp),k-1);
        long long second = mul(solveMem(n-1,k,dp),k-1);
        return dp[n] = add(first,second);
    }
    
    long long solveTab(int n, int k){
        vector<long long>dp(n+1,0);
        dp[1] = k;
        dp[2] = mul(k,k);
        for(int i=3;i<=n;i++){
            dp[i] = add(mul((k-1),(dp[i-2])) , mul((k-1),(dp[i-1])));
        }
        return dp[n]%mod;
    }
    
    long long solveSO(int n, int k){
        long long prev2 = k;               //prev2=>dp[1]
        long long prev1 = mul(k,k);    //prev1=>dp[2]
        if(n==1) return prev2;
        if(n==2) return prev1;
        for(int i=3;i<=n;i++){
            long long ans = add(mul(prev1,k-1),mul(prev2,k-1)); 
            prev2 = prev1%mod;
            prev1 = ans%mod;
        }
        return prev1%mod;
    }
    
    long long countWays(int n, int k){
        //APPROACH 1: RECURSION
        // return solve(n,k);
        
        //APPROACH 2: RECURSION + MEMOIZATION
        // vector<long long>dp(n+1,-1);
        // return solveMem(n,k,dp);
        
        //APPROACH 3: TABULATION
        // return solveTab(n,k);
        
        //APPROACH 4: SPACE OPTIMIZATION
        return solveSO(n,k);
    }
};
```

### Explanation:
- **Recursive Approach (solve):** It checks if `n` is 1 or 2 and handles those cases directly. Otherwise, it computes the solution by considering two cases:
  - The last two posts are different (use `solve(n-2)`).
  - The last two posts are the same (use `solve(n-1)`).
  
- **Memoization (solveMem):** Similar to the recursive approach but uses a `dp` array to store intermediate results to avoid recomputation.

- **Tabulation (solveTab):** Builds the solution iteratively from the bottom up, storing results in a `dp` array.

- **Space Optimization (solveSO):** Instead of using an array, it optimizes space by only storing the last two results (`prev1` and `prev2`).

### Time Complexity:
- **Recursive Approach (solve):** O(2^N) (exponential, since it solves subproblems repeatedly without memoization).
- **Memoization Approach (solveMem):** O(N) (linear, since it solves each subproblem once and stores it).
- **Tabulation Approach (solveTab):** O(N) (linear, as it builds the solution iteratively).
- **Space-Optimized Approach (solveSO):** O(N) (linear, similar to tabulation but with constant space).

### Space Complexity:
- **Recursive Approach (solve):** O(N) due to recursion stack.
- **Memoization Approach (solveMem):** O(N) due to recursion stack and `dp` array.
- **Tabulation Approach (solveTab):** O(N) due to `dp` array.
- **Space-Optimized Approach (solveSO):** O(1), constant space.

### Dry Run:
For `n = 3` and `k = 2`:
- Using space optimization:
  - `prev2 = 2`, `prev1 = 4`.
  - For `i = 3`: `ans = (1*4 + 1*2) % mod = 6`.
  - Result: `6`.

For `n = 2` and `k = 4`:
- Using space optimization:
  - `prev2 = 4`, `prev1 = 16`.
  - Result: `16`.
