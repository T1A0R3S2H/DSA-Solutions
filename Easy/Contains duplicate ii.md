
## Method 1 (brute force)

Given an integer array `nums` and an integer `k`, we need to determine if there exist two distinct indices `i` and `j` such that:

- `nums[i] == nums[j]`
- `abs(i - j) <= k`

## Approach:

1. **Initialization**:
   - Obtain the size of the array `nums`, denoted as `n`.

2. **Iterative Comparison**:
   - Iterate through each index `i` from `0` to `n-1`.
   - For each `i`, check all subsequent indices `j` within the range `[i + 1, min(i + k, n - 1)]`.
   - The `min(i + k, n - 1)` ensures that `j` stays within bounds and respects the distance constraint `abs(i - j) <= k`.

3. **Duplicate Check**:
   - Within the nested loop, compare `nums[i]` with `nums[j]`.
   - If a duplicate pair is found (`nums[i] == nums[j]`), return `true` immediately.

4. **Return**:
   - If no such duplicates are found after checking all possible pairs `(i, j)`, return `false`.

## Complexity Analysis:

- **Time Complexity**: \( O(n \times k) \), where `n` is the size of the array `nums` and `k` is the maximum distance allowed between indices.
  - The nested loops iterate over the array and within a bounded range based on `k`.
  
- **Space Complexity**: \( O(1) \).
  - The algorithm uses only a constant amount of extra space for variables (`n`, `i`, `j`, `k`, etc.), regardless of the input size.
 
## Code
```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int n = nums.size();
        for (int i=0;i<n;++i) {
            for (int j=i+1; j<=min(i+k,n-1); ++j) {
                if (nums[i]==nums[j]) {
                    return true;
                }
            }
        }
        return false;
    }
};

```

This approach efficiently checks for nearby duplicates within the array by iterating through each pair of indices and ensuring that the distance condition `abs(i - j) <= k` is respected.


## Method 2 (hash map)
```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int,int> mp;  // Define an unordered_map to store number -> index mappings
        int n = nums.size();  // Get the size of the input array nums
        
        // Iterate through each element of nums
        for(int i = 0; i < n; i++) {
            // Check if nums[i] already exists in the map
            if(mp.count(nums[i])) {
                // If nums[i] exists in the map, check the distance condition
                if(abs(i - mp[nums[i]]) <= k) {
                    return true;  // Return true if the condition abs(i - mp[nums[i]]) <= k is satisfied
                }
            }
            // Update the map with the current index of nums[i]
            mp[nums[i]] = i;
        }
        
        return false;  // Return false if no such pair is found after iterating through the array
    }
};

```
