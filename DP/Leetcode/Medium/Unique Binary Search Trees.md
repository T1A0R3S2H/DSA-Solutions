### C++ Code
```cpp
class Solution {
public:
    int solveRec(int n){
        if(n<=1) return 1;
        int ans=0;
        for(int i=1; i<=n; i++){
            ans+=solveRec(i-1)*solveRec(n-i);
        }
        return ans;
    }

    int solveMem(int n, vector<int>&dp){
        if(n<=1) return 1;
        if(dp[n]!=-1) return dp[n];
        int ans=0;
        for(int i=1; i<=n; i++){
            ans+=solveMem(i-1, dp)*solveMem(n-i, dp);
        }
        return dp[n]=ans;
    }

    int solveTab(int n) {
        vector<int> dp(n+1, 0);
        dp[0]=1;
        dp[1]=1;
        for (int nodes=2; nodes<=n; nodes++) {
            int ans=0;
            for (int i=1; i<=nodes; i++) {
                ans+=dp[i-1]*dp[nodes-i];
            }
            dp[nodes]=ans;
        }
        return dp[n];
    }

    int numTrees(int n) {
        // return solveRec(n);

        // vector<int>dp(n+1, -1);
        // return solveMem(n, dp);

        return solveTab(n);
    }
};
```

### Explanation
- **`solveRec`**: This function calculates the number of unique BSTs using pure recursion. It recursively counts the unique BSTs for all left and right subtrees for each possible root, from `1` to `n`.
- **`solveMem`**: This is the memoized version of the recursive function, storing intermediate results in `dp` to avoid redundant calculations.
- **`solveTab`**: The tabulation approach builds the solution iteratively by filling in values for `dp[0]` to `dp[n]`. For each node count `nodes`, it iterates through possible roots and sums the products of the left and right subtree counts stored in `dp`.
- **`numTrees`**: This function decides which approach to use. In this case, it uses `solveTab` for optimal performance.

### Time Complexity
- `solveRec`: \(O(2^n)\) due to overlapping recursive calls.
- `solveMem`: \(O(n^2)\), as it computes each unique subtree count only once.
- `solveTab`: \(O(n^2)\), due to the nested loops iterating through nodes and roots.

### Space Complexity
- `solveRec`: \(O(n)\) due to the recursive call stack.
- `solveMem`: \(O(n)\) for the `dp` array and recursion stack.
- `solveTab`: \(O(n)\) for the `dp` array.

### Dry Run (for `solveTab`)
Consider `n = 3`:
1. Initialize `dp[0] = 1` and `dp[1] = 1`.
2. For `nodes = 2`:  
   - Root at `1`: Left subtree = `dp[0]`, Right subtree = `dp[1]`, contributing `1 * 1 = 1`.
   - Root at `2`: Left subtree = `dp[1]`, Right subtree = `dp[0]`, contributing `1 * 1 = 1`.  
   - `dp[2] = 2`.
3. For `nodes = 3`:  
   - Root at `1`: Left = `dp[0]`, Right = `dp[2]`, contributing `1 * 2 = 2`.
   - Root at `2`: Left = `dp[1]`, Right = `dp[1]`, contributing `1 * 1 = 1`.
   - Root at `3`: Left = `dp[2]`, Right = `dp[0]`, contributing `2 * 1 = 2`.  
   - `dp[3] = 5`.  
4. Result: `dp[3] = 5`.
