# Minimum Size Subarray Sum Algorithm

## Problem Statement

Given an array of positive integers `nums` and a positive integer `target`, find the minimal length of a contiguous subarray whose sum is greater than or equal to `target`. If there is no such subarray, return 0 instead.

## Solution Explanation

This solution uses the sliding window technique to efficiently solve the problem. Here's how it works:

1. **Initialize variables:**
   - `mini`: Set to INT_MAX, will store the minimum length of valid subarray
   - `curr`: Current sum of the window, initially 0
   - `prev`: Start index of the current window, initially 0

2. **Iterate through the array:**
   - For each element `nums[i]`:
     - Add `nums[i]` to `curr`
     - While `curr` is greater than or equal to `target`:
       - Update `mini` if current window length is smaller
       - Subtract `nums[prev]` from `curr`
       - Increment `prev` to shrink the window

3. **Return result:**
   - If `mini` is still INT_MAX, return 0 (no valid subarray found)
   - Otherwise, return `mini`

## Example

Let's walk through the algorithm with an example:

**Input:** target = 7, nums = [2,3,1,2,4,3]

1. Start with `mini = INT_MAX`, `curr = 0`, `prev = 0`
2. Iterate through array:
   - i = 0: curr = 2, not enough
   - i = 1: curr = 5, not enough
   - i = 2: curr = 6, not enough
   - i = 3: curr = 8, update mini to 4, subtract nums[0], prev = 1
   - i = 4: curr = 10, update mini to 3, subtract nums[1] and nums[2], prev = 3
   - i = 5: curr = 9, update mini to 2, subtract nums[3] and nums[4]

3. Return mini = 2

The shortest subarray with sum >= 7 is [4,3], with length 2.

## Time Complexity

O(n), where n is the length of the input array. Each element is added once and removed at most once.

## Space Complexity

O(1), as it uses only a constant amount of extra space.

## Code

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int mini=INT_MAX;
        int curr=0;
        int prev=0;
        for(int i=0; i<nums.size(); i++){
            curr+=nums[i];
            while(curr>=target){
                mini=min(mini,i-prev+1);
                curr-=nums[prev];
                prev++;
            }
        }
        return mini==INT_MAX?0:mini;
    }
};
```
