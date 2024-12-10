# Method 1 🔥🔥:

### Code:

```cpp
class Solution {
public:
    // Helper function for the recursive + memoization approach
    int solveMem(int num, vector<int>& dp, unordered_map<int, int>& freq) {
        if (num <= 0) return 0;
        
        // If the result is already computed, return it from dp
        if (dp[num] != -1) return dp[num];
        
        // Maximize the points by either:
        // - Skipping the current number (take dp[num-1])
        // - Taking the current number and removing num-1 (take dp[num-2] + freq[num] * num)
        dp[num] = max(solveMem(num - 1, dp, freq), solveMem(num - 2, dp, freq) + freq[num] * num);
        
        return dp[num];
    }

    int deleteAndEarn(vector<int>& nums) {
        // Create a frequency map of the numbers
        unordered_map<int, int> freq;
        int maxVal = 0;
        
        // Calculate the frequency of each number in nums and find the max value
        for (int num : nums) {
            freq[num]++;
            maxVal = max(maxVal, num);
        }

        // Create a memoization table (dp) for storing the maximum score up to each number
        vector<int> dp(maxVal + 1, -1);

        // Call the helper function to compute the maximum score
        return solveMem(maxVal, dp, freq);
    }
};
```

### Explanation:

1. **Frequency Map**:
   - We create an `unordered_map<int, int> freq` to store the frequency of each number in the `nums` array. This map helps us calculate how many times each number appears, which we will use to compute the total points for each number.

2. **Recursive + Memoization**:
   - The `solveMem` function is a recursive helper function with memoization:
     - It calculates the maximum points we can earn considering numbers up to `num`.
     - We either:
       - **Skip** the number `num` (by taking `solveMem(num - 1)`).
       - **Take** the number `num` (by taking `solveMem(num - 2) + freq[num] * num` to avoid adjacent numbers).
     - The result is stored in the `dp` array for each number `num` to avoid recomputing the same value multiple times.

3. **Base Case**:
   - The recursion stops when `num <= 0`, returning 0, meaning there are no more numbers to consider.
   
4. **Memoization Table**:
   - We use a `dp` array initialized to `-1` to store the maximum score we can achieve up to each number `num`.

5. **Main Function** (`deleteAndEarn`):
   - The main function first calculates the frequency of each number in the input array `nums` and finds the maximum number `maxVal`.
   - We then call the `solveMem` function to compute the result for the largest possible number (`maxVal`).

### Time Complexity:

- **Time Complexity**:  
  The time complexity is **O(n + m)**, where:
  - `n` is the length of the input array `nums` (we iterate through `nums` to build the frequency map).
  - `m` is the maximum number in the array (`maxVal`). We perform recursive calls up to `maxVal`, and memoization ensures each call is computed only once.

  Hence, the total complexity is **O(n + m)**.

- **Space Complexity**:
  - The space complexity is **O(m)**, where:
    - The `dp` array has size `m + 1`, where `m` is the maximum number in the array.
    - The frequency map `freq` stores the count of each number, and it requires **O(m)** space in the worst case (when all numbers in the array are distinct).

  Therefore, the space complexity is **O(m)**.

### Dry Run:

Let’s dry run the example:  
`nums = [3, 4, 2]`, `n = 3`.

1. **Frequency Map**:
   ```
   freq = {2: 1, 3: 1, 4: 1}
   maxVal = 4
   ```

2. **Memoization Table (`dp`)**:
   ```
   dp = [-1, -1, -1, -1, -1]  // dp[0] to dp[4]
   ```

3. **Recursive Call (`solveMem`)**:
   - Start with `solveMem(4, dp, freq)`:
     - `solveMem(4)`:
       - Either skip `4`: `solveMem(3)`
       - Or take `4`: `solveMem(2) + freq[4] * 4 = solveMem(2) + 4`
     - `solveMem(3)`:
       - Either skip `3`: `solveMem(2)`
       - Or take `3`: `solveMem(1) + freq[3] * 3 = solveMem(1) + 3`
     - Continue recursively until reaching the base cases and memoize the results in the `dp` table.

4. **Final DP Table** after computation:
   ```
   dp = [0, 0, 2, 4, 6]
   ```

5. **Result**:
   - The final result is `dp[4] = 6`, so the maximum points we can earn is `6`.

### Final Output:

```
6
```

### Summary:

- The code uses **memoization** to optimize the recursive solution. We calculate the maximum score for each number by either skipping it or taking it and removing adjacent numbers, storing the results in a `dp` array to avoid redundant calculations.
- Time and space complexity are both **O(n + m)**, where `n` is the length of the array and `m` is the largest number in the array.

