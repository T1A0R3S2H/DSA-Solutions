# Method 1 (Stack)

## Approach:

1. **Initialization**:
   - Determine the length `n` of the input path.
   - Use a stack `st` to store directory components.
   - Use a string `component` to build individual directory names as we traverse the path.

2. **Iterate Through the Path**:
   - Loop through each character in the path:
     - If the character is a slash `'/'`:
       - If `component` is not empty:
         - Check if `component` is `".."`:
           - If so, pop from the stack if it's not empty (move up one directory).
         - Else, if `component` is not `"."`, push it onto the stack.
       - Clear `component` after processing.
     - If the character is not a slash, add it to `component`.

3. **Final Component Check**:
   - After the loop, if `component` is not empty:
     - Process it similarly:
       - If `component` is `".."`, pop from the stack if it's not empty.
       - Else, if `component` is not `"."`, push it onto the stack.

4. **Rebuild the Path**:
   - Initialize an empty string `result`.
   - While the stack is not empty:
     - Prepend each component from the stack to `result` with a slash `'/'`.

5. **Return Result**:
   - If the `result` is empty, return `"/"` (indicating the root directory).
   - Otherwise, return `result`.
  
## Code
```cpp
class Solution {
public:
    string simplifyPath(string path) {
        int n = path.size();
        stack<string> st;
        string component;
        
        // 1st operations for slash
        for (int i = 0; i < n; ++i) {

            // if it is a slash
            if (path[i] == '/') {
                if (!component.empty()) {
                    if (component == "..") {
                        if (!st.empty()) {
                            st.pop();
                        }
                    }

                    // if not a dot
                    else if (component != ".") {
                        st.push(component);
                    }
                    component.clear();
                }
            } 

            // if it is not a slash
            else {
                // concatenation
                component += path[i];
            }
        }
        
        // 2nd operation for dots
        if (!component.empty() && component != ".") {
            if (component == "..") {
                if (!st.empty()) {
                    st.pop();
                }
            } 
            else {
                st.push(component);
            }
        }
        
        string result;
        while (!st.empty()) {
            result = "/" + st.top() + result;
            st.pop();
        }
        
        return result.empty() ? "/" : result;
    }
};

```

## Example:

- **Input**: `"/home//foo/"`
- **Processing**:
  1. Split the path by `'/'`, resulting in components: `["", "home", "", "foo", ""]`.
  2. Process each component:
     - Ignore empty strings and `"."`.
     - Push `"home"` and `"foo"` onto the stack.
  3. Rebuild the path from the stack: `"/home/foo"`.
- **Output**: `"/home/foo"`

## Time Complexity:

- **O(n)**, where `n` is the length of the input path. We traverse each character in the path once.

## Space Complexity:

- **O(n)**, for the stack and the components stored in it. In the worst case, each component is stored separately.
