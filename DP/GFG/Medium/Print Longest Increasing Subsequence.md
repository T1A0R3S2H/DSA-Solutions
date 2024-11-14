### Code:
```cpp
class Solution {
public:
    int solveMem(vector<int>& arr, int n, vector<int>& dp, vector<int>& previous) {
        int maxLength = 1;
        int endIndex = 0;
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (arr[i] > arr[j] && dp[i] < dp[j] + 1) {
                    dp[i] = dp[j] + 1;
                    previous[i] = j;
                }
            }
            if (dp[i] > maxLength) {
                maxLength = dp[i];
                endIndex = i;
            }
        }
        return endIndex;
    }

    vector<int> longestIncreasingSubsequence(int n, vector<int>& arr) {
        vector<int> dp(n, 1); // dp[i] stores the length of LIS ending at i
        vector<int> previous(n, -1); // previous[i] helps trace the sequence
        
        // Calculate LIS using solveMem and get the end index of the LIS
        int endIndex = solveMem(arr, n, dp, previous);

        // Reconstruct the LIS using the end index and previous array
        vector<int> lis;
        while (endIndex != -1) {
            lis.push_back(arr[endIndex]);
            endIndex = previous[endIndex];
        }

        // Since we traced LIS backwards, reverse it
        reverse(lis.begin(), lis.end());
        return lis;
    }
};
```

### Explanation
1. **Purpose of `solveMem`**:
   - The `solveMem` function calculates the length of the Longest Increasing Subsequence (LIS) ending at each index and stores the previous index in the sequence to reconstruct the LIS later.
   
2. **Components of `solveMem`**:
   - `dp[i]`: Stores the LIS length ending at index `i`.
   - `previous[i]`: Tracks the index of the previous element in the LIS ending at `i`.
   - `maxLength`: Stores the maximum LIS length found.
   - `endIndex`: Index of the last element in the LIS with the maximum length.
   
3. **Function Flow**:
   - For each index `i` from 1 to `n-1`:
     - For each previous index `j` from 0 to `i-1`, check if `arr[i]` can extend the LIS ending at `j` (i.e., if `arr[i] > arr[j]`).
     - If `arr[i]` can extend the LIS and creates a longer subsequence, update `dp[i]` and store `j` as `previous[i]`.
   - After processing all indices, `dp` will have the LIS length ending at each index, and `previous` will have the previous indices to help reconstruct the LIS.
   - `endIndex` is returned, marking the end of the longest LIS.

4. **Reconstruction in `longestIncreasingSubsequence`**:
   - Starts at `endIndex` and follows `previous` to trace back through the LIS elements.
   - Since the elements are added from the end of the LIS, we reverse the sequence to return it in the correct order.

### Time Complexity
- **O(n^2)**: The `solveMem` function uses a nested loop where the outer loop iterates over each element, and the inner loop checks each previous element. This results in an O(n^2) complexity.

### Space Complexity
- **O(n)**: We use two auxiliary arrays, `dp` and `previous`, each of size `n`.

### Dry Run

Let’s dry run the code with an example input:

**Input**:
```cpp
n = 8;
arr = [10, 22, 9, 33, 21, 50, 41, 60];
```

1. **Initialization**:
   - `dp = [1, 1, 1, 1, 1, 1, 1, 1]` (Each element is its own subsequence)
   - `previous = [-1, -1, -1, -1, -1, -1, -1, -1]`

2. **Processing each element**:
   - For `i = 1`: 
     - `j = 0`: `arr[1] > arr[0]` (`22 > 10`) → `dp[1] = dp[0] + 1 = 2`, `previous[1] = 0`
     - **Updated**: `dp = [1, 2, 1, 1, 1, 1, 1, 1]`, `previous = [-1, 0, -1, -1, -1, -1, -1, -1]`

   - For `i = 2`: 
     - `j = 0, j = 1`: `arr[2]` is not greater than `arr[0]` or `arr[1]` → no change

   - For `i = 3`: 
     - `j = 0, j = 1, j = 2`: 
       - `arr[3] > arr[1]` (`33 > 22`) → `dp[3] = dp[1] + 1 = 3`, `previous[3] = 1`
     - **Updated**: `dp = [1, 2, 1, 3, 1, 1, 1, 1]`, `previous = [-1, 0, -1, 1, -1, -1, -1, -1]`

   - Repeat similar steps for all `i`, updating `dp` and `previous` accordingly.

3. **Result after processing**:
   - Final `dp = [1, 2, 1, 3, 2, 4, 4, 5]`
   - Final `previous = [-1, 0, -1, 1, 0, 3, 3, 5]`
   - `maxLength = 5`, `endIndex = 7`

4. **Reconstructing LIS**:
   - Start from `endIndex = 7` and follow `previous`:
     - `arr[7] = 60`
     - `arr[previous[7]] = 50`
     - `arr[previous[5]] = 33`
     - `arr[previous[3]] = 22`
     - `arr[previous[1]] = 10`
   - LIS in reverse: `[60, 50, 33, 22, 10]`
   - After reversing: `[10, 22, 33, 50, 60]`

**Output**:
```cpp
[10, 22, 33, 50, 60]
```
