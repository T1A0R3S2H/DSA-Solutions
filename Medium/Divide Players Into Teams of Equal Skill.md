# 🔥Method 1 (brute force)
### Code:
```cpp
class Solution {
public:
    long long dividePlayers(vector<int>& skill) {
        sort(skill.begin(), skill.end());
        int n=skill.size();
        int limit=skill[0]+skill[n-1];
        long long chem=0;
        for(int i=0;i<n/2;i++){
            if(skill[i]+skill[n-i-1]!=limit){
                return -1;
            }
            chem+=(long long)skill[i]*skill[n-i-1];
        }
        return chem;
    }
};
```

### Explanation:
The problem asks to divide players into teams of two such that the sum of their skills in each team is equal. The chemistry of a team is the product of the skills of the two players on that team. You need to find the total sum of chemistry of all the teams, or return `-1` if it's not possible to divide them as required.

Steps:
1. **Sort the skill array** so that we can easily pair the smallest and largest numbers.
2. **Initialize a target sum**, which is the sum of the first (smallest) and last (largest) element. This is the required sum for each team.
3. **Loop through the first half of the sorted array** and pair the smallest element with the largest, checking if the sum of each pair equals the target sum.
4. If the sum of any pair is not equal to the target, return `-1`, as it is not possible to divide them into valid teams.
5. **Calculate the chemistry** by multiplying the elements in the valid pairs and sum the chemistry of all pairs.
6. Return the total chemistry.

### Time Complexity:
- Sorting the `skill` array takes \(O(n \log n)\), where `n` is the size of the input array.
- The subsequent loop over half of the sorted array takes \(O(n/2) = O(n)\).
- Therefore, the overall time complexity is **O(n log n)** due to the sorting step.

### Space Complexity:
- The algorithm uses only a few extra variables (`n`, `limit`, `chem`) and modifies the input array in place, so the extra space complexity is **O(1)** (constant space).

### Dry Run:
Let’s dry-run the algorithm for **Example 1**:
```cpp
skill = [3,2,5,1,3,4]
```
1. After sorting: `[1, 2, 3, 3, 4, 5]`
2. The target limit: `1 + 5 = 6`
3. Loop through the first half:
   - Pair `1` and `5`: Their sum is `6` (valid), chemistry = `1 * 5 = 5`.
   - Pair `2` and `4`: Their sum is `6` (valid), chemistry = `2 * 4 = 8`.
   - Pair `3` and `3`: Their sum is `6` (valid), chemistry = `3 * 3 = 9`.
4. Total chemistry = `5 + 8 + 9 = 22`. Return `22`.

For **Example 3**:
```cpp
skill = [1,1,2,3]
```
1. After sorting: `[1, 1, 2, 3]`
2. The target limit: `1 + 3 = 4`
3. Loop through the first half:
   - Pair `1` and `3`: Their sum is `4` (valid).
   - Pair `1` and `2`: Their sum is `3` (invalid). So, return `-1`.

---
# 🔥Method 2 (without sorting)
Yes, it's possible to solve the problem without sorting, but it's important to understand how the problem's structure depends on forming valid player pairs.

### Problem Breakdown:
1. The task is to divide players into pairs in such a way that the sum of their skills is constant across all pairs.
2. Sorting ensures that we pair the smallest skill with the largest one, which works well for the solution. But without sorting, we need a way to verify that there exists such a valid pairing.

### Key Idea Without Sorting:
Instead of sorting, we can use a **hash map (or frequency array)** to count occurrences of each skill level. With this count, we can attempt to form valid pairs by matching the skill levels that sum up to the same value.

### Approach:
1. Calculate the total sum of the skill array. This sum must be divisible by `n/2` to ensure that pairs can be formed with equal sum.
2. Traverse the skill array and try to form pairs using the count of each skill.
3. For each skill, find its complement (i.e., the skill that, when added, gives the desired sum). Ensure the complement exists and has enough frequency to form the pair.

### Code:

