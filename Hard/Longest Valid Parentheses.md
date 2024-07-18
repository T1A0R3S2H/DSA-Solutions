The provided `longestValidParentheses` function calculates the length of the longest valid parentheses substring in a given string `s`. It uses a stack to track the indices of the parentheses, starting with an initial value of `-1` to handle edge cases. As it iterates through the string, it pushes indices of `'('` onto the stack. When encountering `')'`, it pops the stack and checks if it becomes empty; if so, it pushes the current index to serve as a new base for future valid substrings. If the stack is not empty after a pop, it updates the maximum length `maxi` with the difference between the current index and the new top of the stack. The time complexity is O(n) as it processes each character of the string once, and the space complexity is O(n) due to the stack potentially holding all indices in the worst case.
```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> st;
        st.push(-1); //pehle se default me kuch bhi number rakhdo
        int maxi=0;

        for (int i=0;i<s.length();i++) {
            if (s[i]=='(') {
                st.push(i);
            }
            else {
                st.pop();
                if (st.empty()) {
                    st.push(i);
                } 
                else {
                    maxi=max(maxi, i-st.top());
                }
            }
        }

        return maxi;        
    }
};
```

Let's do a dry run of the `longestValidParentheses` function with the input `")())"`.

### Initialization
- `st` (stack) is initialized with `-1`: `st = [-1]`
- `maxi` is initialized to `0`: `maxi = 0`

### Iteration through the string
We'll iterate through each character of the string and update the stack and `maxi` accordingly.

#### Step-by-Step Execution

1. **i = 0, s[i] = ')'**
   - Since `s[i]` is `')'`, we pop from the stack.
   - Stack after popping: `st = []`
   - Stack is empty after popping, so we push the current index `i` onto the stack.
   - Stack after pushing: `st = [0]`
   - `maxi` remains `0`.

2. **i = 1, s[i] = '('**
   - Since `s[i]` is `'('`, we push the current index `i` onto the stack.
   - Stack after pushing: `st = [0, 1]`
   - `maxi` remains `0`.

3. **i = 2, s[i] = ')'**
   - Since `s[i]` is `')'`, we pop from the stack.
   - Stack after popping: `st = [0]`
   - Stack is not empty, so we update `maxi` with the difference between the current index `i` and the new top of the stack.
   - `maxi = max(0, 2 - 0) = 2`
   - Stack remains: `st = [0]`

4. **i = 3, s[i] = ')'**
   - Since `s[i]` is `')'`, we pop from the stack.
   - Stack after popping: `st = []`
   - Stack is empty after popping, so we push the current index `i` onto the stack.
   - Stack after pushing: `st = [3]`
   - `maxi` remains `2`.

### Final Result
After iterating through the entire string, the maximum length of the valid parentheses substring is stored in `maxi`.

Thus, the function returns `2`.

### Summary of Stack and `maxi` at each step:
- `i = 0`: `st = [0]`, `maxi = 0`
- `i = 1`: `st = [0, 1]`, `maxi = 0`
- `i = 2`: `st = [0]`, `maxi = 2`
- `i = 3`: `st = [3]`, `maxi = 2`

The longest valid parentheses substring in the given input `")())"` is of length `2`.




# We can use deque also instead of a stack
```cpp
#include <deque>
#include <string>
#include <algorithm>

class Solution {
public:
    int longestValidParentheses(std::string s) {
        std::deque<int> dq;
        dq.push_back(-1); // Initial base value
        int maxi = 0;

        for (int i = 0; i < s.length(); ++i) {
            if (s[i] == '(') {
                dq.push_back(i);
            } else {
                dq.pop_back();
                if (dq.empty()) {
                    dq.push_back(i);
                } else {
                    maxi = std::max(maxi, i - dq.back());
                }
            }
        }

        return maxi;
    }
};
```
