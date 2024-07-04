# Method 1 (brute force)

This solution finds the maximum possible sum of heights in a mountain-shaped tower arrangement, where you can remove bricks but not add them.

## Code
```cpp
class Solution {
public:
    long long maximumSumOfHeights(vector<int>& heights) {
        int n=heights.size();
        long long maxSum=0;

        for (int peak=0;peak<n;peak++) {
            long long sum=heights[peak];
            
            // Left side of the peak
            int prev=heights[peak];
            for (int i=peak-1;i>=0;i--) {
                prev=min(prev, heights[i]);
                sum+=prev;
            }
            
            // Right side of the peak
            prev=heights[peak];
            for (int i=peak+1;i<n;i++) {
                prev=min(prev, heights[i]);
                sum+=prev;
            }
            
            maxSum=max(maxSum, sum);
        }

        return maxSum;
    }
};
```

## Algorithm Overview

1. Iterate through each position in the array, considering it as a potential peak.
2. For each potential peak:
   - Calculate the sum of heights to its left (non-decreasing)
   - Calculate the sum of heights to its right (non-increasing)
   - Sum these with the peak height
3. Keep track of the maximum sum encountered

## Detailed Breakdown

### Initialization
- `n`: Store the size of the input array
- `maxSum`: Initialize to 0, will store the maximum sum found

### Main Loop
Iterate through each index `peak` in the array:

1. Initialize `sum` with the height at the current peak

2. Left side of the peak:
   - Start from `peak-1` and move left
   - `prev`: Tracks the minimum height seen so far
   - For each position:
     - Update `prev` to be the minimum of itself and the current height
     - Add `prev` to `sum`

3. Right side of the peak:
   - Reset `prev` to the peak's height
   - Start from `peak+1` and move right
   - For each position:
     - Update `prev` to be the minimum of itself and the current height
     - Add `prev` to `sum`

4. Update `maxSum`:
   - Set `maxSum` to the maximum of itself and the current `sum`

### Return Result
Return `maxSum` as the final answer

## Time and Space Complexity

- Time Complexity: O(n^2), where n is the number of elements in the array
  - We consider each element as a potential peak
  - For each peak, we traverse the array to its left and right
- Space Complexity: O(1)
  - We use only a constant amount of extra space

## Key Insights

- The algorithm ensures a mountain shape by using `min` to maintain non-decreasing heights to the left of the peak and non-increasing heights to the right
- By considering each position as a potential peak, we explore all possible mountain shapes
- The use of `prev` allows us to efficiently track the maximum allowable height at each position

This approach guarantees finding the maximum sum while respecting the mountain shape constraint and the rule of only removing (never adding) bricks.

# Method 2 (stacks)
```cpp
#include <vector>
#include <stack>
#include <algorithm>

class Solution {
public:
    long long maximumSumOfHeights(std::vector<int>& heights) {
        int n = heights.size();
        long long maxSum = 0;

        // Iterate over each possible peak
        for (int peak = 0; peak < n; ++peak) {
            long long sum = heights[peak];
            std::stack<int> leftStack, rightStack;
            int leftMin = heights[peak], rightMin = heights[peak];

            // Process the left side of the peak
            for (int i = peak - 1; i >= 0; --i) {
                leftMin = std::min(leftMin, heights[i]);
                sum += leftMin;
                leftStack.push(leftMin);
            }

            // Process the right side of the peak
            for (int i = peak + 1; i < n; ++i) {
                rightMin = std::min(rightMin, heights[i]);
                sum += rightMin;
                rightStack.push(rightMin);
            }

            maxSum = std::max(maxSum, sum);
        }

        return maxSum;
    }
};

```
