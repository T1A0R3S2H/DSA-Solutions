### Code:
```cpp
class Solution {
public:
    int solve(vector<int>& coins, int amount){
        vector<int> dp(amount+1, INT_MAX);
        dp[0] = 0;

        // check for every amount from 1 to amount value
        for (int i=1; i<=amount; i++) {
            for (int j=0; j<coins.size(); j++) {
                if (i-coins[j]>=0 && dp[i-coins[j]]!=INT_MAX) {
                    dp[i]=min(dp[i], 1+dp[i-coins[j]]);
                }
            }
        }
        if (dp[amount]==INT_MAX) return -1;
        return dp[amount];
    }

    int coinChange(vector<int>& coins, int amount) {
        return solve(coins, amount);
    }
};
```

## GIST
Here's a detailed summary that combines the explanations of both the **outer loop** and the **inner loop** of your dynamic programming solution for the "Coin Change" problem, along with how they work together to solve it.

### Outer Loop: Iterating Over the Amount (`for (int i = 1; i <= amount; i++)`)

- The outer loop iterates over every possible amount from `1` to `amount`. The goal is to **build up the solution** by progressively solving smaller subproblems.
- The variable `i` represents the **current amount** we are trying to compute the minimum number of coins for.
- For example:
    - When `i = 1`, we are trying to find the minimum number of coins needed to make an amount of `1`.
    - When `i = 2`, we are trying to find the minimum number of coins to make `2`, and so on, until `i = amount`.
    
- This approach ensures that for each amount `i`, we already have the solutions for smaller amounts (`dp[0]`, `dp[1]`, ..., `dp[i-1]`). This is a **bottom-up dynamic programming** technique, where we build solutions for larger amounts using the solutions for smaller amounts.

### Inner Loop: Checking Each Coin (`for (int j = 0; j < coins.size(); j++)`)

- The inner loop iterates over each coin denomination in the `coins` array to check if we can use that coin to form the current amount `i`.
- The variable `j` represents the index of the current coin being considered. The value `coins[j]` is the denomination of the current coin.
- For each coin `coins[j]`, we check two conditions:
    1. **`i - coins[j] >= 0`**: This checks whether we can use the current coin `coins[j]` to form the amount `i`. If `i - coins[j] < 0`, it means the coin is too large to be used for this amount.
    2. **`dp[i - coins[j]] != INT_MAX`**: This checks whether the remaining amount `i - coins[j]` can be formed using the available coins. If `dp[i - coins[j]] == INT_MAX`, it means we can't form the remaining amount, and therefore we can't use this coin to form the amount `i`.

### Combining the Two Loops: Building the Solution

The two loops work together to build the solution progressively for each amount.

1. **Initialization**:
    - We start with a `dp[]` array, where `dp[i]` represents the minimum number of coins needed to form the amount `i`.
    - Initially, `dp[0] = 0` because zero coins are required to make the amount `0`. All other values are set to `INT_MAX` to represent that the corresponding amounts cannot yet be formed.

2. **For each amount `i` (outer loop)**:
    - We check every coin `coins[j]` (inner loop) to see if it can contribute to forming the amount `i`.
    - For each valid coin (where `i - coins[j] >= 0` and `dp[i - coins[j]] != INT_MAX`):
        - We update `dp[i]` to be the minimum value between the current `dp[i]` and `1 + dp[i - coins[j]]`.
        - **`1 + dp[i - coins[j]]`** means:
            - The `1` represents using the current coin `coins[j]`.
            - `dp[i - coins[j]]` is the minimum number of coins needed to form the remaining amount `i - coins[j]`.
        - This ensures that `dp[i]` always holds the minimum number of coins needed to form the amount `i`.

### Example Dry Run: `coins = [1, 2, 5]`, `amount = 5`

Let's see how the two loops work together step by step for `coins = [1, 2, 5]` and `amount = 5`:

1. **Initialization**:
    - `dp[] = [0, INT_MAX, INT_MAX, INT_MAX, INT_MAX, INT_MAX]`

2. **i = 1**:
    - Check coin `1`:
        - `1 - 1 = 0` and `dp[0] = 0`, so we can use the coin `1`.
        - Update `dp[1] = min(dp[1], 1 + dp[0]) = min(INT_MAX, 1 + 0) = 1`.
    - Result: `dp[] = [0, 1, INT_MAX, INT_MAX, INT_MAX, INT_MAX]`.

3. **i = 2**:
    - Check coin `1`:
        - `2 - 1 = 1` and `dp[1] = 1`, so we can use the coin `1`.
        - Update `dp[2] = min(dp[2], 1 + dp[1]) = min(INT_MAX, 1 + 1) = 2`.
    - Check coin `2`:
        - `2 - 2 = 0` and `dp[0] = 0`, so we can use the coin `2`.
        - Update `dp[2] = min(dp[2], 1 + dp[0]) = min(2, 1 + 0) = 1`.
    - Result: `dp[] = [0, 1, 1, INT_MAX, INT_MAX, INT_MAX]`.

4. **i = 3**:
    - Check coin `1`:
        - `3 - 1 = 2` and `dp[2] = 1`, so we can use the coin `1`.
        - Update `dp[3] = min(dp[3], 1 + dp[2]) = min(INT_MAX, 1 + 1) = 2`.
    - Check coin `2`:
        - `3 - 2 = 1` and `dp[1] = 1`, so we can use the coin `2`.
        - Update `dp[3] = min(dp[3], 1 + dp[1]) = min(2, 1 + 1) = 2`.
    - Result: `dp[] = [0, 1, 1, 2, INT_MAX, INT_MAX]`.

