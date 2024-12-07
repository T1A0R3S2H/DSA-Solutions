## C++ Code:
```cpp
#include <bits/stdc++.h>

long long mod = 1e9 + 7;
long long mul(long long a, long long b) {
  return ((a % mod) * (b % mod)) % mod;
}

long long add(long long a, long long b) {
  return ((a % mod) + (b % mod)) % mod;
}

long long solve(int n, int k) {
  if (n == 1)
    return k;
  if (n == 2)
    return mul(k, k);
  long long first = mul(solve(n - 2, k), k - 1);
  long long second = mul(solve(n - 1, k), k - 1);
  return add(first, second);
}

long long solveMem(int n, int k, vector<long long> &dp) {
  if (n == 1)
    return (long long)k;
  if (n == 2)
    return (long long)mul(k, k);
  if (dp[n] != -1)
    return dp[n] % mod;
  long long first = mul(solveMem(n - 2, k, dp), k - 1);
  long long second = mul(solveMem(n - 1, k, dp), k - 1);
  return dp[n] = add(first, second);
}

long long solveTab(int n, int k) {
  vector<long long> dp(n + 1, 0);
  dp[1] = k;
  dp[2] = mul(k, k);
  for (int i = 3; i <= n; i++) {
    dp[i] = add(mul((k - 1), (dp[i - 2])), mul((k - 1), (dp[i - 1])));
  }
  return dp[n] % mod;
}

long long solveSO(int n, int k) {
  long long prev2 = k;         // prev2=>dp[1]
  long long prev1 = mul(k, k); // prev1=>dp[2]
  if (n == 1)
    return prev2;
  if (n == 2)
    return prev1;
  for (int i = 3; i <= n; i++) {
    long long ans = add(mul(prev1, k - 1), mul(prev2, k - 1));
    prev2 = prev1 % mod;
    prev1 = ans % mod;
  }
  return prev1 % mod;
}

int numberOfWays(int n, int k) { 
    return solveSO(n, k); 
}
```
