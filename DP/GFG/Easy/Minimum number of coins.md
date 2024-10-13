### Code:

```cpp
class Solution {
public:
    vector<int> minPartition(int N) {
        // Indian currency denominations
        vector<int> coins = {2000, 500, 200, 100, 50, 20, 10, 5, 2, 1};
        vector<int> result;

        // Start with the largest denomination and keep subtracting it from N
        for (int i = 0; i < coins.size(); i++) {
            while (N >= coins[i]) {
                N -= coins[i];
                result.push_back(coins[i]);
            }
        }

        return result;
    }
};
```

### Explanation:
1. **Denominations:** The vector `coins` stores the available denominations of Indian currency in decreasing order.
2. **Greedy approach:** For each denomination, subtract it from `N` as many times as possible while it's less than or equal to `N`, and add it to the `result` list.
3. **Result:** Return the list of denominations used.

### Dry Run:
For the input `N = 43`:
- Start with the largest denomination, 2000: too large.
- Try 500: too large.
- Try 20: subtract twice (`N = 43 - 20 - 20 = 3`).
- Try 2: subtract once (`N = 3 - 2 = 1`).
- Try 1: subtract once (`N = 1 - 1 = 0`).

Result = `{20, 20, 2, 1}`.

For the input `N = 1000`:
- Start with 500: subtract twice (`N = 1000 - 500 - 500 = 0`).

Result = `{500, 500}`.

### Time Complexity:
- **Time complexity:** O(N), where N is the input value.
- **Auxiliary Space:** O(N), since the result array depends on N.
