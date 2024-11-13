### Code
```cpp
class Solution{
public:
    int solveMem(int N, int arr[], int left, int right, vector<vector<int>>& dp){
        if(left==right) return 0;  //single matrix only, so no multiplication
        if(dp[left][right]!=-1) return dp[left][right];
        int ans=INT_MAX;
        for (int k=left; k<right; k++) {
            int cost=(arr[left-1]*arr[k]*arr[right]) + solveMem(N, arr, left, k, dp) + solveMem(N, arr, k+1, right, dp);
            ans=min(ans, cost);
        }
        return dp[left][right]=ans;
    }
    int matrixMultiplication(int N, int arr[]){
        vector<vector<int>>dp(N+1, vector<int>(N+1, -1));
        return solveMem(N, arr, 1, N-1, dp);
    }
};
```

### Explanation

The goal is to find the minimum cost of multiplying a chain of matrices with dimensions specified in the `arr` array. If the array `arr` is of size `N`, there are `N-1` matrices, where matrix `i` has dimensions `(arr[i-1] x arr[i])`. 

The function `matrixMultiplication` initializes a `dp` table to memoize the results and then calls `solveMem` to compute the answer.

**`solveMem` Function**:
- **Base Case**: If `left == right`, there’s only one matrix, so no multiplication is needed, and the cost is 0.
- **Memoization Check**: If we have already computed the minimum multiplication cost for `dp[left][right]`, we return it to avoid redundant calculations.
- **Recursive Case**:
  - We try to split the sequence of matrices between `left` and `right` at every possible `k` (where `left <= k < right`). This splits the problem into two subproblems: matrices from `left` to `k` and matrices from `k+1` to `right`.
  - **Why `k < right`**: The purpose of this range is to ensure each possible position between `left` and `right` is tested as a potential split point. Splitting at each `k` represents placing parentheses around different parts of the matrix sequence.
  - **Cost Calculation**: The cost of multiplying the two resulting matrices (from the split) is `arr[left-1] * arr[k] * arr[right]`, combined with the recursive costs for the two subproblems `solveMem(arr, left, k, dp)` and `solveMem(arr, k+1, right, dp)`.
  - **Choosing Minimum**: After calculating the cost for each split point `k`, we store the minimum cost in `ans`.
- **Store Result**: Finally, we memoize the result for `dp[left][right]` and return it.

### Time Complexity

The time complexity is \(O(N^3)\) because:
1. There are \(O(N^2)\) subproblems due to the ranges `left` and `right` that each range from `1` to `N-1`.
2. For each subproblem, we iterate through \(N\) values of `k`, making the complexity \(O(N^3)\).

### Space Complexity

The space complexity is \(O(N^2)\), primarily due to the `dp` array, which stores intermediate results for each range of matrices. This memoization table helps avoid recomputation.

### Dry Run

Consider `N = 5` and `arr = {40, 20, 30, 10, 30}`.

1. **Initial Call**: `matrixMultiplication(5, arr)` initializes `dp` and calls `solveMem(arr, 1, 4, dp)`.
2. **Recursive Split**:
   - For `solveMem(arr, 1, 4, dp)`, we try splitting at each `k` in `[1, 3]`.
   - **Split at `k = 1`**:
     - Left cost (`solveMem(arr, 1, 1, dp)`) = 0.
     - Right cost (`solveMem(arr, 2, 4, dp)`) = computed recursively.
     - Total cost for this split is `40 * 20 * 30 + cost of solveMem(arr, 2, 4, dp)`.
   - This process continues recursively, exploring each split point for each subproblem until we find the minimum cost for each range and memoize it.

The final result for `solveMem(arr, 1, 4, dp)` would be `26000`, as calculated in the example.
