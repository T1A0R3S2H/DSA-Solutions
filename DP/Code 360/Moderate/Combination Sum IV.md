```cpp
#include <bits/stdc++.h> 

int solveTab(vector<int>& num, int tar){
        vector<double> dp(tar+1, 0);
        // base case
        dp[0]=1;
        for(int i=1; i<=tar; i++){
            for(int j=0; j<num.size(); j++){
                if(i-num[j]>=0){
                    dp[i] = (dp[i] + dp[i - num[j]]);
                }
            }
        }
        return (int)dp[tar];
    }
int findWays(vector<int> &num, int tar)
{
    return solveTab(num, tar);
}
```
