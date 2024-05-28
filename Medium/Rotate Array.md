## Method 1 (Striver)
- First, we calculate the effective rotation steps k by taking the modulus of k with the size of the array n. This ensures that k is within the range [0, n).
- We then reverse the entire array. This effectively moves the last k elements to the front.
- After reversing, the first k elements in the array are the elements that should be at the end of the rotated array. So, we reverse only the first k elements to bring them to their correct positions.
- Finally, we reverse the rest of the elements in the array after the first k elements. This brings the remaining elements back to their correct positions after rotation.
  ##
```bash

class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n; // rotation steps

        // Reverse the entire array
        reverse(nums.begin(), nums.end());
        
        // Reverse the first k elements
        reverse(nums.begin(), nums.begin() + k);
        
        // Reverse the rest of the elements after k
        reverse(nums.begin() + k, nums.end());
    }
};
```
##
This solution has a time complexity of O(n), where n is the size of the vector, since it performs three reverse operations on the entire vector or portions of it. It also has a space complexity of O(1), as it modifies the vector in-place without using any additional data structures that scale with the size of the input.
