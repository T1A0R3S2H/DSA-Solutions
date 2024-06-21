## Method 1 (Swapping)
This approach is a backtracking algorithm to generate all possible permutations of a given list of integers. Let's break down the algorithm step-by-step using the provided code and an example.

### Code Explanation

1. **Private Helper Function (`solve`)**:
   - **Parameters**:
     - `vector<int> nums`: The current permutation of the list.
     - `vector<vector<int>>& ans`: A reference to the result list where all permutations will be stored.
     - `int index`: The current index to be processed.
   - **Base Case**: If `index` is greater than or equal to the size of `nums`, it means we have a complete permutation, and we add this permutation to `ans`.
   - **Recursive Case**: For each index `j` from `index` to the end of `nums`:
     - Swap the elements at positions `index` and `j`.
     - Recursively call `solve` with the next index (`index + 1`).
     - Backtrack by swapping the elements at positions `index` and `j` back to their original positions to restore the state for the next iteration.

2. **Public Function (`permute`)**:
   - Initializes an empty result vector `ans`.
   - Calls the `solve` function starting from the first index (0).
   - Returns the result vector `ans` containing all the permutations.

### Example Walkthrough

Let's use the example `nums = [1, 2, 3]` to illustrate how the algorithm works.

- **Initial Call**:
  ```cpp
  permute([1, 2, 3])
  ```
  This initializes `ans` as an empty vector and calls `solve([1, 2, 3], ans, 0)`.

- **First Call to `solve`** (`index = 0`):
  - Swaps and recursive calls:
    - Swap `nums[0]` and `nums[0]` -> `solve([1, 2, 3], ans, 1)`
    - Swap `nums[0]` and `nums[1]` -> `solve([2, 1, 3], ans, 1)`
    - Swap `nums[0]` and `nums[2]` -> `solve([3, 2, 1], ans, 1)`

- **Second Call to `solve`** (`index = 1`):
  For each call in the previous step, similar swaps and recursive calls happen:
  - For `solve([1, 2, 3], ans, 1)`:
    - Swap `nums[1]` and `nums[1]` -> `solve([1, 2, 3], ans, 2)`
    - Swap `nums[1]` and `nums[2]` -> `solve([1, 3, 2], ans, 2)`
  - For `solve([2, 1, 3], ans, 1)`:
    - Swap `nums[1]` and `nums[1]` -> `solve([2, 1, 3], ans, 2)`
    - Swap `nums[1]` and `nums[2]` -> `solve([2, 3, 1], ans, 2)`
  - For `solve([3, 2, 1], ans, 1)`:
    - Swap `nums[1]` and `nums[1]` -> `solve([3, 2, 1], ans, 2)`
    - Swap `nums[1]` and `nums[2]` -> `solve([3, 1, 2], ans, 2)`

- **Third Call to `solve`** (`index = 2`):
  At this point, `index` equals the size of the array, so the current permutation is added to `ans`.

### Permutation Generation
By following the swaps and recursive calls, all permutations are generated:

1. `solve([1, 2, 3], ans, 2)` -> `[1, 2, 3]`
2. `solve([1, 3, 2], ans, 2)` -> `[1, 3, 2]`
3. `solve([2, 1, 3], ans, 2)` -> `[2, 1, 3]`
4. `solve([2, 3, 1], ans, 2)` -> `[2, 3, 1]`
5. `solve([3, 2, 1], ans, 2)` -> `[3, 2, 1]`
6. `solve([3, 1, 2], ans, 2)` -> `[3, 1, 2]`

### Summary

The algorithm effectively explores all possible swaps of elements using backtracking, ensuring that every permutation is generated and stored in `ans`. The backtracking step (swapping back to the original state) ensures that the state is restored for the next iteration.

```bash
class Solution {
private:
    void solve (vector<int> nums, vector<vector<int>>&ans, int index){
        //base case
        if (index>=nums.size()){
            ans.push_back(nums);
            return;
        }

        for (int j=index;j<nums.size();j++){
            swap(nums[index], nums[j]);
            solve(nums, ans, index+1);
            //backtrack
            swap(nums[index], nums[j]);
        }
    }
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> ans;
        int index=0;
        solve (nums, ans, index);
        return ans;
    }
};
```
