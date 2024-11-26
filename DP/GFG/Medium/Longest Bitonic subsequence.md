
#### Code
```cpp
class Solution {
public:
    int LongestBitonicSequence(int n, vector<int> &nums) {
        vector<int> inc(n, 1), dec(n, 1);

        // Calculate LIS from left to right
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    inc[i] = max(inc[i], inc[j] + 1);
                }
            }
        }

        // Calculate LIS from right to left for decreasing sequence
        for (int i = n - 1; i >= 0; i--) {
            for (int j = n - 1; j > i; j--) {
                if (nums[i] > nums[j]) {
                    dec[i] = max(dec[i], dec[j] + 1);
                }
            }
        }

        // Calculate the maximum length of the bitonic sequence
        int maxLength = 0;
        for (int i = 0; i < n; i++) {
            if (inc[i] > 1 && dec[i] > 1) {
                maxLength = max(maxLength, inc[i] + dec[i] - 1);
            }
        }

        return maxLength;
    }
};
```

---

### Explanation
1. **Increasing Subsequence (`inc` array)**:  
   We iterate from left to right, and for each `i`, calculate the length of the longest increasing subsequence ending at `i`.  
   This is done by comparing `nums[i]` with all previous elements (`nums[j]`) and updating `inc[i]` accordingly.

2. **Decreasing Subsequence (`dec` array)**:  
   We iterate from right to left, and for each `i`, calculate the length of the longest decreasing subsequence starting at `i`.  
   This is done by comparing `nums[i]` with all subsequent elements (`nums[j]`) and updating `dec[i]`.

3. **Combine Results**:  
   For each `i`, the total length of the bitonic sequence centered at `i` is `inc[i] + dec[i] - 1`.  
   The subtraction is to avoid double-counting `nums[i]`.  
   We track the maximum length of such sequences, ensuring both increasing and decreasing parts exist (`inc[i] > 1 && dec[i] > 1`).

4. **Edge Cases**:  
   - If the array is entirely increasing or decreasing, no valid bitonic sequence exists (result is `0`).

---

### Time Complexity
1. **Left-to-right LIS calculation**: \(O(n^2)\).  
   Nested loops iterate through pairs of indices.
   
2. **Right-to-left LIS calculation**: \(O(n^2)\).  
   Similar nested loops for decreasing sequence.

3. **Final Combination**: \(O(n)\).  
   Single traversal to find the maximum bitonic sequence length.

**Overall**: \(O(n^2)\).

---

### Space Complexity
1. `inc` and `dec` arrays each require \(O(n)\) space.
2. Total auxiliary space: \(O(n)\).

---

### Dry Run

#### Input:  
\(n = 5, nums = [1, 2, 5, 3, 2]\)

#### Step-by-step Execution:  

1. **Initialize `inc` and `dec`**:  
   ```
   inc = [1, 1, 1, 1, 1]  
   dec = [1, 1, 1, 1, 1]
   ```

2. **Calculate Increasing Subsequences (`inc` array)**:  
   - For \(i = 0\): No previous elements, `inc[0]` remains 1.  
   - For \(i = 1\): Compare with \(j = 0\). Since \(nums[1] > nums[0]\), update:  
     `inc[1] = inc[0] + 1 = 2`.  
     ```
     inc = [1, 2, 1, 1, 1]
     ```
   - For \(i = 2\): Compare with \(j = 0, 1\). Update:  
     `inc[2] = max(inc[2], inc[1] + 1) = 3`.  
     ```
     inc = [1, 2, 3, 1, 1]
     ```
   - For \(i = 3\): Compare with \(j = 0, 1, 2\). Update:  
     `inc[3] = max(inc[3], inc[1] + 1) = 3`.  
     ```
     inc = [1, 2, 3, 3, 1]
     ```
   - For \(i = 4\): Compare with \(j = 0, 1, 2, 3\). Update:  
     `inc[4] = max(inc[4], inc[1] + 1) = 3`.  
     ```
     inc = [1, 2, 3, 3, 3]
     ```

3. **Calculate Decreasing Subsequences (`dec` array)**:  
   - For \(i = 4\): No subsequent elements, `dec[4]` remains 1.  
   - For \(i = 3\): Compare with \(j = 4\). Since \(nums[3] > nums[4]\), update:  
     `dec[3] = dec[4] + 1 = 2`.  
     ```
     dec = [1, 1, 1, 2, 1]
     ```
   - For \(i = 2\): Compare with \(j = 3, 4\). Update:  
     `dec[2] = dec[3] + 1 = 3`.  
     ```
     dec = [1, 1, 3, 2, 1]
     ```
   - For \(i = 1\): Compare with \(j = 2, 3, 4\). No updates since `nums[1] < nums[j]`.  
     ```
     dec = [1, 1, 3, 2, 1]
     ```
   - For \(i = 0\): Compare with \(j = 1, 2, 3, 4\). No updates since `nums[0] < nums[j]`.  
     ```
     dec = [1, 1, 3, 2, 1]
     ```

4. **Calculate Maximum Length**:  
   ```
   maxLength = 0  
   For i = 0: inc[0] + dec[0] - 1 = 1 + 1 - 1 = 1 (Invalid, inc[i] and dec[i] <= 1)  
   For i = 1: inc[1] + dec[1] - 1 = 2 + 1 - 1 = 2 (Invalid)  
   For i = 2: inc[2] + dec[2] - 1 = 3 + 3 - 1 = 5 (Valid)  
   For i = 3: inc[3] + dec[3] - 1 = 3 + 2 - 1 = 4 (Valid)  
   For i = 4: inc[4] + dec[4] - 1 = 3 + 1 - 1 = 3 (Invalid)
   ```

   Final `maxLength = 5`.

#### Output:  
5




---

### Why -1 in the o/p?

The subtraction of \( -1 \) in `inc[i] + dec[i] - 1` is necessary to avoid **double-counting** the element at position \( i \), which acts as the "peak" of the bitonic subsequence.

### Detailed Reason:
1. **`inc[i]`**:
   - Represents the length of the longest increasing subsequence **up to and including** \( nums[i] \).
   
2. **`dec[i]`**:
   - Represents the length of the longest decreasing subsequence **starting from and including** \( nums[i] \).

3. **Overlap**:
   - Since \( nums[i] \) is included in both `inc[i]` and `dec[i]`, it gets **counted twice** when summing them. To correct this, we subtract \( 1 \) to ensure the peak element is counted only once.

### Without `-1`:
If we don't subtract \( 1 \), the result will overestimate the length of the bitonic sequence.

### Example:
#### Input:  
`nums = [1, 2, 5, 3, 2]`

#### Calculation:
- `inc = [1, 2, 3, 3, 3]`  
- `dec = [1, 1, 3, 2, 1]`

At \( i = 2 \) (\( nums[2] = 5 \)):  
- Longest increasing subsequence = `[1, 2, 5]`, length = `3`.  
- Longest decreasing subsequence = `[5, 3, 2]`, length = `3`.  

Without \( -1 \):  
- \( inc[2] + dec[2] = 3 + 3 = 6 \)  
This counts \( 5 \) twice.

With \( -1 \):  
- \( inc[2] + dec[2] - 1 = 3 + 3 - 1 = 5 \), which is correct.

### Conclusion:
The \( -1 \) ensures the bitonic sequence length is calculated correctly by avoiding double-counting the peak element.
