### Code

```cpp
class Solution {
public:
    long long countPairsWithSumLessThan(vector<int>& sorted_nums, int target_sum) {
        int left = 0;
        int right = sorted_nums.size() - 1;
        long long count = 0;
        
        while (left < right) {
            long long sum = sorted_nums[left] + sorted_nums[right];
            if (sum < target_sum) {
                count += right - left; // All pairs (left, right), (left, right-1), ..., (left, left+1) are valid.
                left++;
            } else {
                right--;
            }
        }
        
        return count;
    }

    long long countFairPairs(vector<int>& nums, int lower, int upper) {
        sort(nums.begin(), nums.end());
        return countPairsWithSumLessThan(nums, upper + 1) - countPairsWithSumLessThan(nums, lower);
    }
};
```

### Explanation of the Process

1. **Sorting the Array**:  
   The `countFairPairs` function begins by sorting `nums`. Sorting allows us to efficiently count pairs that meet specific sum criteria using a two-pointer approach.

2. **Counting Pairs with Sum Less Than Target**:
   The helper function `countPairsWithSumLessThan` counts the number of pairs with a sum less than a specified `target_sum`. This function uses a two-pointer technique:
   
   - **Two-pointer setup**: Initialize `left` at the beginning of `sorted_nums` and `right` at the end.
   - **Moving pointers**: 
     - If the sum of `sorted_nums[left]` and `sorted_nums[right]` is less than `target_sum`, all pairs from `(left, right)` down to `(left, left+1)` are valid. This is because adding a smaller element from the left side of the array will keep the sum below `target_sum`.
     - If the sum is greater than or equal to `target_sum`, decrement `right` to try for a smaller sum.
   
3. **Final Calculation**:
   - In `countFairPairs`, the difference `countPairsWithSumLessThan(nums, upper + 1) - countPairsWithSumLessThan(nums, lower)` gives the count of pairs whose sums are between `lower` and `upper`, inclusive. 

### Time Complexity
- Sorting the array takes \(O(n \log n)\).
- Each call to `countPairsWithSumLessThan` has a time complexity of \(O(n)\) since each pointer (`left` and `right`) only moves in one direction and does not reset.
  
### Space Complexity
- The space complexity is \(O(1)\) if the sorting is done in-place, otherwise \(O(n)\) if an additional array is needed for sorting.

### Dry Run
Let’s perform a dry run of the code with the example provided:

### Example Input
```plaintext
nums = [0, 1, 7, 4, 4, 5], lower = 3, upper = 6
```

### Sorted `nums`
After sorting, `nums` becomes:
```plaintext
nums = [0, 1, 4, 4, 5, 7]
```

### Step 1: Calculate Pairs with Sum Less Than `upper + 1`
We call `countPairsWithSumLessThan(nums, upper + 1)` to count pairs with sums less than `7`.

#### `target_sum = 7`
1. **Initialize**: `left = 0`, `right = 5` (points to `7`), `count = 0`
2. **Iteration 1**:
   - `sum = nums[0] + nums[5] = 0 + 7 = 7` (not less than 7)
   - Decrement `right`: `right = 4` (points to `5`)
3. **Iteration 2**:
   - `sum = nums[0] + nums[4] = 0 + 5 = 5` (less than 7)
   - Add `right - left = 4 - 0 = 4` to `count`
   - Increment `left`: `left = 1`
   - `count = 4`
4. **Iteration 3**:
   - `sum = nums[1] + nums[4] = 1 + 5 = 6` (less than 7)
   - Add `right - left = 4 - 1 = 3` to `count`
   - Increment `left`: `left = 2`
   - `count = 7`
5. **Iteration 4**:
   - `sum = nums[2] + nums[4] = 4 + 5 = 9` (not less than 7)
   - Decrement `right`: `right = 3`
6. **Iteration 5**:
   - `sum = nums[2] + nums[3] = 4 + 4 = 8` (not less than 7)
   - Decrement `right`: `right = 2`
7. **Exit**: `left` is no longer less than `right`.

**Result**: `countPairsWithSumLessThan(nums, 7) = 7`

### Step 2: Calculate Pairs with Sum Less Than `lower`
We call `countPairsWithSumLessThan(nums, lower)` to count pairs with sums less than `3`.

#### `target_sum = 3`
1. **Initialize**: `left = 0`, `right = 5`, `count = 0`
2. **Iteration 1**:
   - `sum = nums[0] + nums[5] = 0 + 7 = 7` (not less than 3)
   - Decrement `right`: `right = 4`
3. **Iteration 2**:
   - `sum = nums[0] + nums[4] = 0 + 5 = 5` (not less than 3)
   - Decrement `right`: `right = 3`
4. **Iteration 3**:
   - `sum = nums[0] + nums[3] = 0 + 4 = 4` (not less than 3)
   - Decrement `right`: `right = 2`
5. **Iteration 4**:
   - `sum = nums[0] + nums[2] = 0 + 4 = 4` (not less than 3)
   - Decrement `right`: `right = 1`
6. **Iteration 5**:
   - `sum = nums[0] + nums[1] = 0 + 1 = 1` (less than 3)
   - Add `right - left = 1 - 0 = 1` to `count`
   - Increment `left`: `left = 1`
   - `count = 1`
7. **Exit**: `left` is no longer less than `right`.

**Result**: `countPairsWithSumLessThan(nums, 3) = 1`

### Step 3: Calculate Fair Pairs
Using the results from Step 1 and Step 2:
```plaintext
countFairPairs(nums, lower, upper) = countPairsWithSumLessThan(nums, 7) - countPairsWithSumLessThan(nums, 3)
                                   = 7 - 1
                                   = 6
```

### Final Result
The number of fair pairs is `6`, which matches the expected output.
