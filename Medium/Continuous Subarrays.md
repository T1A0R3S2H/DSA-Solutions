### Code:

```cpp
class Solution {
public:
    #define ll long long
    long long continuousSubarrays(vector<int>& nums) {
        ll cnt = 0;                // To store the count of valid subarrays
        int l = 0, r = 0;          // Left pointer l and Right pointer r
        priority_queue<pair<int, int>> pq1; // Max heap to track the max value in the current window
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq2; // Min heap to track the min value in the current window
        
        while (r < nums.size()) {   // Iterate over the entire array with r as the right end of the subarray
            pq1.push({nums[r], r});  // Add the current number with its index in max-heap
            pq2.push({nums[r], r});  // Add the current number with its index in min-heap

            // If the condition fails (i.e., the difference between max and min exceeds 2)
            while (!pq1.empty() && !pq2.empty() && (pq1.top().first - pq2.top().first) > 2) {
                l++;  // Move the left pointer to the right
                // Remove the elements that are outside the new window (i.e., index < l)
                while (!pq1.empty() && pq1.top().second < l) pq1.pop();
                while (!pq2.empty() && pq2.top().second < l) pq2.pop();
            }

            // If the condition is valid, all subarrays from l to r are valid
            cnt += (r - l + 1);  // Count the valid subarrays ending at r
            r++;  // Move the right pointer to the next index
        }
        return cnt;  // Return the total count of valid subarrays
    }
};
```

### Explanation:

1. **Variables:**
   - `cnt`: To store the total number of valid subarrays.
   - `l`: Left pointer, which tracks the start of the current valid window.
   - `r`: Right pointer, which moves through the array and checks all possible subarrays ending at `r`.
   - `pq1`: A max heap to store the values in the current window, which helps in tracking the maximum value.
   - `pq2`: A min heap to store the values in the current window, which helps in tracking the minimum value.

2. **Logic:**
   - We iterate over the array with `r` as the right boundary of the subarray.
   - For each `r`, add `nums[r]` to both the max and min heaps (`pq1` and `pq2`).
   - Then we check if the condition `max(nums[l...r]) - min(nums[l...r]) <= 2` is satisfied.
     - If the difference exceeds 2, we increment `l` (shrink the window) to make the condition valid again.
   - Once the condition is valid, all subarrays from `l` to `r` are valid, and we add `(r - l + 1)` to `cnt`.
   - Finally, return the total count of valid subarrays.

### Time Complexity:

- **Time Complexity**: `O(n log n)`
  - We iterate through the array once, and for each element, we perform insertions and deletions in the heaps.
  - Inserting and deleting from a heap takes `O(log n)`, and we do this for each element in the array, so the total time complexity is `O(n log n)`.

- **Space Complexity**: `O(n)`
  - We use two heaps to store the elements, and in the worst case, the size of the heaps can be `O(n)`. Hence, the space complexity is `O(n)`.

### Dry Run:

Let's dry-run this for the input `nums = [5, 4, 2, 4]`.

#### Initial Setup:
- `l = 0`, `r = 0`, `cnt = 0`
- `pq1 = []`, `pq2 = []`

---

#### Iteration 1 (`r = 0`):
- We add `nums[r] = 5` to both heaps:
  - `pq1 = [(5, 0)]`
  - `pq2 = [(5, 0)]`
- Check if `max(nums[l...r]) - min(nums[l...r]) <= 2`:
  - `max - min = 5 - 5 = 0`, which is valid.
- All subarrays from `l = 0` to `r = 0` are valid, so we add `(r - l + 1) = 1` to `cnt`.
- `cnt = 1`
- Move `r` to 1: `r = 1`

---

#### Iteration 2 (`r = 1`):
- We add `nums[r] = 4` to both heaps:
  - `pq1 = [(5, 0), (4, 1)]`
  - `pq2 = [(4, 1), (5, 0)]`
- Check if `max(nums[l...r]) - min(nums[l...r]) <= 2`:
  - `max - min = 5 - 4 = 1`, which is valid.
- All subarrays from `l = 0` to `r = 1` are valid:
  - Subarrays: `[4]`, `[5, 4]`
  - We add `(r - l + 1) = 2` to `cnt`.
- `cnt = 3`
- Move `r` to 2: `r = 2`

---

#### Iteration 3 (`r = 2`):
- We add `nums[r] = 2` to both heaps:
  - `pq1 = [(5, 0), (4, 1), (2, 2)]`
  - `pq2 = [(2, 2), (4, 1), (5, 0)]`
- Check if `max(nums[l...r]) - min(nums[l...r]) <= 2`:
  - `max - min = 5 - 2 = 3`, which **exceeds 2**.
- So, we increment `l` to `1` and remove the elements where `index < 1` from both heaps:
  - `pq1 = [(4, 1), (2, 2)]`
  - `pq2 = [(2, 2), (4, 1)]`
- Check again:
  - `max - min = 4 - 2 = 2`, which is valid.
- All subarrays from `l = 1` to `r = 2` are valid:
  - Subarrays: `[4]`, `[4, 2]`, `[5, 4, 2]`
  - We add `(r - l + 1) = 2` to `cnt`.
- `cnt = 5`
- Move `r` to 3: `r = 3`

---

#### Iteration 4 (`r = 3`):
- We add `nums[r] = 4` to both heaps:
  - `pq1 = [(4, 1), (4, 3), (2, 2)]`
  - `pq2 = [(2, 2), (4, 1), (4, 3)]`
- Check if `max(nums[l...r]) - min(nums[l...r]) <= 2`:
  - `max - min = 4 - 2 = 2`, which is valid.
- All subarrays from `l = 1` to `r = 3` are valid:
  - Subarrays: `[4]`, `[4, 2]`, `[2, 4]`, `[4, 2, 4]`
  - We add `(r - l + 1) = 3` to `cnt`.
- `cnt = 8`
- End of array reached, so return `cnt = 8`.

### Final Output:
The final answer for the array `[5, 4, 2, 4]` is `8` valid subarrays.

### Conclusion:
This approach efficiently counts the valid subarrays using heaps to track the min and max values in a sliding window, ensuring we get the correct result with optimal time and space complexity.
