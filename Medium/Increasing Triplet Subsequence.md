## Approach:
We can use two variables, first_small and second_small, to keep track of the two smallest elements encountered so far. We initialize them with the maximum possible integer value initially. Then, we iterate through the array nums.

If the current element nums[i] is less than or equal to first_small, we update first_small with nums[i].
If nums[i] is greater than first_small but less than or equal to second_small, we update second_small with nums[i].
If nums[i] is greater than both first_small and second_small, we have found an increasing triplet, and we return true.
If we finish iterating through the array without finding an increasing triplet, we return false.
Complexity
## Time complexity: 
The time complexity of this approach is O(n), where n is the size of the input array nums. We traverse the array once.
## Space complexity: 
The space complexity of this approach is O(1), as we only use a constant amount of extra space regardless of the size of the input array.

## Code
```cpp
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) 
    {
        int first_small = INT_MAX;
        int second_small = INT_MAX;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]<=first_small)
                first_small=nums[i];
            else if( nums[i]<= second_small )
                second_small= nums[i];
            else
                return true;
        }
        return false;
    }
};
```
