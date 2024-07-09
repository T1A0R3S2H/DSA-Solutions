# Method 1 (stacks)
### TC: O(n): as eacch element ka iteration exactly once.
### SC: O(n)
```cpp
#include <vector>
#include <stack>

class Solution {
public:
    vector<int> help_classmate(vector<int> arr, int n) {
        vector<int> result(n, -1);
        stack<int> s;
        
        // Traverse the array from right to left
        for (int i = n - 1; i >= 0; i--) {
            // Remove elements from stack that are greater than or equal to current element
            while (!s.empty() && arr[s.top()] >= arr[i]) {
                s.pop();
            }
            
            // If stack is not empty, top element is the next smaller element
            if (!s.empty()) {
                result[i] = arr[s.top()];
            }
            
            // Push current element's index to stack
            s.push(i);
        }
        
        return result;
    }
};
```

# Method 2 (brute force)
### TC: O(n^2): because in worst case, each element is iterated n times.
### SC: O(n)
```cpp
class Solution{
    
    public:
    vector<int> help_classmate(vector<int> arr, int n) {
        vector<int> result(n, -1);
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[i]) {
                    result[i] = arr[j];
                    break;
                }
            }
        }
        
        return result;
    }
};
```
