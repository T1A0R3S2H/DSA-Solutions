# Method 1 (brute force, O(n^2), O(1))
- Finding all subarrays and checcking
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int n=nums.size();
        int ans=0;
        for (int i=0;i<n;i++){
            int sum=nums[i];
            if(sum==k){
                ans++;
            }
            for(int j=i+1;j<n;j++){
                sum+=nums[j];
                if(sum==k){
                    ans++;
                }
            }
        }
        return ans;
    }
};
```

# Method 2 (Optimisd)
