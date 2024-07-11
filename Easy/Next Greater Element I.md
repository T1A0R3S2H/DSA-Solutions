# Method 1 (brute force)
```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector<int> ans(nums1.size(), -1);  // Initialize the result vector with -1
        
        for (int i = 0; i < nums1.size(); i++) {
            bool found = false;
            for (int j = 0; j < nums2.size(); j++) {
                if (nums2[j] == nums1[i]) {
                    found = true;
                }
                if (found && nums2[j] > nums1[i]) {
                    ans[i] = nums2[j];
                    break;
                }
            }
        }
        return ans;
    }
};

```

# Method 2 (stacks)
A common approach is to use a stack to keep track of the elements and their next greater elements while iterating through `nums2`.

Here’s the corrected version of the approach:

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> nextGreaterMap;
        stack<int> s;
        
        // Traverse nums2 to fill the map with the next greater elements
        for (int num : nums2) {
            while (!s.empty() && s.top() < num) {
                nextGreaterMap[s.top()] = num;
                s.pop();
            }
            s.push(num);
        }
        
        // For elements that have no next greater element, assign -1
        while (!s.empty()) {
            nextGreaterMap[s.top()] = -1;
            s.pop();
        }
        
        // Create the result array for nums1 based on the next greater map
        vector<int> result;
        for (int num : nums1) {
            result.push_back(nextGreaterMap[num]);
        }
        
        return result;
    }
};
```

### Explanation:
1. **Mapping the Next Greater Elements**:
    - We use a stack to keep track of elements for which we have not yet found the next greater element.
    - As we iterate through `nums2`, for each element, we check if it is greater than the element at the top of the stack.
    - If it is, we pop from the stack and set the current element as the next greater element in the map.
    - We push the current element onto the stack.

2. **Handling Remaining Elements**:
    - After the loop, any elements left in the stack do not have a next greater element, so we map them to `-1`.

3. **Building the Result**:
    - We iterate through `nums1` and use the map to construct the result array, which contains the next greater elements for the elements in `nums1`.

This approach efficiently computes the next greater elements in O(nums1.length + nums2.length) time, as required.
