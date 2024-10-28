### Code
```cpp
class Solution {
public:
    int longestSquareStreak(std::vector<int>& nums) {
        unordered_set<int> st(nums.begin(), nums.end());
        int maxi=-1;
        for (int num:nums) {
            int count=1;
            long long current=num;
            while (true) {
                long long next=current*current; // Calculate the next square
                if (next>100000 || st.find(next)==st.end()) { // Check for overflow and existence
                    break; // Exit if next square is too large or not found
                }
                current=next;
                count++;
            }

            // Check for at least 2 elements in the square streak
            if (count>=2) {
                maxi=max(maxi, count);
            }
        }
        return maxi;
    }
};

```

### Explanation
1. **Initialization**:
   - An unordered set `st` is created from `nums` to allow O(1) average time complexity for lookups.
   - An integer `maxi` is initialized to -1 to track the longest square streak found.

2. **Outer Loop**:
   - Iterate over each number `num` in `nums`.
   - Initialize `count` to 1, since the number itself is the start of a potential square streak.
   - Set `current` to `num`.

3. **Finding the Square Streak**:
   - Enter an infinite loop to check for square streaks.
   - Calculate `next` as `current * current`.
   - Check if `next` exceeds `100000` or if it is not found in the set `st`. If either condition is true, break the loop.
   - If valid, update `current` to `next` and increment the count.

4. **Update Maximum Streak**:
   - After exiting the loop, check if `count` is at least 2 (as the problem specifies a square streak requires a length of at least 2).
   - If so, update `maxi` to be the maximum of its current value and the new count.

5. **Return Result**:
   - Finally, return `maxi`, which holds the length of the longest square streak found, or -1 if none exists.

### Time Complexity
- **O(n log m)**:
  - The outer loop runs for each element in `nums`, giving an O(n) complexity.
  - The inner loop checks for squares, which can be considered logarithmic in nature due to the exponential growth of squared numbers, leading to a maximum of O(log m) checks where `m` is the maximum value in `nums`. This results in an overall complexity of O(n log m).

### Space Complexity
- **O(n)**:
  - The unordered set `st` stores all unique elements of `nums`, which in the worst case takes O(n) space.

### Dry Run
Let’s dry run the function with an example:

**Input**: `nums = [4, 3, 6, 16, 8, 2]`

1. **Set Creation**:
   - `st = {2, 3, 4, 6, 8, 16}`

2. **Iteration**:
   - For `num = 4`:
     - `count = 1`, `current = 4`
     - Calculate `next = 16` (4 * 4) → found in `st`, `count = 2`, `current = 16`.
     - Calculate `next = 256` (16 * 16) → exceeds 100000, break the loop.
     - Update `maxi = max(-1, 2) = 2`.

   - For `num = 3`:
     - `count = 1`, `current = 3`
     - Calculate `next = 9` (3 * 3) → not found in `st`, break the loop.
     - `maxi` remains 2.

   - For `num = 6`:
     - `count = 1`, `current = 6`
     - Calculate `next = 36` (6 * 6) → not found in `st`, break the loop.
     - `maxi` remains 2.

   - For `num = 16`:
     - `count = 1`, `current = 16`
     - Calculate `next = 256` (16 * 16) → exceeds 100000, break the loop.
     - `maxi` remains 2.

   - For `num = 8`:
     - `count = 1`, `current = 8`
     - Calculate `next = 64` (8 * 8) → not found in `st`, break the loop.
     - `maxi` remains 2.

   - For `num = 2`:
     - `count = 1`, `current = 2`
     - Calculate `next = 4` (2 * 2) → found in `st`, `count = 2`, `current = 4`.
     - Calculate `next = 16` (4 * 4) → found in `st`, `count = 3`, `current = 16`.
     - Calculate `next = 256` (16 * 16) → exceeds 100000, break the loop.
     - Update `maxi = max(2, 3) = 3`.

3. **Final Output**:
   - The function returns `3`, which is the length of the longest square streak: `[2, 4, 16]`.

### Conclusion
This implementation efficiently identifies the longest square streak in the input array, handling potential overflow issues and meeting the problem requirements.
