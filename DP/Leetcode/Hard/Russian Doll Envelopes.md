### Code:
```cpp
class Solution {
private:
    int solve(int n, vector<int>& nums) {
        vector<int> ans;
        ans.push_back(nums[0]);
        for (int i = 1; i < n; i++) {
            if (nums[i] > ans.back()) {
                ans.push_back(nums[i]);
            } 
            else {
                int index = lower_bound(ans.begin(), ans.end(), nums[i]) - ans.begin();
                ans[index] = nums[i];
            }
        }
        return ans.size();
    }

public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        int n = envelopes.size();
        if (n == 0) return 0;
        sort(envelopes.begin(), envelopes.end(), [](vector<int>& a, vector<int>& b) {
            if (a[0] == b[0]) {
                return a[1] > b[1]; // Sort by height in descending order if widths are equal
            }
            return a[0] < b[0]; // Sort by width in ascending order
        });
        vector<int> nums;
        for (int i = 0; i < n; i++) {
            nums.push_back(envelopes[i][1]); // Collect heights
        }

        return solve(n, nums); // Solve the Longest Increasing Subsequence on heights
    }
};

```

---

### Explanation:
- **Problem Recap**: The task is to find the maximum number of envelopes that can be "Russian dolled," meaning that one envelope fits into another if both its width and height are greater.
- **Plan**:
  1. **Sorting**: First, we sort the envelopes based on their width (`w`). If two envelopes have the same width, we sort them by height (`h`) in decreasing order to avoid including envelopes with the same width in the result.
  2. **Dynamic Programming with Binary Search**: After sorting, the problem reduces to finding the longest increasing subsequence (LIS) on the height values (`h`). For this, we use a modified version of binary search.

- **Steps**:
  1. **Sorting by Width and Height**: 
      - We sort envelopes by their width in ascending order. If two envelopes have the same width, we sort them by height in descending order. This helps to avoid considering two envelopes with the same width but different heights.
  2. **Extracting Heights**: After sorting, we extract the heights (`h`) from the envelopes. Now the task reduces to finding the LIS on these heights.
  3. **Finding LIS Using Binary Search**:
      - We maintain a list `ans` where we keep updating the longest increasing subsequence.
      - For each new height, if it is larger than the last element of `ans`, we append it. Otherwise, we use binary search to find the appropriate position where this element should replace an existing one (to keep `ans` valid).

---

### Time Complexity:
- **Sorting**: Sorting the `envelopes` array takes \(O(n \log n)\), where `n` is the number of envelopes.
- **LIS Calculation (Binary Search)**: We iterate through the heights and use binary search (`lower_bound`), which takes \(O(\log n)\) for each element, leading to a total of \(O(n \log n)\) for this part.
  
Thus, the overall time complexity is **O(n log n)**.

---

### Space Complexity:
- **O(n)**: 
  - We use an auxiliary array `ans` to store the LIS, which can have at most `n` elements, and a vector `nums` of size `n` to store the heights. Therefore, the space complexity is linear with respect to the input size.

---

### Dry Run:
Let's go through a dry run of **Example 1** where `envelopes = [[5,4],[6,4],[6,7],[2,3]]`:

1. **Sorting Envelopes**: 
   After sorting, we get `envelopes = [[2,3], [5,4], [6,7], [6,4]]`.
   
2. **Extracting Heights**: 
   We extract the heights: `nums = [3, 4, 7, 4]`.

3. **LIS on Heights**:
   - Start with `ans = [3]` (from `nums[0]`).
   - `nums[1] = 4`: Since `4 > 3`, append to `ans`: `ans = [3, 4]`.
   - `nums[2] = 7`: Since `7 > 4`, append to `ans`: `ans = [3, 4, 7]`.
   - `nums[3] = 4`: Use binary search (`lower_bound`) to replace `4` at the correct position: `ans = [3, 4, 7]`.

The final answer is the size of `ans`, which is `3`.

Thus, the maximum number of envelopes you can Russian doll is `3`.
