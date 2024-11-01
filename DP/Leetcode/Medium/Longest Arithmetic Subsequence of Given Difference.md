![image](https://github.com/user-attachments/assets/2f7fa70b-b17b-40db-9fc3-17ccd27ea35f)
### **Code:**
```cpp
class Solution {
public:
    // Pure Recursion
    int solveRec(int index, int difference, vector<int>& arr) {
        int ans=1;
        for (int j=index-1; j>=0; j--) {
            if (arr[index]-arr[j]==difference) {
                ans=max(ans, 1+solveRec(j, difference, arr));
            }
        }
        return ans;
    }

    // Recursion + Memoization
    int solveMem(int index, int difference, vector<int>& arr, unordered_map<int, unordered_map<int, int>>& dp) {
        if (dp.count(index) && dp[index].count(difference)) return dp[index][difference];
        int ans=1;
        for (int j=index-1; j>=0; j--) {
            if (arr[index]-arr[j]==difference) {
                ans=max(ans, 1+solveMem(j, difference, arr, dp));
            }
        }
        return dp[index][difference]=ans;
    }

    // Tabulation
    int solveTab(vector<int>& arr, int difference) {
        unordered_map<int, int> dp;
        int maxLength=1;
        for (int num:arr) {
            dp[num]=dp[num-difference]+1;
            maxLength=max(maxLength, dp[num]);
        }
        return maxLength;
    }

    // Main function calling all three methods
    int longestSubsequence(vector<int>& arr, int difference) {
        int n=arr.size();

        // // 1. Pure Recursive solution
        // int maxLengthRec = 1;
        // for (int i = 0; i < n; i++) {
        //     maxLengthRec = max(maxLengthRec, solveRec(i, difference, arr));
        // }

        // // 2. Memoized Recursive solution
        // unordered_map<int, unordered_map<int, int>> memo;
        // int maxLengthMem = 1;
        // for (int i = 0; i < n; i++) {
        //     maxLengthMem = max(maxLengthMem, solveMem(i, difference, arr, memo));
        // }

        // 3. Tabulation solution
        int maxLengthTab=solveTab(arr, difference);
        return maxLengthTab;
    }
};

```

### ETSD

#### `solveRec`
- **Code**: `solveRec` recursively checks each subsequence by comparing pairs and exploring valid sequences backward.
- **Explanation**: For each `index`, it looks at prior elements to find any with a matching `difference`, forming subsequences.
- **Time Complexity**: \(O(2^n)\), as it explores each subset.
- **Space Complexity**: \(O(n)\) recursive depth.
- **Dry Run**: For `arr = [1,2,3,4]` and `difference = 1`, `solveRec` starts from each element and tries to extend the sequence backward, finding the longest subsequence as `[1,2,3,4]`.

#### `solveMem`
- **Code**: `solveMem` builds upon `solveRec` by adding memoization with `dp[index][difference]` to avoid redundant calculations.
- **Explanation**: This caches intermediate results for subsequences at each `index` and `difference`, reducing overlapping subproblems.
- **Time Complexity**: \(O(n^2)\) in the worst case.
- **Space Complexity**: \(O(n^2)\) for the memoization table.
- **Dry Run**: For `arr = [1,2,3,4]`, calls to `solveMem` for each `index` memoize the longest subsequence length, producing `[1,2,3,4]` efficiently.

#### `solveTab`
- **Code**: `solveTab` uses a hash map to store the maximum length of subsequences for each element based on `difference`.
- **Explanation**: For each element, it calculates the maximum subsequence length that ends at that element by checking if there is a previous element matching `element - difference`.
- **Time Complexity**: \(O(n)\), as it iterates through the array once.
- **Space Complexity**: \(O(n)\) for the hash map.
- **Dry Run**: For `arr = [1,2,3,4]`, it sequentially updates lengths in `dp` using prior elements, producing the longest subsequence `[1,2,3,4]` in one pass.


### **Dry Run:**
Let’s do a dry run of `solveTab` for the example `arr = [1, 2, 4, 3, 5, 7, 8]` with `difference = 2`. This example has more irregular placements, so it’s a good choice to see how the function handles non-continuous subsequences.

### Initial Setup:
- **Array**: `arr = [1, 2, 4, 3, 5, 7, 8]`
- **Difference**: `difference = 2`
- **Goal**: Find the length of the longest subsequence with a difference of `2` between consecutive elements.

### Code Recap for `solveTab`:
The function initializes an empty hash map `dp` where each key is an element in `arr`, and the value is the length of the longest subsequence ending at that element. It also keeps track of the `maxLength` of any such subsequence.

```cpp
int solveTab(vector<int>& arr, int difference) {
    unordered_map<int, int> dp;
    int maxLength = 1;
    for (int num : arr) {
        dp[num] = dp[num - difference] + 1;
        maxLength = max(maxLength, dp[num]);
    }
    return maxLength;
}
```

### Step-by-Step Dry Run:

**1. Initialization**
- `dp` starts as an empty map: `{}`.
- `maxLength` is initialized to `1`.

**2. Iteration through `arr`**
   We iterate over each element `num` in `arr`.

#### First Iteration: `num = 1`
- We calculate `num - difference`, which is `1 - 2 = -1`.
- `dp[-1]` does not exist in `dp`, so `dp[-1]` is considered `0`.
- Update `dp[1] = dp[-1] + 1 = 0 + 1 = 1`.
- `maxLength` remains `1`.
- `dp` now looks like this: `{1: 1}`.

#### Second Iteration: `num = 2`
- Calculate `num - difference`, which is `2 - 2 = 0`.
- `dp[0]` does not exist in `dp`, so `dp[0]` is `0`.
- Update `dp[2] = dp[0] + 1 = 0 + 1 = 1`.
- `maxLength` remains `1`.
- `dp` now looks like this: `{1: 1, 2: 1}`.

#### Third Iteration: `num = 4`
- Calculate `num - difference`, which is `4 - 2 = 2`.
- `dp[2]` exists in `dp` with a value of `1`.
- Update `dp[4] = dp[2] + 1 = 1 + 1 = 2`.
- Update `maxLength` to `2` since `max(1, dp[4]) = 2`.
- `dp` now looks like this: `{1: 1, 2: 1, 4: 2}`.

#### Fourth Iteration: `num = 3`
- Calculate `num - difference`, which is `3 - 2 = 1`.
- `dp[1]` exists in `dp` with a value of `1`.
- Update `dp[3] = dp[1] + 1 = 1 + 1 = 2`.
- `maxLength` remains `2` since `max(2, dp[3]) = 2`.
- `dp` now looks like this: `{1: 1, 2: 1, 4: 2, 3: 2}`.

#### Fifth Iteration: `num = 5`
- Calculate `num - difference`, which is `5 - 2 = 3`.
- `dp[3]` exists in `dp` with a value of `2`.
- Update `dp[5] = dp[3] + 1 = 2 + 1 = 3`.
- Update `maxLength` to `3` since `max(2, dp[5]) = 3`.
- `dp` now looks like this: `{1: 1, 2: 1, 4: 2, 3: 2, 5: 3}`.

#### Sixth Iteration: `num = 7`
- Calculate `num - difference`, which is `7 - 2 = 5`.
- `dp[5]` exists in `dp` with a value of `3`.
- Update `dp[7] = dp[5] + 1 = 3 + 1 = 4`.
- Update `maxLength` to `4` since `max(3, dp[7]) = 4`.
- `dp` now looks like this: `{1: 1, 2: 1, 4: 2, 3: 2, 5: 3, 7: 4}`.

#### Seventh Iteration: `num = 8`
- Calculate `num - difference`, which is `8 - 2 = 6`.
- `dp[6]` does not exist in `dp`, so `dp[6]` is `0`.
- Update `dp[8] = dp[6] + 1 = 0 + 1 = 1`.
- `maxLength` remains `4` since `max(4, dp[8]) = 4`.
- `dp` now looks like this: `{1: 1, 2: 1, 4: 2, 3: 2, 5: 3, 7: 4, 8: 1}`.

### Final State:
- `dp` = `{1: 1, 2: 1, 4: 2, 3: 2, 5: 3, 7: 4, 8: 1}`, meaning:
  - The longest subsequence ending at `1` has length `1`.
  - The longest subsequence ending at `2` has length `1`.
  - The longest subsequence ending at `4` has length `2`.
  - The longest subsequence ending at `3` has length `2`.
  - The longest subsequence ending at `5` has length `3`.
  - The longest subsequence ending at `7` has length `4`.
  - The longest subsequence ending at `8` has length `1`.
- `maxLength` = `4`, representing the length of the longest arithmetic subsequence with a difference of `2`.

### Result:
- `solveTab` returns `maxLength`, which is `4`, as the final answer. The longest subsequence with a difference of `2` is `[1, 3, 5, 7]`.