Let me know if you need any further clarification!

---

# Method 2 🔥🔥:
#### Code:
```cpp
class Solution {
public:
    int solveMem(vector<int>& points, int n, vector<int>& dp) {
        if (n <= 0) return 0; // Base case: no points can be earned
        if (dp[n] != -1) return dp[n]; // Return cached result if already computed
        
        // Take or skip
        int take = points[n] + solveMem(points, n - 2, dp); // Take current and skip one
        int skip = solveMem(points, n - 1, dp); // Skip current
        
        return dp[n] = max(take, skip); // Store and return maximum result
    }

    int deleteAndEarn(vector<int>& nums) {
        int maxNum = *max_element(nums.begin(), nums.end()); // Find max number in nums
        vector<int> points(maxNum + 1, 0); // Array to store total points for each number
        
        // Fill points array
        for (int num : nums) {
            points[num] += num;
        }

        vector<int> dp(maxNum + 1, -1); // Memoization table
        return solveMem(points, maxNum, dp);
    }
};
```
![image](https://github.com/user-attachments/assets/732df380-e75d-4c63-bfd6-93ab12a417ef)

---

### Explanation:

1. **Transformation of Input (`points` array):**
   - Instead of working directly with `nums`, convert it to a `points` array.
   - For a number `x` in `nums`, add `x` to `points[x]`. This accumulates the total points from all occurrences of each number.
   - Example: `nums = [3, 4, 2]` → `points = [0, 0, 2, 3, 4]`.

2. **Recursive Approach with Memoization:**
   - Solve the problem like the **House Robber Problem**:
     - Decide for each index `n` in `points`:
       - Either **take it** (and skip the adjacent index).
       - Or **skip it** and move to the next index.
   - Use recursion with memoization to compute results efficiently.

3. **Memoization Table (`dp`):**
   - `dp[i]` stores the maximum points that can be earned from indices `1` to `i` in the `points` array.

---

### Time Complexity:
- **O(n):** We iterate through the `points` array once to fill it, and solve the problem for each index `1` to `maxNum` exactly once due to memoization.

### Space Complexity:
- **O(n):** Space for the `points` array and the `dp` array.

---

### Dry Run:

#### Input:
`nums = [3, 4, 2]`

#### Transformation:
`points = [0, 0, 2, 3, 4]`  
`maxNum = 4`  
`dp = [-1, -1, -1, -1, -1]`

---

#### Recursive Steps:

1. **`solveMem(points, 4, dp)`**  
   - Take: `points[4] + solveMem(points, 2, dp)`  
     - `4 + solveMem(points, 2, dp)`  
   - Skip: `solveMem(points, 3, dp)`  
   - Result: `max(4 + solveMem(points, 2, dp), solveMem(points, 3, dp))`.

2. **`solveMem(points, 3, dp)`**  
   - Take: `points[3] + solveMem(points, 1, dp)`  
     - `3 + solveMem(points, 1, dp)`  
   - Skip: `solveMem(points, 2, dp)`  
   - Result: `max(3 + solveMem(points, 1, dp), solveMem(points, 2, dp))`.

3. **`solveMem(points, 2, dp)`**  
   - Take: `points[2] + solveMem(points, 0, dp)`  
     - `2 + solveMem(points, 0, dp)`  
   - Skip: `solveMem(points, 1, dp)`  
   - Result: `max(2 + solveMem(points, 0, dp), solveMem(points, 1, dp))`.

4. **`solveMem(points, 1, dp)`**  
   - Base case: `n <= 0` → `return 0`.

5. **`solveMem(points, 0, dp)`**  
   - Base case: `n <= 0` → `return 0`.

---

#### Backtracking with Results:

- **`solveMem(points, 1, dp)` → `0`**  
- **`solveMem(points, 2, dp)`**  
  - Take: `2 + 0 = 2`.  
  - Skip: `0`.  
  - Result: `max(2, 0) = 2`.  
  - `dp[2] = 2`.

- **`solveMem(points, 3, dp)`**  
  - Take: `3 + 0 = 3`.  
  - Skip: `2`.  
  - Result: `max(3, 2) = 3`.  
  - `dp[3] = 3`.

- **`solveMem(points, 4, dp)`**  
  - Take: `4 + 2 = 6`.  
  - Skip: `3`.  
  - Result: `max(6, 3) = 6`.  
  - `dp[4] = 6`.

---

#### Final Output:
- `dp[maxNum] = dp[4] = 6`.  
- **Answer: `6`**.

--- 
