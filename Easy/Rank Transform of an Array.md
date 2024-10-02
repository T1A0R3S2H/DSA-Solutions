### Code:
```cpp
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        set<int>st(begin(arr), end(arr));
        int x=1;
        unordered_map<int, int>rank;
        for(auto val:st) rank[val]=x++;
        for(auto &val:arr) val=rank[val];
        return arr;
    }
};
```


### Explanation:

Given the input array `[100, 100, 100]`, we need to replace each element with its rank, where the rank is determined by the size of the element. All elements with the same value must have the same rank. The solution follows these steps:

1. **Create a set from the array**: The `set<int> st(begin(arr), end(arr));` extracts unique elements from the array and automatically sorts them. In this case, the set will contain only one element, `{100}`.
   
2. **Assign ranks**: The `unordered_map<int, int> rank;` is used to assign a rank to each unique element. Since we only have one unique element (100), it is assigned rank 1.

3. **Replace elements with ranks**: We then iterate through the original array, replacing each element with its corresponding rank from the `rank` map. Since all the values in `arr` are 100, they all get replaced by 1.

4. **Return the modified array**: The final array `[1, 1, 1]` is returned.

### Time Complexity:

- **O(n log n)**: 
  - Constructing the set from the input array takes **O(n log n)**, where `n` is the size of the array, because a set maintains sorted order while removing duplicates.
  - Iterating through the set to fill the rank map takes **O(n)**.
  - Updating the original array with ranks takes **O(n)**.
  
Thus, the overall time complexity is **O(n log n)** due to set construction.

### Space Complexity:

- **O(n)**: 
  - The set `st` and the unordered map `rank` store up to `n` elements, so the space complexity is **O(n)**.
 

### Dry Run:

Let’s dry run the code with input `arr = [100, 100, 100]`.

1. **Input**: `arr = [100, 100, 100]`
   
2. **Step 1 - Set Construction**: 
   - Extract unique elements and sort them using a set:
   ```
   st = {100}
   ```

3. **Step 2 - Rank Assignment**: 
   - Assign ranks starting from 1:
   ```
   rank = {100: 1}
   ```

4. **Step 3 - Array Transformation**: 
   - Replace each element in the original array with its rank:
   ```
   arr = [rank[100], rank[100], rank[100]] = [1, 1, 1]
   ```

5. **Step 4 - Return Output**: 
   - The final transformed array is:
   ```
   Output: [1, 1, 1]
   ```

### Final Output:
For the input `[100, 100, 100]`, the output is `[1, 1, 1]`. Since all elements are the same, they share the same rank.
