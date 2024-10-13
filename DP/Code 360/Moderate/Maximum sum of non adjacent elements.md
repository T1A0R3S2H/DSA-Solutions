```cpp
#include <bits/stdc++.h> 


int tabu(vector<int>& nums){
        int n=nums.size();
        vector<int> dp(n, 0);  //represents the maximum amount
        dp[0]=nums[0];
        if (n>1) dp[1]=max(nums[0], nums[1]);  //to avoid heap buffer overflow
        for(int i=2; i<n; i++){
            int inc=dp[i-2]+nums[i];
            int ex=dp[i-1]+0;
            dp[i]=max(inc,  ex);
        }
        return dp[n-1];
    }


int maximumNonAdjacentSum(vector<int> &nums){
    return tabu(nums);
}
```
