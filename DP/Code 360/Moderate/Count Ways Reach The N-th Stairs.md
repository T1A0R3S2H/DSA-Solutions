### Code:
```cpp
#include <vector>
using namespace std;
int countDistinctWays(int nStairs) {
  // Create a dp array with size nStairs + 1, initialized to -1
  int n = nStairs;
  if (n == 0 || n == 1) {
    return 1;
  }
  int m = 1e9 + 7;
  int prev2 = 1;
  int prev = 1;
  for (int i = 2; i <= n; i++) {
    int cur_i = (prev2 + prev) % m;
    prev2 = prev;
    prev = cur_i;
  }
  return prev;
}
```