5. **i = 5**:
    - Check coin `1`:
        - `5 - 1 = 4` and `dp[4] = 2`, so we can use the coin `1`.
        - Update `dp[5] = min(dp[5], 1 + dp[4]) = min(INT_MAX, 1 + 2) = 3`.
    - Check coin `2`:
        - `5 - 2 = 3` and `dp[3] = 2`, so we can use the coin `2`.
        - Update `dp[5] = min(dp[5], 1 + dp[3]) = min(3, 1 + 2) = 3`.
    - Check coin `5`:
        - `5 - 5 = 0` and `dp[0] = 0`, so we can use the coin `5`.
        - Update `dp[5] = min(dp[5], 1 + dp[0]) = min(3, 1 + 0) = 1`.
    - Result: `dp[] = [0, 1, 1, 2, 2, 1]`.

### Conclusion

The two loops work together to progressively build up the solution for the amount by considering every coin for every possible amount. The `dp[]` array stores the minimum number of coins required for each amount, and the inner loop checks every coin to find the optimal way to form the current amount. The final result is stored in `dp[amount]`.

# Detailed 👇

### 1. **Explanation**

The problem asks to find the minimum number of coins needed to make up a given `amount` using an array `coins` of different denominations. If it's impossible to form the `amount`, the function should return `-1`.

#### Approach:
- The algorithm uses **dynamic programming** to solve this problem. 
- We define a `dp[]` array where `dp[i]` represents the minimum number of coins required to make up an amount `i`. 
- We initialize `dp[0] = 0`, as zero coins are needed to make amount `0`.
- All other entries in the `dp[]` array are initialized to `INT_MAX`, which indicates that the amount is not yet reachable.
  
For each amount from `1` to `amount`:
- We check each coin in `coins[]`.
- If the current coin can be used (i.e., `i - coins[j] >= 0`), we update `dp[i]` to the minimum value between the current `dp[i]` and `1 + dp[i - coins[j]]`. 
    - This represents taking the current coin and adding it to the minimum coins needed to make the remaining amount (`i - coins[j]`).

Finally:
- If `dp[amount]` is still `INT_MAX`, it means it's impossible to form the `amount` using the given coins, and we return `-1`. 
- Otherwise, we return `dp[amount]`, which gives the minimum number of coins required.

### 2. **Time Complexity**

The time complexity of this solution is:
- **O(amount * n)**, where `n` is the number of coins in the `coins` array. Here's why:
    - We iterate over each amount from `1` to `amount` (total of `amount` iterations).
    - For each amount, we iterate over all the coins (total of `n` iterations).
    - Hence, the total number of operations is `amount * n`.

### 3. **Space Complexity**

The space complexity of the solution is:
- **O(amount)** because we use a 1D `dp[]` array of size `amount + 1` to store the minimum number of coins required for each amount.

### 4. **Dry Run**

Let's perform a dry run for the input: `coins = [1, 2, 5]` and `amount = 11`.

#### Initialization:
- `dp[] = [0, INT_MAX, INT_MAX, INT_MAX, ..., INT_MAX]` (size 12)

#### Iterating over amounts:

- **i = 1**:
    - For coin `1`: `dp[1] = min(dp[1], 1 + dp[1 - 1]) = min(INT_MAX, 1 + 0) = 1`
    - Updated `dp[] = [0, 1, INT_MAX, INT_MAX, ..., INT_MAX]`

- **i = 2**:
    - For coin `1`: `dp[2] = min(dp[2], 1 + dp[2 - 1]) = min(INT_MAX, 1 + 1) = 2`
    - For coin `2`: `dp[2] = min(dp[2], 1 + dp[2 - 2]) = min(2, 1 + 0) = 1`
    - Updated `dp[] = [0, 1, 1, INT_MAX, ..., INT_MAX]`

- **i = 3**:
    - For coin `1`: `dp[3] = min(dp[3], 1 + dp[3 - 1]) = min(INT_MAX, 1 + 1) = 2`
    - For coin `2`: `dp[3] = min(dp[3], 1 + dp[3 - 2]) = min(2, 1 + 1) = 2`
    - Updated `dp[] = [0, 1, 1, 2, INT_MAX, ..., INT_MAX]`

- **i = 4**:
    - For coin `1`: `dp[4] = min(dp[4], 1 + dp[4 - 1]) = min(INT_MAX, 1 + 2) = 3`
    - For coin `2`: `dp[4] = min(dp[4], 1 + dp[4 - 2]) = min(3, 1 + 1) = 2`
    - Updated `dp[] = [0, 1, 1, 2, 2, INT_MAX, ..., INT_MAX]`

- **i = 5**:
    - For coin `1`: `dp[5] = min(dp[5], 1 + dp[5 - 1]) = min(INT_MAX, 1 + 2) = 3`
    - For coin `2`: `dp[5] = min(dp[5], 1 + dp[5 - 2]) = min(3, 1 + 2) = 3`
    - For coin `5`: `dp[5] = min(dp[5], 1 + dp[5 - 5]) = min(3, 1 + 0) = 1`
    - Updated `dp[] = [0, 1, 1, 2, 2, 1, INT_MAX, ..., INT_MAX]`

- **i = 11** (final result):
    - The algorithm will similarly update for all amounts until it reaches `11`, where `dp[11] = 3`.
    - This corresponds to `5 + 5 + 1 = 11` using 3 coins.

Thus, the output is `3`.

### Final Notes:
This algorithm provides an efficient solution to the problem using dynamic programming. The `dp[]` array ensures that we avoid redundant computations, and by iterating over all possible coins for each amount, we find the optimal solution.
