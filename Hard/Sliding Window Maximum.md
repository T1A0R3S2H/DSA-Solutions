# Sliding Window + Deque
```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n=nums.size();
        deque<int>dq;
        int i=0, j=0;
        vector<int>res;
        while(j<n){
            while(!dq.empty() && dq.back()<nums[j]){
                dq.pop_back();
            }
            dq.push_back(nums[j]);
            if(j-i+1<k) j++;
            else if(j-i+1==k){
                res.push_back(dq.front());
                if (dq.front()==nums[i]) dq.pop_front();
                i++;
                j++;
            }
        }
        return res;
    }
};
```

### Detailed Explanation

1. **Initialization**:
    - `n` stores the size of the input array `nums`.
    - `dq` is a deque that will store the indices of the elements in the current sliding window.
    - `i` and `j` are pointers for the left and right ends of the sliding window, respectively.
    - `res` is a vector that will store the maximum values of each sliding window.

2. **Iteration through the Array**:
    - The `while (j < n)` loop continues until the right pointer `j` reaches the end of the array.

3. **Maintaining the Deque**:
    - The `while (!dq.empty() && nums[dq.back()] < nums[j])` loop ensures that the deque only contains indices of elements in non-increasing order of their values. This means the front of the deque always holds the index of the maximum element in the current window.
    - `dq.push_back(j)` adds the current index `j` to the deque.

4. **Sliding the Window**:
    - `if (j - i + 1 < k)`: If the current window size is less than `k`, simply expand the window by incrementing `j`.
    - `else if (j - i + 1 == k)`: When the window size reaches `k`:
        - `res.push_back(nums[dq.front()])` adds the maximum element in the current window (element at the index stored in the front of the deque) to the result vector.
        - If the index of the element that is about to be removed from the window (`i`) is at the front of the deque, remove it with `dq.pop_front()`.
        - Slide the window by incrementing both `i` and `j`.

5. **Return the Result**:
    - Finally, return the `res` vector containing the maximum values of each sliding window.

### Efficiency
- **Time Complexity**: \(O(n)\), where \(n\) is the number of elements in the input array. Each element is added and removed from the deque at most once.
- **Space Complexity**: \(O(k)\), for storing the indices in the deque.

This solution is efficient and leverages the properties of the deque to maintain a sliding window of maximum elements in linear time.

## Dry Run

### Example
Input: `nums = [1, 3, -1, -3, 5, 3, 6, 7]`, `k = 3`

### Step-by-Step Execution

**Initialization:**
- `n = 8` (size of nums)
- `dq = []` (deque is empty)
- `i = 0, j = 0` (window pointers)
- `res = []` (result vector is empty)

### Iteration through the Array

1. **First Iteration (j = 0)**
   - `nums[j] = 1`
   - `dq` is empty, so we add `j` to `dq`: `dq = [0]`
   - Window size (`j - i + 1 = 1`) is less than `k`, so increment `j`: `j = 1`

2. **Second Iteration (j = 1)**
   - `nums[j] = 3`
   - `nums[dq.back()] = 1 < 3`, so we remove `0` from `dq`: `dq = []`
   - Add `j` to `dq`: `dq = [1]`
   - Window size (`j - i + 1 = 2`) is less than `k`, so increment `j`: `j = 2`

3. **Third Iteration (j = 2)**
   - `nums[j] = -1`
   - `nums[dq.back()] = 3 > -1`, so we add `j` to `dq`: `dq = [1, 2]`
   - Window size (`j - i + 1 = 3`) is equal to `k`, so:
     - Add `nums[dq.front()] = 3` to `res`: `res = [3]`
     - `dq.front() = 1` is not `i = 0`, so no change in `dq`
     - Increment both `i` and `j`: `i = 1`, `j = 3`

4. **Fourth Iteration (j = 3)**
   - `nums[j] = -3`
   - `nums[dq.back()] = -1 > -3`, so we add `j` to `dq`: `dq = [1, 2, 3]`
   - Window size (`j - i + 1 = 3`) is equal to `k`, so:
     - Add `nums[dq.front()] = 3` to `res`: `res = [3, 3]`
     - `dq.front() = 1` is `i = 1`, so remove front from `dq`: `dq = [2, 3]`
     - Increment both `i` and `j`: `i = 2`, `j = 4`

5. **Fifth Iteration (j = 4)**
   - `nums[j] = 5`
   - `nums[dq.back()] = -3 < 5`, so remove `3` from `dq`: `dq = [2]`
   - `nums[dq.back()] = -1 < 5`, so remove `2` from `dq`: `dq = []`
   - Add `j` to `dq`: `dq = [4]`
   - Window size (`j - i + 1 = 3`) is equal to `k`, so:
     - Add `nums[dq.front()] = 5` to `res`: `res = [3, 3, 5]`
     - `dq.front() = 4` is not `i = 2`, so no change in `dq`
     - Increment both `i` and `j`: `i = 3`, `j = 5`

6. **Sixth Iteration (j = 5)**
   - `nums[j] = 3`
   - `nums[dq.back()] = 5 > 3`, so we add `j` to `dq`: `dq = [4, 5]`
   - Window size (`j - i + 1 = 3`) is equal to `k`, so:
     - Add `nums[dq.front()] = 5` to `res`: `res = [3, 3, 5, 5]`
     - `dq.front() = 4` is not `i = 3`, so no change in `dq`
     - Increment both `i` and `j`: `i = 4`, `j = 6`

7. **Seventh Iteration (j = 6)**
   - `nums[j] = 6`
   - `nums[dq.back()] = 3 < 6`, so remove `5` from `dq`: `dq = [4]`
   - `nums[dq.back()] = 5 < 6`, so remove `4` from `dq`: `dq = []`
   - Add `j` to `dq`: `dq = [6]`
   - Window size (`j - i + 1 = 3`) is equal to `k`, so:
     - Add `nums[dq.front()] = 6` to `res`: `res = [3, 3, 5, 5, 6]`
     - `dq.front() = 6` is not `i = 4`, so no change in `dq`
     - Increment both `i` and `j`: `i = 5`, `j = 7`

8. **Eighth Iteration (j = 7)**
   - `nums[j] = 7`
   - `nums[dq.back()] = 6 < 7`, so remove `6` from `dq`: `dq = []`
   - Add `j` to `dq`: `dq = [7]`
   - Window size (`j - i + 1 = 3`) is equal to `k`, so:
     - Add `nums[dq.front()] = 7` to `res`: `res = [3, 3, 5, 5, 6, 7]`
     - `dq.front() = 7` is not `i = 5`, so no change in `dq`
     - Increment both `i` and `j`: `i = 6`, `j = 8`

### Final Result

The final result vector `res` contains the maximum values for each sliding window of size `k`:

`res = [3, 3, 5, 5, 6, 7]`

This concludes the dry run, demonstrating how the algorithm maintains the deque to track the maximum values efficiently for each sliding window.
