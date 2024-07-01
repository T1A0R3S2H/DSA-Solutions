## Method 1
Har ek ke liye check karwa liya kyuki 3 (6) hi possibilities hai in total
##
```bash
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for (char c : s) {
            if (c == '(' || c == '{' || c == '[') {
                st.push(c);
            } 
            
            else {
                if (st.empty() || 
                    (c == ')' && st.top() != '(') || 
                    (c == '}' && st.top() != '{') ||
                    (c == ']' && st.top() != '[')) {
                    return false;
                }
                st.pop();
            }
        }
        return st.empty();
    }
};
```
# Valid Parentheses: Stack-based Solution Explanation

## Algorithm Overview

1. Initialize an empty stack.
2. Iterate through each character in the input string:
   - If it's an opening bracket ('(', '{', '['), push it onto the stack.
   - If it's a closing bracket (')', '}', ']'):
     - If the stack is empty or the top doesn't match, return false.
     - Otherwise, pop the top element from the stack.
3. After processing all characters, return true if the stack is empty, false otherwise.

## Dry Run Examples

### Example 1: "({[]})"

| Step | Character | Action                   | Stack    |
|------|-----------|--------------------------|----------|
| 1    | '('       | Push                     | ['(']    |
| 2    | '{'       | Push                     | ['(', '{']|
| 3    | '['       | Push                     | ['(', '{', '[']|
| 4    | ']'       | Pop (matches)            | ['(', '{']|
| 5    | '}'       | Pop (matches)            | ['(']    |
| 6    | ')'       | Pop (matches)            | []       |

Result: Valid (stack is empty)

### Example 2: "([)]"

| Step | Character | Action                   | Stack    |
|------|-----------|--------------------------|----------|
| 1    | '('       | Push                     | ['(']    |
| 2    | '['       | Push                     | ['(', '[']|
| 3    | ')'       | Invalid (doesn't match)  | -        |

Result: Invalid (closing bracket doesn't match top of stack)

### Example 3: "{[]}"

| Step | Character | Action                   | Stack    |
|------|-----------|--------------------------|----------|
| 1    | '{'       | Push                     | ['{']    |
| 2    | '['       | Push                     | ['{', '[']|
| 3    | ']'       | Pop (matches)            | ['{']    |
| 4    | '}'       | Pop (matches)            | []       |

Result: Valid (stack is empty)

### Example 4: "((("

| Step | Character | Action                   | Stack    |
|------|-----------|--------------------------|----------|
| 1    | '('       | Push                     | ['(']    |
| 2    | '('       | Push                     | ['(', '(']|
| 3    | '('       | Push                     | ['(', '(', '(']|

Result: Invalid (stack is not empty at the end)

### Example 5: ")))"

| Step | Character | Action                   | Stack    |
|------|-----------|--------------------------|----------|
| 1    | ')'       | Invalid (stack empty)    | -        |

Result: Invalid (closing bracket with empty stack)

### Example 6: "{[()]}"

| Step | Character | Action                   | Stack    |
|------|-----------|--------------------------|----------|
| 1    | '{'       | Push                     | ['{']    |
| 2    | '['       | Push                     | ['{', '[']|
| 3    | '('       | Push                     | ['{', '[', '(']|
| 4    | ')'       | Pop (matches)            | ['{', '[']|
| 5    | ']'       | Pop (matches)            | ['{']    |
| 6    | '}'       | Pop (matches)            | []       |

Result: Valid (stack is empty)

## Key Insights

1. The stack maintains the order of opening brackets, allowing for proper matching.
2. The algorithm handles nested brackets correctly.
3. It can quickly identify mismatched or extra brackets.
4. The time complexity is O(n), where n is the length of the input string.
5. The space complexity is O(n) in the worst case (e.g., all opening brackets).
