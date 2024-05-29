## Method 1
- This approach follows a greedy strategy to find the minimum number of jumps required to reach the last index of the given array `nums`. It maintains two variables: `currentJumpEnd` to keep track of the maximum index that can be reached with the current jump, and `farthest` to store the farthest index that can be reached from the current position. The algorithm iterates through the array and updates `farthest` at each index by taking the maximum of its current value and `i + nums[i]`, which represents the maximum index that can be reached from the current position. Whenever the current index `i` is equal to `currentJumpEnd`, it means that we have reached the maximum index possible with the current jump, so we increment the number of jumps `jumps` and update `currentJumpEnd` to `farthest`. After the loop, the algorithm returns `jumps`, which represents the minimum number of jumps required to reach the last index.
##
```bash
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();
        int jumps = 0;
        int currentJumpEnd = 0, farthest = 0;
        
        for (int i = 0; i < n - 1; i++) {
            farthest = max(farthest, i + nums[i]);
            
            if (i == currentJumpEnd) {
                jumps++;
                currentJumpEnd = farthest;
            }
        }
        
        return jumps;
    }
};
```
##
- Time Complexity: O(n), where n is the length of the input array nums. The algorithm iterates through the array once.
- Space Complexity: O(1), as the algorithm uses a constant amount of extra space to store the variables jumps, currentJumpEnd, and farthest.
