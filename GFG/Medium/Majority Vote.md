### Code
```cpp
class Solution {
  public:
    // Function to find the majority elements in the array
    vector<int> findMajority(vector<int>& nums) {
        unordered_map<int, int> map;
        for(auto i: nums){
            map[i]++;
        }
        int limit=nums.size()/3;
        vector<int> output;
        for(auto i:map){
            if(i.second>limit){
                output.push_back(i.first);
            }
        }
        if(output.empty()) {
            return {-1};
        }
        sort(output.begin(), output.end());
        return output;
    }
};
```
### Explanation:
The goal is to find the elements in the array `nums[]` that appear more than `n/3` times (where `n` is the size of the array). If no element satisfies this condition, the function should return `-1`. Here's how the algorithm works:
1. First, it counts the frequency of each element using a hash map (`unordered_map<int, int>`).
2. Then, it iterates through the map to find elements whose frequency exceeds `n/3`.
3. If such elements are found, they are added to the result array `output`.
4. If no element satisfies the condition, `-1` is added to `output`.
5. Finally, the result is sorted and returned in increasing order.

### Time Complexity:
- Building the frequency map takes O(n), where `n` is the number of elements in the input array `nums[]`, since we iterate through the array once.
- Iterating through the map to find elements that appear more than `n/3` times also takes O(n) in the worst case (when all elements are unique).
- Sorting the result takes O(m log m), where `m` is the number of majority elements found. In the worst case, `m` is a small constant because at most 2 candidates can have more than `n/3` occurrences.
- **Overall Time Complexity**: O(n) because sorting only happens on a small constant (m ≤ 2).

### Space Complexity:
- The `unordered_map` takes O(n) space to store the frequency of each element.
- The result array `output` takes O(1) additional space in the worst case because at most two elements can satisfy the condition of appearing more than `n/3` times.
- **Overall Space Complexity**: O(n).

### Dry Run:
Let's dry run the code on the first input example.

**Input**: `nums = [2, 1, 5, 5, 5, 5, 6, 6, 6, 6, 6]`
1. We create a frequency map:
   ```
   map = {2: 1, 1: 1, 5: 4, 6: 5}
   ```
2. We calculate the threshold as `n / 3 = 11 / 3 = 3`.
3. Now we check which elements have frequency greater than 3:
   - `5` has 4 occurrences (greater than 3), so it is added to `output`.
   - `6` has 5 occurrences (greater than 3), so it is also added to `output`.
4. The `output` becomes `[5, 6]`.
5. Sorting it (though already sorted), the final result is `[5, 6]`.

**Output**: `[5, 6]`
