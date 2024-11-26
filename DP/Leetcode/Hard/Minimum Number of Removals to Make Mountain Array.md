### Explanation:

1. **Calculate LIS from the left**: We determine the longest increasing subsequence up to each index.
2. **Calculate LIS from the right**: This is the longest decreasing subsequence starting from each index.
3. **Find the maximum mountain length**: For an index to qualify as a mountain peak, `inc[i] > 1` and `dec[i] > 1` (because a peak needs at least one element increasing before and one decreasing after).
4. **Calculate the result**: Subtract the maximum mountain length from the total number of elements to find the minimum removals.

Here's the solution:

### Code:

```cpp
class Solution {
public:
    int LongestBitonicSequence(int n, vector<int> &nums) {
        vector<int> inc(n, 1), dec(n, 1);
    
        // Calculate LIS from left to right
        for (int i=0; i<n; i++) {
            for (int j=0; j<i; j++) {
                if (nums[i]>nums[j]) {
                    inc[i]=max(inc[i], inc[j]+1);
                }
            }
        }
    
        // Calculate LIS from right to left for decreasing sequence
        for (int i=n-1; i>=0; i--) {
            for (int j=n-1; j>i; j--) {
                if (nums[i]>nums[j]) {
                    dec[i]=max(dec[i], dec[j]+1);
                }
            }
        }
    
        // Calculate the maximum length of the bitonic sequence
        int maxLength=0;
        for (int i=0; i<n; i++) {
            if(inc[i]>1 and dec[i]>1)
            maxLength=max(maxLength, inc[i]+dec[i]-1);
        }
        return maxLength;
    }

    int minimumMountainRemovals(vector<int>& nums) {
        return nums.size()-LongestBitonicSequence(nums.size(), nums);
    }
};
```

### ETSD:

**Explanation**:
1. The code calculates the length of the longest bitonic subsequence that forms a mountain.
2. Peaks in the mountain must have an increasing sequence before and a decreasing sequence after.
3. The minimum removals are derived by subtracting the maximum mountain length from the array size.

**Time Complexity**:
- \(O(n^2)\) for both LIS and LDS calculations.
- Total: \(O(n^2)\).

**Space Complexity**:
- \(O(n)\) for the `inc` and `dec` arrays.

**Dry Run**:
Input: `nums = [2,1,1,5,6,2,3,1]`

1. Calculate `inc`: `[1, 1, 1, 2, 3, 3, 4, 1]`
2. Calculate `dec`: `[1, 1, 1, 3, 2, 2, 2, 1]`
3. Peaks: Index 3 (`inc[3]=2`, `dec[3]=3`), Index 4 (`inc[4]=3`, `dec[4]=2`).
4. Max mountain length: 5.
5. Minimum removals: \(8 - 5 = 3\).

Output: `3`.
