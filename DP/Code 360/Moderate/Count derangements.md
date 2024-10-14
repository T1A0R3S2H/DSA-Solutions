```cpp
#include <bits/stdc++.h>
long long int countDerangements(int n) {
    const int MOD=1e9 + 7;
    //base cases
        if (n==1) return 0;
        if (n==2) return 1;
        vector<long int> dp(n + 1);
        
        //base values
        dp[0]=1;
        dp[1]=0;
        dp[2]=1;

        for (int i=3; i<=n; i++) {
            dp[i]=((i-1)*(dp[i-1]+dp[i-2])%MOD)%MOD;
        }
        return dp[n];
}
```
