### Code
```cpp
class Solution {
public:
    int solveRec(vector<int> &nums,int &S,int index,int sum){
        if(index==nums.size()){
            if(sum==S) return 1;
            return 0;
        }
        int count=0;
        count+=solveRec(nums, S, index+1, sum-nums[index]);
        count+=solveRec(nums, S, index+1, sum+nums[index]);
        return count;
    }

    int solveMem(vector<int> &nums, int target, int index, int sum, vector<vector<int>> &dp) {
        if (index==nums.size()) {
            if (sum==target) return 1;
            else return 0;
        }
        if (dp[index][sum+1000]!=-1) return dp[index][sum+1000];

        int add=solveMem(nums, target, index+1, sum+nums[index], dp);
        int subtract=solveMem(nums, target, index+1, sum-nums[index], dp);
        dp[index][sum+1000]=add+subtract;
        return dp[index][sum+1000];
    }

    int findTargetSumWays(vector<int>& nums, int S) {
        // return solveRec(nums, S, 0, 0);
        vector<vector<int>> dp(nums.size(), vector<int>(2001, -1));
        return solveMem(nums, S, 0, 0, dp);
    }
};
```
### Explanation

The problem requires finding the number of ways to assign `+` or `-` signs to elements in the `nums` array so that their sum equals the `target`.

---

### Code Walkthrough

#### `solveMem` Function
1. **Base Case**:
   - If `index == nums.size()` (we've processed all elements in the array):
     - Return `1` if `sum == target`.
     - Otherwise, return `0`.

2. **Memoization**:
   - Use a `dp` table to store results for states `(index, sum)`.
   - Since `sum` can be negative, the index in the `dp` table is adjusted using an offset of `+1000` (to map `[-1000, 1000]` to `[0, 2000]`).

3. **Recursive Step**:
   - Add the current element: `solveMem(nums, target, index + 1, sum + nums[index], dp)`.
   - Subtract the current element: `solveMem(nums, target, index + 1, sum - nums[index], dp)`.

4. **Combine Results**:
   - The total ways for the current state is the sum of both possibilities (adding and subtracting the current element).

---

### Time Complexity
- **Number of States**:
  - The function explores all states of `(index, sum)`, where:
    - `index` ranges from `0` to `nums.size() - 1` (\(n\) states).
    - `sum` ranges from `-1000` to `1000` (\(2001\) states).
  - Total states: \(O(n \cdot 2001)\).

- **Computation per State**:
  - Each state performs \(O(1)\) work to compute the result.

**Overall Time Complexity**: \(O(n \cdot 2001) = O(n \cdot 2000)\).

---

### Space Complexity
1. **Memoization Table**:
   - The `dp` table stores results for \(n \cdot 2001\) states.
   - Space: \(O(n \cdot 2001) = O(n \cdot 2000)\).
   
2. **Recursion Stack**:
   - Depth of the recursion tree is at most \(n\).
   - Space: \(O(n)\).

**Overall Space Complexity**: \(O(n \cdot 2000)\).

---

### Dry Run
Input: `nums = [1, 1, 1, 1, 1]`, `target = 3`.

#### Initialization:
- `dp` is a `5 x 2001` table initialized to `-1`.

#### Steps:

1. Call: `solveMem(nums, 3, 0, 0, dp)`  
   - Add: `solveMem(nums, 3, 1, 1, dp)`  
   - Subtract: `solveMem(nums, 3, 1, -1, dp)`

---

2. Call: `solveMem(nums, 3, 1, 1, dp)`  
   - Add: `solveMem(nums, 3, 2, 2, dp)`  
   - Subtract: `solveMem(nums, 3, 2, 0, dp)`

---

3. Call: `solveMem(nums, 3, 2, 2, dp)`  
   - Add: `solveMem(nums, 3, 3, 3, dp)`  
   - Subtract: `solveMem(nums, 3, 3, 1, dp)`

---

4. Call: `solveMem(nums, 3, 3, 3, dp)`  
   - Add: `solveMem(nums, 3, 4, 4, dp)`  
   - Subtract: `solveMem(nums, 3, 4, 2, dp)`

---

5. Call: `solveMem(nums, 3, 4, 4, dp)`  
   - Base case: `index == 5`.
   - `sum = 4 != 3`, return `0`.

---

6. Call: `solveMem(nums, 3, 4, 2, dp)`  
   - Base case: `index == 5`.
   - `sum = 2 != 3`, return `0`.

---

Backtrack and accumulate results:

- `solveMem(nums, 3, 3, 3, dp) = 0 + 0 = 0`.

---

Continue similar steps for all calls until:

Final Result: `solveMem(nums, 3, 0, 0, dp) = 5`.

---

### Output:
The number of ways to assign symbols to make the sum equal to `target` is **5**.
