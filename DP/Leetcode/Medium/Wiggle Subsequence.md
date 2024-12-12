# Method 1 (without DP)
### CETSD (Code + ETSD)

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if (nums.size() < 2) return nums.size();
        vector<int> differences;

        for (int i = 1; i < nums.size(); i++) {
            int diff = nums[i] - nums[i - 1];
            if (diff != 0) { // Ignore zero differences
                differences.push_back(diff);
            }
        }

        if (differences.empty()) return 1; // If all differences are zero, the longest wiggle sequence is 1.

        // Check for alternating pattern
        int count = 1; // At least one element is part of the wiggle sequence
        for (int i = 1; i < differences.size(); i++) {
            if ((differences[i] > 0 && differences[i - 1] < 0) ||
                (differences[i] < 0 && differences[i - 1] > 0)) {
                count++; // Count the wiggle if it alternates
            }
        }

        return count + 1; // Include the first element in the count
    }
};
```

---

### Explanation

1. **Step 1: Base Case**
   ```cpp
   if (nums.size() < 2) return nums.size();
   ```
   - **Kyun kiya?** Agar `nums` ki size 2 se chhoti hai (matlab array me sirf 0 ya 1 element hai), to poori sequence ek wiggle sequence hai.
   - **Kaise?** Single-element or empty array har case me valid wiggle sequence hoga.
   - **Example:**  
     - `nums = [1]` → Output: `1`
     - `nums = []` → Output: `0`

---

2. **Step 2: Calculate Differences**
   ```cpp
   for (int i = 1; i < nums.size(); i++) {
       int diff = nums[i] - nums[i - 1];
       if (diff != 0) { // Ignore zero differences
           differences.push_back(diff);
       }
   }
   ```
   - **Kyun kiya?** Har consecutive elements ka difference calculate karte hain.
   - **Kaise?** Agar difference `0` hai, to usse ignore karte hain kyunki woh wiggle pattern nahi banata. Sirf non-zero differences store karte hain.
   - **Example:**  
     - `nums = [1, 2, 2, 3]` → `differences = [1, 1]`
     - `nums = [1, 7, 7, 4, 9]` → `differences = [6, -3, 5]`

---

3. **Step 3: Handle All Zero Differences**
   ```cpp
   if (differences.empty()) return 1;
   ```
   - **Kyun kiya?** Agar sabhi differences `0` hai, to array ka longest wiggle sequence sirf ek element hoga.
   - **Kaise?** Empty `differences` ka matlab hai ki array ke sabhi elements same hain.
   - **Example:**  
     - `nums = [5, 5, 5]` → `differences = []` → Output: `1`

---

4. **Step 4: Check for Alternating Pattern**
   ```cpp
   int count = 1; // At least one element is part of the wiggle sequence
   for (int i = 1; i < differences.size(); i++) {
       if ((differences[i] > 0 && differences[i - 1] < 0) ||
           (differences[i] < 0 && differences[i - 1] > 0)) {
           count++;
       }
   }
   ```
   - **Kyun kiya?** Alternate positive-negative pattern check karte hain.  
     - Positive ke baad negative aana chahiye.
     - Negative ke baad positive aana chahiye.
   - **Kaise?** 
     - Agar condition match hoti hai, to `count` ko increment karte hain.
   - **Example:**  
     - `differences = [6, -3, 5, -7, 3]`  
       - `6 > 0, -3 < 0` → `count = 2`
       - `-3 < 0, 5 > 0` → `count = 3`
       - `5 > 0, -7 < 0` → `count = 4`
       - `-7 < 0, 3 > 0` → `count = 5`

---

5. **Step 5: Return Final Count**
   ```cpp
   return count + 1; // Include the first element in the count
   ```
   - **Kyun kiya?** Wiggle sequence ke count me first element ko include karte hain kyunki woh hamesha part hota hai.

---

### Time Complexity
- **O(n)**:
  - Differences calculate karne me \(O(n)\).
  - Alternating pattern check karne me \(O(n)\).

### Space Complexity
- **O(n)**:
  - Differences ke liye ek vector use kiya gaya hai.

---

### Dry Run

#### Input: `nums = [1, 7, 4, 9, 2, 5]`
1. **Base Case Check**:
   - `nums.size() = 6` → Proceed.

2. **Calculate Differences**:
   - `differences = [6, -3, 5, -7, 3]`

3. **Check Alternating Pattern**:
   - Compare:
     - `6 > 0, -3 < 0` → `count = 2`
     - `-3 < 0, 5 > 0` → `count = 3`
     - `5 > 0, -7 < 0` → `count = 4`
     - `-7 < 0, 3 > 0` → `count = 5`

4. **Return Result**:
   - `count + 1 = 6`

Output: `6`

---



# Method 2 (DP use karke)
**DP** (Dynamic Programming) ka use karke bhi isko solve kar sakte hain. DP ka approach thoda systematic hota hai jisme hum har index ke liye longest wiggle subsequence ka length calculate karte hain based on previous elements. Let me explain the DP approach step by step:

---

### Approach (Using DP)

1. **DP Array Definition**:
   - Hum do DP arrays banate hain:
     - `up[i]`: Longest wiggle subsequence ending at index `i`, where the last difference is positive.
     - `down[i]`: Longest wiggle subsequence ending at index `i`, where the last difference is negative.

2. **Transition Logic**:
   - **If `nums[i] > nums[j]` (difference positive)**:
     - `up[i] = max(up[i], down[j] + 1)`
   - **If `nums[i] < nums[j]` (difference negative)**:
     - `down[i] = max(down[i], up[j] + 1)`
   - **If `nums[i] == nums[j]`**:
     - Skip, no change.

3. **Result**:
   - Maximum of the last elements in `up` and `down` arrays gives the result.

---


---

### Explanation of the Code

1. **Base Case**:
   - If the array has less than 2 elements, return its size as it is trivially a wiggle sequence.

2. **Initialization**:
   - `up[i]` and `down[i]` are initialized to 1 for all indices because the minimum wiggle subsequence length ending at any index is 1.

3. **Nested Loops**:
   - The outer loop iterates over the current index `i`.
   - The inner loop iterates over all previous indices `j`.
   - For each pair `(i, j)`, it checks whether the difference `nums[i] - nums[j]` is positive, negative, or zero and updates the respective DP arrays accordingly.

4. **Result**:
   - The longest wiggle subsequence is the maximum value in `up` and `down` at the last index.

---

### Optimization for O(n) Time Complexity

To optimize this approach to **O(n)**:
- Instead of maintaining full `up` and `down` arrays, maintain two variables:
  - `up`: Tracks the longest wiggle subsequence ending with a positive difference.
  - `down`: Tracks the longest wiggle subsequence ending with a negative difference.

### Optimized Code

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if (nums.size() < 2) return nums.size();

        int up = 1, down = 1; // Initialize both to 1

        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] > nums[i - 1]) {
                up = down + 1; // Increase up when the difference is positive
            } else if (nums[i] < nums[i - 1]) {
                down = up + 1; // Increase down when the difference is negative
            }
        }

        return max(up, down); // Maximum of both gives the result
    }
};
```

---

### Dry Run (Optimized Approach)

#### Input: `nums = [1, 7, 4, 9, 2, 5]`

1. Initialize: `up = 1`, `down = 1`
2. Iterate through `nums`:
   - `i = 1`: `nums[1] > nums[0]` → `up = down + 1 = 2`
   - `i = 2`: `nums[2] < nums[1]` → `down = up + 1 = 3`
   - `i = 3`: `nums[3] > nums[2]` → `up = down + 1 = 4`
   - `i = 4`: `nums[4] < nums[3]` → `down = up + 1 = 5`
   - `i = 5`: `nums[5] > nums[4]` → `up = down + 1 = 6`
3. Result: `max(up, down) = 6`

---

### Time and Space Complexity

#### Optimized Approach
- **Time Complexity**: \(O(n)\), as we iterate through the array once.
- **Space Complexity**: \(O(1)\), as we only use two variables (`up` and `down`).
