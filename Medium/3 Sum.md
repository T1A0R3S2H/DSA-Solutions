## Method 1 
- It involves the use of set. first we sort the given vector then start a loop, inside the loop, we take two pointers, one from thre starting and other from the end.
- Then, we move inside a while loop to calculate the sum, we see that the calculated sum can be < or  == or > than the target, if equal, then insert the three numbers into the set s. If sum<target, mive aage from the starting pointer. And else move from the end towards piche.
- ![image](https://github.com/T1A0R3S2H/Leetcode-Progess/assets/123285559/1399b510-356a-404e-b288-373b2c7db815)

```bash
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int target = 0;
        sort(nums.begin(), nums.end());
        set<vector<int>> s;
        vector<vector<int>> output;
        for (int i = 0; i < nums.size(); i++){
            int j = i + 1;
            int k = nums.size() - 1;
            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                if (sum == target) {
                    s.insert({nums[i], nums[j], nums[k]});
                    j++;
                    k--;
                } else if (sum < target) {
                    j++;
                } else {
                    k--;
                }
            }
        }
        for(auto triplets : s)
            output.push_back(triplets);
        return output;
    }
};
```