```cpp
class Solution {
public:
    long long dividePlayers(vector<int>& skill) {
        unordered_map<int, int> freq;
        long long sum = 0;
        int n = skill.size();
        
        // Calculate the total sum of skill values
        for (int s : skill) {
            freq[s]++;
            sum += s;
        }
        
        // If total sum is not divisible by n/2, we cannot form valid pairs
        if (sum % (n / 2) != 0) return -1;
        
        // Each pair should sum up to this value
        int limit = sum / (n / 2);
        long long chem = 0;
        
        // Try to form pairs
        for (int s : skill) {
            if (freq[s] == 0) continue; // This skill has already been used
            int complement = limit - s;
            
            // If complement doesn't exist or is already used up, return -1
            if (freq[complement] <= 0) return -1;
            
            // Form a pair (s, complement)
            chem += (long long)s * complement;
            freq[s]--;
            freq[complement]--;
        }
        
        return chem;
    }
};
```

### Explanation:
- **Frequency map:** We store the count of each skill in the `freq` map.
- **Pair Sum:** The desired sum for each pair is calculated as `limit = sum / (n / 2)`.
- **Pair Matching:** For each skill, we find the complement that forms a valid pair and check if it exists using the `freq` map.
- **Update Chemistry:** When a valid pair is found, their product is added to the total chemistry.
- **Termination:** If we find a skill that can't be paired, we return `-1`.

### Time Complexity:
- **O(n):** Building the frequency map and iterating over the skills both take linear time.

### Space Complexity:
- **O(n):** Storing the frequency map requires space proportional to the number of distinct skills.

This approach eliminates the need for sorting and achieves an optimal solution.

### Dry Run:
Let's perform a **dry run** of the optimized solution (without sorting) using the following input:

#### Input:
```cpp
skill = [3, 6, 2, 5]
```

#### Steps:

1. **Initialization:**
   - `n = 4` (size of `skill` array).
   - Create a frequency map (`freq`) to store the occurrences of each skill.
   - Calculate the total sum of all skill values:
     \[
     \text{sum} = 3 + 6 + 2 + 5 = 16
     \]
   - Since we are dividing players into pairs, each pair's sum should be:
     \[
     \text{limit} = \frac{\text{sum}}{n / 2} = \frac{16}{2} = 8
     \]

2. **Building the frequency map:**
   The frequency map is built as we iterate through the `skill` array:
   - For skill `3`, update `freq[3] = 1`.
   - For skill `6`, update `freq[6] = 1`.
   - For skill `2`, update `freq[2] = 1`.
   - For skill `5`, update `freq[5] = 1`.

   Now, `freq = {3: 1, 6: 1, 2: 1, 5: 1}`.

3. **Pairing Process:**
   - Start iterating over the `skill` array to form pairs.

   **1st Iteration (skill = 3):**
   - We need to find a complement for `3` such that their sum equals `8`.
   - Complement = `8 - 3 = 5`.
   - Check if `5` exists in `freq` and its frequency is greater than `0`.
     - `freq[5] = 1`, so it exists.
   - Form the pair `(3, 5)` and update the total chemistry:
     \[
     \text{chemistry} += 3 \times 5 = 15
     \]
   - Decrement the frequency of `3` and `5`:
     - `freq[3]--` → `freq[3] = 0`
     - `freq[5]--` → `freq[5] = 0`
   - Updated `freq = {3: 0, 6: 1, 2: 1, 5: 0}`.

   **2nd Iteration (skill = 6):**
   - We need to find a complement for `6` such that their sum equals `8`.
   - Complement = `8 - 6 = 2`.
   - Check if `2` exists in `freq` and its frequency is greater than `0`.
     - `freq[2] = 1`, so it exists.
   - Form the pair `(6, 2)` and update the total chemistry:
     \[
     \text{chemistry} += 6 \times 2 = 12
     \]
   - Decrement the frequency of `6` and `2`:
     - `freq[6]--` → `freq[6] = 0`
     - `freq[2]--` → `freq[2] = 0`
   - Updated `freq = {3: 0, 6: 0, 2: 0, 5: 0}`.

4. **Final Check:**
   - All skills have been paired successfully, and the chemistry is:
     \[
     \text{chemistry} = 15 + 12 = 27
     \]

#### Output:
The function returns `27` as the total chemistry.

---

#### Summary of Dry Run:
- **Pairs formed:** (3, 5) and (6, 2).
- **Total chemistry:** 27.
- The solution successfully divides the players into pairs with equal sum and calculates the chemistry accordingly.
