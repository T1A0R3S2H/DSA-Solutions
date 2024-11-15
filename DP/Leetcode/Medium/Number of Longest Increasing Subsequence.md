### Code:
```cpp
class Solution {
public:
    int findNumberOfLIS(vector<int>& arr) {
        int n = arr.size();

        vector<int> dp(n, 1); // dp[i] stores the length of the LIS ending at arr[i]
        vector<int> ct(n, 1); // ct[i] stores the count of LIS ending at arr[i]

        int maxi = 1;

        for (int i = 0; i < n; i++) {
            for (int prev_index = 0; prev_index < i; prev_index++) {
                if (arr[prev_index] < arr[i] && dp[prev_index] + 1 > dp[i]) {
                    dp[i] = dp[prev_index] + 1;
                    ct[i] = ct[prev_index];
                } 
                else if (arr[prev_index] < arr[i] &&
                           dp[prev_index] + 1 == dp[i]) {
                    ct[i] += ct[prev_index];
                }
            }
            maxi = max(maxi, dp[i]);
        }

        int numberOfLIS = 0;
        for (int i = 0; i < n; i++) {
            if (dp[i] == maxi) {
                numberOfLIS += ct[i];
            }
        }

        return numberOfLIS;
    }
};
```


### Explanation

To solve the problem of finding the **number of longest increasing subsequences (LIS)** in the given array, we use dynamic programming to track both the length of the LIS ending at each element and the count of such subsequences. 

Here's the breakdown:

1. **Tracking LIS Lengths and Counts**:
   - We maintain two arrays:
     - `dp[i]`: Length of the longest increasing subsequence that ends at `arr[i]`.
     - `ct[i]`: Count of longest increasing subsequences of length `dp[i]` that end at `arr[i]`.
   - Initialize both arrays to `1` for each index, because each element can be an increasing subsequence of length `1` by itself.

2. **Updating `dp` and `ct` Values**:
   - For each element at index `i`, look at all previous elements at indices `j` (`0 <= j < i`):
     - If `arr[j] < arr[i]`, `arr[i]` can extend the increasing subsequence ending at `arr[j]`:
       - **If `dp[j] + 1 > dp[i]`**:
         - This indicates we found a longer increasing subsequence ending at `i`.
         - Update `dp[i] = dp[j] + 1` to reflect the longer sequence.
         - Set `ct[i] = ct[j]`, inheriting the count from `j`, as the sequence count from `j` now applies to `i`.
       - **If `dp[j] + 1 == dp[i]`**:
         - This means `arr[i]` can extend multiple subsequences to reach the same length.
         - Add `ct[j]` to `ct[i]` to account for the additional sequences ending at `i` with the same length.

3. **Finding the Maximum LIS Length**:
   - As we update `dp`, track the maximum LIS length, `maxi`.
   - After the loop, `maxi` holds the length of the longest increasing subsequence across the entire array.

4. **Summing Counts of LIS of Length `maxi`**:
   - Sum `ct[i]` for all indices `i` where `dp[i] == maxi`, which gives the number of longest increasing subsequences of the longest length.

### Time Complexity

- **Time Complexity**: \(O(n^2)\)
  - We use two nested loops: for each element at `i`, we check all previous elements `j`, resulting in \(O(n^2)\).
  
- **Space Complexity**: \(O(n)\)
  - We maintain two arrays, `dp` and `ct`, each of size \(n\), where `n` is the length of the array.

### Dry Run (Example)

Let's walk through the example `arr = [1, 3, 5, 4, 7]`:

1. **Initialize**: `dp = [1, 1, 1, 1, 1]`, `ct = [1, 1, 1, 1, 1]`, `maxi = 1`.

2. **Step-by-Step Calculation**:

   - **`i = 1` (arr[1] = 3)**:
     - Compare with `arr[0] = 1`:
       - `1 < 3`, so we can extend the subsequence ending at `arr[0]`.
       - Update `dp[1] = 2`, `ct[1] = 1` (inherited from `ct[0]`).
     - Update `maxi = 2`.
   
   - **`i = 2` (arr[2] = 5)**:
     - Compare with `arr[0] = 1`:
       - `1 < 5`, update `dp[2] = 2`, `ct[2] = 1`.
     - Compare with `arr[1] = 3`:
       - `3 < 5`, update `dp[2] = 3`, `ct[2] = 1`.
     - Update `maxi = 3`.

   - **`i = 3` (arr[3] = 4)**:
     - Compare with `arr[0] = 1`:
       - `1 < 4`, update `dp[3] = 2`, `ct[3] = 1`.
     - Compare with `arr[1] = 3`:
       - `3 < 4`, update `dp[3] = 3`, `ct[3] = 1`.
     - `maxi` remains `3`.

   - **`i = 4` (arr[4] = 7)**:
     - Compare with `arr[0] = 1`, `arr[1] = 3`, `arr[2] = 5`, and `arr[3] = 4`:
       - Update `dp[4] = 4`, `ct[4] = 2` (sum of counts from valid subsequences ending at `arr[2]` and `arr[3]`).
     - Update `maxi = 4`.

3. **Result**:
   - Sum `ct[i]` for indices where `dp[i] == maxi` (only `ct[4] = 2`), giving a result of `2`.
