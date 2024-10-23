### 👉Code

```cpp
class Solution {
public:
    bool check(vector<int>& base1, vector<int>& base2) {
        if (base2[0] <= base1[0] && base2[1] <= base1[1] &&
            base2[2] <= base1[2]) {
            return true;
        } 
        else
            return false;
    }

    int solve(vector<vector<int>>& nums, int n) {
        vector<int> dp(n + 1, 0);

        for (int curr = n - 1; curr >= 0; curr--) {
            for (int prev = curr - 1; prev >= -1; prev--) {
                // INCLUDE THE HEIGHT
                int take = 0;
                if (prev == -1 || check(nums[curr], nums[prev]))
                    take = nums[curr][2] + dp[curr + 1];

                // EXCLUDE THE HEIGHT
                int notake = 0 + dp[prev + 1];

                dp[prev + 1] = max(take, notake);
            }
        }

        return dp[0];
    }

    int maxHeight(vector<vector<int>>& cuboids) {
        for (auto& i : cuboids) {
            sort(i.begin(), i.end());
        }
        sort(cuboids.begin(), cuboids.end());

        return solve(cuboids, cuboids.size());
    }
};
```


### 👉Step-by-step concise explanation

1. **Sort each cuboid's dimensions**:
   - For each cuboid, sort its dimensions to ensure it's easier to compare. This allows any dimension to be treated as width, length, or height.

2. **Sort cuboids based on dimensions**:
   - Sort all cuboids based on their first dimension (`width`), then their second (`length`), and finally their third (`height`). This ensures we can process them like in the Longest Increasing Subsequence (LIS) problem, where we only need to check if one cuboid can be stacked on top of another.

3. **Apply LIS-type dynamic programming**:
   - For each cuboid, try to find the maximum height by either:
     - **Including the current cuboid**: Check if it can be placed on top of the previous one (i.e., the width, length, and height of the current cuboid are all less than or equal to those of the previous cuboid).
     - **Excluding the current cuboid**: Skip it and take the maximum height found so far.
   - Store the maximum possible height in a DP array for each cuboid and return the maximum value from that array.

This approach ensures we find the maximum possible height of the stacked cuboids.

---
  
The line **"sorting all cuboids based on their dimensions (first width, then length, then height)"** means that when sorting the cuboids, we prioritize their dimensions in the following order:

1. **First**: Sort by the **width** of each cuboid (first element of each cuboid's sorted dimensions).
2. **Then**: If two cuboids have the same width, compare their **length** (second element of the sorted dimensions) to decide the order.
3. **Finally**: If two cuboids have the same width and length, sort by their **height** (third element of the sorted dimensions).

### Example:
If you have three cuboids:
```
cuboids = [[20, 45, 50], [37, 53, 95], [12, 23, 45]]
```

After sorting:
1. **First** by width: `12 < 20 < 37`
2. **Then** by length (if widths are equal): not applicable here since all widths are different.
3. **Finally** by height (if both width and length are equal): also not applicable here.

So, the cuboids will be sorted as:
```
[[12, 23, 45], [20, 45, 50], [37, 53, 95]]
```

The sorting process ensures that when processing the cuboids, the smaller ones (based on width, then length, then height) come first, making it easier to apply the Longest Increasing Subsequence (LIS)-like logic to stack them.

### 👉Time Complexity
- Sorting the cuboids: `O(n log n)`
- Dynamic programming loop: `O(n^2)`
Thus, the overall time complexity is `O(n^2)`.

### 👉Space Complexity
- The space complexity is `O(n)` due to the DP array.

Let's perform a **dry run** step by step based on the concise explanation using the input:

**Input:** `cuboids = [[50,45,20],[95,37,53],[45,23,12]]`

### 👉Step-by-step Dry Run

1. **Sort each cuboid's dimensions:**
   - `cuboids[0] = [20, 45, 50]`
   - `cuboids[1] = [37, 53, 95]`
   - `cuboids[2] = [12, 23, 45]`

   After sorting individual cuboid dimensions, we get:
   ```
   cuboids = [[20, 45, 50], [37, 53, 95], [12, 23, 45]]
   ```

2. **Sort cuboids based on dimensions:**
   - After sorting all cuboids based on their dimensions (first width, then length, then height):
   ```
   cuboids = [[12, 23, 45], [20, 45, 50], [37, 53, 95]]
   ```

3. **Initialize DP array**:  
   Initialize a `dp` array where `dp[i]` will store the maximum height achievable using cuboids from `0` to `i`. Initially, each cuboid's height is its own height:
   ```
   dp = [45, 50, 95]  // dp[i] = cuboids[i][2] (height of cuboid i)
   ```

4. **Dynamic Programming (LIS Logic)**:  
   - Start from the second cuboid (`i = 1`), and for each cuboid, check if it can be stacked on top of a previous cuboid (`j < i`).

   - **For `i = 1` (`cuboid = [20, 45, 50]`)**:
     - Compare with `j = 0` (`cuboid = [12, 23, 45]`):
       - Since `20 > 12`, `45 > 23`, and `50 > 45`, cuboid 1 can be placed on top of cuboid 0.
       - Update `dp[1] = max(dp[1], dp[0] + 50) = max(50, 45 + 50) = 95`
     ```
     dp = [45, 95, 95]
     ```

   - **For `i = 2` (`cuboid = [37, 53, 95]`)**:
     - Compare with `j = 0` (`cuboid = [12, 23, 45]`):
       - Since `37 > 12`, `53 > 23`, and `95 > 45`, cuboid 2 can be placed on top of cuboid 0.
       - Update `dp[2] = max(dp[2], dp[0] + 95) = max(95, 45 + 95) = 140`
     - Compare with `j = 1` (`cuboid = [20, 45, 50]`):
       - Since `37 > 20`, `53 > 45`, and `95 > 50`, cuboid 2 can be placed on top of cuboid 1.
       - Update `dp[2] = max(dp[2], dp[1] + 95) = max(140, 95 + 95) = 190`
     ```
     dp = [45, 95, 190]
     ```

5. **Final Result**:
   The maximum height is the maximum value in the `dp` array:
   ```
   max(dp) = 190
   ```

**Output:** `190`

### Conclusion
The cuboid stacking with the maximum height is:
- Cuboid 2 (`[37, 53, 95]`)
- On top of cuboid 1 (`[20, 45, 50]`)
- On top of cuboid 0 (`[12, 23, 45]`)

The total height is `95 + 50 + 45 = 190`.
