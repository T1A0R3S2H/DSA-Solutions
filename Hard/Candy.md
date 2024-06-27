# Method 1 (brute forcce)
![Screenshot 2024-06-27 130132.png](https://assets.leetcode.com/users/images/c72b9d67-be8c-4926-9ce5-b00a5a76eb35_1719473634.192599.png)


## Code
```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        if (n == 0) return 0; // Handle edge case of empty ratings
        
        vector<int> allocate(n, 1); // Start with 1 candy for each child
        
        // First pass: from left to right
        for (int i = 1; i < n; ++i) {
            if (ratings[i] > ratings[i - 1]) {
                allocate[i] = allocate[i - 1] + 1;
            }
        }
        
        // Second pass: from right to left
        for (int i = n - 2; i >= 0; --i) {
            if (ratings[i] > ratings[i + 1]) {
                allocate[i] = max(allocate[i], allocate[i + 1] + 1);
            }
        }
        
        // Calculate total candies
        int totalCandies = 0;
        for (int candy : allocate) {
            totalCandies += candy;
        }
        
        return totalCandies;
    }
};

```
## Detailed Explanation
In the given approach, both passes (left to right and right to left) contribute to ensuring that both conditions of the candy distribution problem are met:

1. **Each child must have at least one candy**: This condition is inherently met by initializing each child with 1 candy (`vector<int> allocate(n, 1)`).

2. **Children with a higher rating get more candies than their neighbors**:

### First Pass (left to right):
- This pass ensures that if a child has a higher rating than the child immediately to their left (`ratings[i] > ratings[i - 1]`), they should receive more candies than that child (`allocate[i] = allocate[i - 1] + 1`).

#### Example:
- Consider `ratings = [1, 0, 2]`.
- After the first pass, `allocate` might become `[1, 1, 2]`.
- Here, child 3 (rating 2) receives more candies than child 2 (rating 0), satisfying the condition.

### Second Pass (right to left):
- This pass ensures that if a child has a higher rating than the child immediately to their right (`ratings[i] > ratings[i + 1]`), they should receive more candies than that child (`allocate[i] = max(allocate[i], allocate[i + 1] + 1)`).

#### Example (continuing from above):
- After the second pass, `allocate` might be adjusted to `[2, 1, 2]`.
- Here, child 1 (rating 1) receives more candies than child 2 (rating 0), ensuring the condition is maintained across both directions.

### Combined Effect:
- By performing both passes (left to right and right to left), the approach ensures that each child receives the minimum required candies (starting with 1) and that children with higher ratings receive more candies than their neighbors as required by the problem statement.

### Efficiency:
- Each pass runs in O(n) time complexity, where n is the number of children (length of the `ratings` array).
- Therefore, the overall time complexity of the approach remains O(n), which is efficient and suitable given the problem constraints (`1 <= n <= 2 * 10^4`).

