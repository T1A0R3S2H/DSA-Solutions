### Code:
```cpp
class Solution {
public:    
    int tabu(int* arr, int n) {
        if (n==0) return 0;
        if (n==1) return arr[0];

        vector<int> dp(n, 0);
        dp[0]=arr[0];
        dp[1]=max(arr[0], arr[1]);  // To avoid heap buffer overflow

        for (int i=2; i<n; i++) {
            int inc=dp[i-2]+arr[i];
            int ex=dp[i-1];
            dp[i]=max(inc, ex);
        }
        return dp[n-1];
    }
    
    int findMaxSum(int *arr, int n) {
        return tabu(arr, n);
    }
};
```

### Explanation:

The problem at hand is finding the maximum sum of non-adjacent elements in an array. This is a classic dynamic programming problem where we can either include or exclude each element in the sum to get the optimal result.

1. **Base Cases**:
   - If the array is empty (`n == 0`), the result is `0` because there's no element to sum.
   - If the array has only one element (`n == 1`), the result is the value of that element because it's the only choice.

2. **Dynamic Programming Approach**:
   - We use a `dp` array where `dp[i]` stores the maximum sum we can obtain by considering elements from index 0 to index `i`, with the condition that no two adjacent elements are selected.
   - The first element (`dp[0]`) is simply the value of the first element in the array (`arr[0]`).
   - The second element (`dp[1]`) is the maximum of the first two elements, `max(arr[0], arr[1])`, because we can either take the first or the second element but not both.
   - For each subsequent element at index `i`, we have two choices:
     - **Include** the current element: In this case, we cannot include the previous element, so we add `arr[i]` to `dp[i-2]` (which is the maximum sum including up to `i-2`).
     - **Exclude** the current element: In this case, the maximum sum up to index `i` is the same as `dp[i-1]`.
   - The recurrence relation becomes: 
     \[
     dp[i] = \max(dp[i-2] + arr[i], dp[i-1])
     \]
   - The result will be stored in `dp[n-1]` after the loop finishes.

### Time Complexity:

- The time complexity is \( O(n) \), where \( n \) is the number of elements in the array. This is because:
  - We initialize the `dp` array in \( O(n) \) time.
  - We iterate through the array exactly once in the `for` loop, processing each element in constant time.

### Space Complexity:

- The space complexity is \( O(n) \), due to the `dp` array that stores the maximum sum up to each index. The `dp` array has the same length as the input array.

### Dry Run:

Let’s walk through the algorithm with an example input array: `arr = {3, 2, 5, 10, 7}` and `n = 5`.

1. **Initialization**:
   - `dp[0] = arr[0] = 3`
   - `dp[1] = max(arr[0], arr[1]) = max(3, 2) = 3`
   - Initial `dp` array: `[3, 3, 0, 0, 0]`

2. **Iteration**:
   - For `i = 2`: 
     - Include `arr[2]`: `dp[0] + arr[2] = 3 + 5 = 8`
     - Exclude `arr[2]`: `dp[1] = 3`
     - `dp[2] = max(8, 3) = 8`
     - `dp` array: `[3, 3, 8, 0, 0]`

   - For `i = 3`: 
     - Include `arr[3]`: `dp[1] + arr[3] = 3 + 10 = 13`
     - Exclude `arr[3]`: `dp[2] = 8`
     - `dp[3] = max(13, 8) = 13`
     - `dp` array: `[3, 3, 8, 13, 0]`

   - For `i = 4`: 
     - Include `arr[4]`: `dp[2] + arr[4] = 8 + 7 = 15`
     - Exclude `arr[4]`: `dp[3] = 13`
     - `dp[4] = max(15, 13) = 15`
     - `dp` array: `[3, 3, 8, 13, 15]`

3. **Final Output**:
   - The final result is `dp[4] = 15`, which is the maximum sum of non-adjacent elements.

### Summary:

- **Explanation**: We build a dynamic programming array where each element stores the maximum sum that can be obtained by considering elements up to that point while skipping adjacent elements.
- **Time Complexity**: \( O(n) \), where \( n \) is the number of elements in the array.
- **Space Complexity**: \( O(n) \), due to the `dp` array.
- **Dry Run**: For an array `arr = {3, 2, 5, 10, 7}`, the maximum sum of non-adjacent elements is `15`.
