![image](https://github.com/user-attachments/assets/0621e9a8-fd9f-41e1-ad1b-6e09f9eae692)


# Method 1 (Recursive)
# Method 2 (Top-Down | Memoised)
#### Code

```cpp
class Solution {
public:
    int recursiveHelper(vector<int>& arr, int left, int right, map<pair<int, int>, int>& maxLeaf, map<pair<int, int>, int>& memo) {
        // Base case: if only one element, no non-leaf node can be formed
        if (left == right) return 0;

        // Check if we have already calculated for this range
        if (memo.find({left, right}) != memo.end()) return memo[{left, right}];

        int minCost = INT_MAX;
        for (int i = left; i < right; ++i) {
            // Split at every possible point and calculate the cost
            int leftMax = maxLeaf[{left, i}];
            int rightMax = maxLeaf[{i + 1, right}];
            int cost = leftMax * rightMax + recursiveHelper(arr, left, i, maxLeaf, memo) + recursiveHelper(arr, i + 1, right, maxLeaf, memo);
            minCost = min(minCost, cost);
        }
        
        // Store result in memo
        return memo[{left, right}] = minCost;
    }

    int mctFromLeafValues(vector<int>& arr) {
        int n = arr.size();
        map<pair<int, int>, int> maxLeaf;
        
        // Precompute maximum values for all subarrays
        for (int i = 0; i < n; ++i) {
            maxLeaf[{i, i}] = arr[i];
            for (int j = i + 1; j < n; ++j) {
                maxLeaf[{i, j}] = max(arr[j], maxLeaf[{i, j - 1}]);
            }
        }
        
        map<pair<int, int>, int> memo;
        return recursiveHelper(arr, 0, n - 1, maxLeaf, memo);
    }
};
```

#### Explanation

1. **Recursive Splitting**:
   - We define a recursive function `recursiveHelper` that splits the array `arr` at every possible point and calculates the non-leaf node cost at each split point.
   
2. **Subarray Max Values Precomputation**:
   - Using `maxLeaf[{i, j}]`, we store the maximum leaf value in every subarray `arr[i:j+1]`. This avoids recalculating maximum values in recursive calls.

3. **Memoization of Results**:
   - We use a `memo` map to store results for subarrays already computed, allowing us to avoid redundant calculations and improve efficiency.

#### Time Complexity

- **O(n^3)**, as the recursive function splits the array in `O(n^2)` ways, and each split takes `O(n)` time to combine subproblems.

#### Space Complexity

- **O(n^2)** for memoization and precomputed `maxLeaf` values.

#### Dry Run for `arr = [6, 2, 4]`

1. Precompute `maxLeaf`:
   - `maxLeaf[{0, 0}] = 6`
   - `maxLeaf[{1, 1}] = 2`
   - `maxLeaf[{2, 2}] = 4`
   - `maxLeaf[{0, 1}] = 6`
   - `maxLeaf[{1, 2}] = 4`
   - `maxLeaf[{0, 2}] = 6`

2. `recursiveHelper(0, 2)`:
   - Split at `i=0`: `cost = 6 * 4 + recursiveHelper(0, 0) + recursiveHelper(1, 2) = 24 + 8 = 32`.
   - `minCost = 32`.

3. Final result: `32`.

---

### 2. Memoization (Same as Recursive Solution)

The memoization approach here is implemented within the recursive solution. By storing the result of each subarray in `memo`, we avoid recalculations and achieve a more efficient solution.

---
# Method 3 (Bottom-Up | Tabulated)


#### Code

```cpp
class Solution {
public:
    int mctFromLeafValues(vector<int>& arr) {
        int n = arr.size();
        vector<vector<int>> dp(n, vector<int>(n, INT_MAX));
        map<pair<int, int>, int> maxLeaf;

        // Precompute maximum values for all subarrays
        for (int i = 0; i < n; ++i) {
            maxLeaf[{i, i}] = arr[i];
            for (int j = i + 1; j < n; ++j) {
                maxLeaf[{i, j}] = max(arr[j], maxLeaf[{i, j - 1}]);
            }
        }

        // Fill the DP table
        for (int len = 1; len <= n; ++len) {
            for (int left = 0; left <= n - len; ++left) {
                int right = left + len - 1;
                if (left == right) {
                    dp[left][right] = 0;
                } else {
                    for (int k = left; k < right; ++k) {
                        dp[left][right] = min(dp[left][right], 
                            maxLeaf[{left, k}] * maxLeaf[{k + 1, right}] + dp[left][k] + dp[k + 1][right]);
                    }
                }
            }
        }
        return dp[0][n - 1];
    }
};
```

#### Explanation

1. **DP Table Setup**:
   - `dp[i][j]` stores the minimum non-leaf node sum for subarray `arr[i:j+1]`.

2. **Filling DP Table**:
   - Iterate over all possible subarray lengths and split each subarray `arr[i:j+1]` at every possible `k` to find the minimum cost.

#### Time Complexity

- **O(n^3)** due to nested loops and subarray splits.

#### Space Complexity

- **O(n^2)** for the DP table and `maxLeaf` map.

---

# Method 4 (Greedy | Heap based)

#### Code

```cpp
class Solution {
public:
    int mctFromLeafValues(vector<int>& arr) {
        int result = 0;
        while (arr.size() > 1) {
            int minProduct = INT_MAX;
            int index = -1;
            
            for (int i = 0; i < arr.size() - 1; ++i) {
                int product = arr[i] * arr[i + 1];
                if (product < minProduct) {
                    minProduct = product;
                    index = i;
                }
            }

            result += minProduct;
            arr[index] = max(arr[index], arr[index + 1]);
            arr.erase(arr.begin() + index + 1);
        }
        
        return result;
    }
};
```

#### Explanation

1. **Find the Minimum Product**:
   - At each step, find the smallest product of two adjacent elements and add this product to `result`.

2. **Combine Adjacent Nodes**:
   - After calculating the minimum product, replace one of the two nodes with the maximum of the two and remove the other. This ensures that we’re always minimizing the impact of each non-leaf node.

#### Time Complexity

- **O(n^2)**, since we need to find the minimum adjacent product and remove elements repeatedly.

#### Space Complexity

- **O(1)** (modifies `arr` in place).



---

If we want to use a **heap-based approach** for this problem, here’s how it would look:

### Greedy Approach Using a Min-Heap (Priority Queue) - **O(n log n)**

In this approach, we use a min-heap (priority queue) to continuously combine the smallest elements. The idea is to keep track of the two smallest adjacent elements to minimize the non-leaf node cost, which can be efficiently achieved with a heap.

#### Code

```cpp
#include <vector>
#include <queue>
#include <climits>

class Solution {
public:
    int mctFromLeafValues(std::vector<int>& arr) {
        int result = 0;
        std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap(arr.begin(), arr.end());
        
        while (minHeap.size() > 1) {
            // Remove the two smallest elements
            int first = minHeap.top();
            minHeap.pop();
            int second = minHeap.top();
            minHeap.pop();
            
            // Add their product to the result
            result += first * second;

            // Insert the larger of the two back into the heap
            minHeap.push(std::max(first, second));
        }

        return result;
    }
};
```

#### Explanation

1. **Initialize Min-Heap**:
   - Insert all elements from `arr` into a min-heap. The min-heap allows us to quickly access the two smallest elements.

2. **Combine the Two Smallest Elements**:
   - Repeatedly extract the two smallest elements from the heap.
   - Calculate their product and add it to `result` (this represents the non-leaf node sum).
   - Insert the larger of the two back into the heap because only one of the two values needs to be preserved in further calculations to minimize the cost.

3. **Return the Result**:
   - Once only one element remains in the heap, return `result`, which now contains the minimum possible sum of non-leaf nodes.

#### Time Complexity

- **O(n log n)** because each insertion and removal in the min-heap takes **O(log n)**, and we perform these operations for each element.

#### Space Complexity

- **O(n)** for storing all elements in the heap.

This approach ensures that the smallest adjacent values are combined first, which aligns with the greedy strategy of minimizing the cost at each step.
# 🔥🔥Method 5 (Most optimised | Stack based)

### Code

```cpp
int mctFromLeafValues(vector<int>& arr) {
        stack<int> s;
        s.push(INT_MAX);  // Sentinel to avoid stack underflow
        int res=0;
        for (int a:arr) {
            while (s.top()<=a) {
                int mid=s.top();
                s.pop();
                res+=mid*min(s.top(), a);
            }
            s.push(a);
        }
        
        // Remaining elements in the stack
        while (s.size()>2) {
            int mid=s.top();
            s.pop();
            res+=mid*s.top();
        }
        return res;
    }
```

---

### Explanation

1. **Initialize the Stack and Result**:
   - We initialize a `stack` with a sentinel value `INT_MAX` to prevent stack underflow and handle edge cases where no adjacent value is larger.
   - Initialize `result` to 0, which will store the sum of all non-leaf nodes.

2. **Iterate Through Each Element in `arr`**:
   - For each element `a` in `arr`, we want to merge it with smaller adjacent values to minimize the non-leaf node sum.

3. **Conditionally Pop Elements from the Stack**:
   - While the top of the stack is less than or equal to `a` (i.e., `s.top() <= a`), we pop the top element (smallest leaf node), say `mid`.
   - We then calculate a non-leaf node value as `mid * min(s.top(), a)`, where `s.top()` represents the next larger element left of `mid` in the stack. This ensures the smallest possible product sum.
   - Add this product to `result`.

4. **Push the Current Element onto the Stack**:
   - After popping all smaller elements, push `a` onto the stack to maintain a non-increasing order from top to bottom.

5. **Handle Remaining Stack Elements**:
   - After the loop, if there are still elements left in the stack (not including `INT_MAX`), pop each adjacent pair and add their product to `result`.

6. **Return the Result**:
   - The final `result` contains the smallest possible sum of non-leaf node values.

---

### Time Complexity

- **O(n)**, where `n` is the length of `arr`.
  - Each element is pushed and popped from the stack at most once, so the algorithm performs in linear time.

### Space Complexity

- **O(n)**, for the stack.
  - In the worst case, all elements are pushed onto the stack.

---

## Dry Run
### Initial Setup
- **Input Array**: `arr = [6, 2, 4]`
- **Stack**: `[INT_MAX]` (we start with a sentinel value `INT_MAX` to handle edge cases).
- **Result (Sum of Non-Leaf Nodes)**: `result = 0`

### Step-by-Step Dry Run

#### Step 1: Process the First Element (6)
- **Current element**: `a = 6`
- **Condition**: `s.top() <= a` is `false` (`INT_MAX > 6`), so we don’t pop anything from the stack.
- **Action**: Push `6` onto the stack.
  
    Stack after this step: `[INT_MAX, 6]`

#### Step 2: Process the Second Element (2)
- **Current element**: `a = 2`
- **Condition**: `s.top() <= a` is `false` (`6 > 2`), so we don’t pop anything from the stack.
- **Action**: Push `2` onto the stack.
  
    Stack after this step: `[INT_MAX, 6, 2]`

#### Step 3: Process the Third Element (4)
- **Current element**: `a = 4`
- **Condition**: `s.top() <= a` is `true` (`2 <= 4`), so we pop `2` from the stack to make it part of a non-leaf node.

    - **Popped Element**: `mid = 2`
    - **Next Top Element**: `s.top() = 6`
    - **Product Calculation**: We create a non-leaf node with value `mid * min(s.top(), a) = 2 * min(6, 4) = 2 * 4 = 8`.
    - **Add to Result**: `result += 8`, so `result = 8`.

    **Stack after popping**: `[INT_MAX, 6]`
    
- **Action**: Now that `s.top() > a` (`6 > 4`), we push `4` onto the stack.

    Stack after this step: `[INT_MAX, 6, 4]`

#### Step 4: Handle Remaining Elements in the Stack
Now that we’ve finished processing all elements in `arr`, we move to the final stage where we handle the remaining elements in the stack (excluding `INT_MAX`).

- **Condition**: The stack has more than two elements (`[INT_MAX, 6, 4]`), so we pop the top element (`4`).

    - **Popped Element**: `mid = 4`
    - **Next Top Element**: `s.top() = 6`
    - **Product Calculation**: We create a non-leaf node with value `mid * s.top() = 4 * 6 = 24`.
    - **Add to Result**: `result += 24`, so `result = 8 + 24 = 32`.

    **Stack after this pop**: `[INT_MAX, 6]`

- **Condition**: Now, only two elements remain in the stack (`[INT_MAX, 6]`), so we're done.

### Final Result
The total sum of non-leaf node values is `result = 32`.

### Summary of Non-Leaf Nodes and their Values
1. **Non-Leaf Node 1**: `2 * 4 = 8`
2. **Non-Leaf Node 2**: `4 * 6 = 24`

Total sum of non-leaf nodes = `8 + 24 = 32`

So, the minimum cost to build the tree is `32`, which matches the output.

### Key Points in this Dry Run
- The stack helps maintain an order where we can easily access the smallest nearby leaf values to form non-leaf nodes.
- By merging smaller values with the smallest larger neighbor repeatedly, we minimize each product sum step-by-step.
- Each element is only pushed and popped once, achieving optimal O(n) complexity.
