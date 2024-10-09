![image](https://github.com/user-attachments/assets/1dbab4af-a1df-41e5-8c01-d710a884176f)
## Code:
```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        int open=0;
        int close=0;
        for (char c:s) {
            if (c=='(') {
                open++;
            } 
            else {  // c == ')'
                if (open==0) {
                    close++;
                } 
                else {
                    open--;
                }
            }
        }
        return open+close;
    }
};
```

## Explanation:
This solution uses two counters:
1. `open`: Counts the number of unmatched opening parentheses '('.
2. `close`: Counts the number of unmatched closing parentheses ')'.

The algorithm works as follows:
1. Iterate through each character in the string.
2. If it's an opening parenthesis '(', increment `open`.
3. If it's a closing parenthesis ')':
   - If there are no unmatched opening parentheses (`open == 0`), increment `close`.
   - Otherwise, decrement `open` to match the current closing parenthesis with an opening one.
4. After processing all characters, return the sum of `open` and `close`.

## Time Complexity: O(n), where n is the length of the input string.
- We iterate through each character in the string exactly once.

## Space Complexity: O(1)
- We only use two integer variables (`open` and `close`) regardless of the input size.

## Dry Run:
Let's use the example: s = "())(()"



| Character | open | close | Explanation |
|-----------|------|-------|-------------|
| Initial   | 0    | 0     |             |
| '('       | 1    | 0     | Increment open |
| ')'       | 0    | 0     | Decrement open |
| ')'       | 0    | 1     | Increment close (no unmatched opening parenthesis) |
| '('       | 1    | 1     | Increment open |
| '('       | 2    | 1     | Increment open |
| ')'       | 1    | 1     | Decrement open |

Final result: open + close = 1 + 1 = 2

This means we need to add 2 parentheses to make the string valid:
- One closing parenthesis ')' to match the last unmatched '('
- One opening parenthesis '(' at the beginning to match the unmatched ')'

The corrected string could be: "(())())"

This solution efficiently handles both types of unmatched parentheses in a single pass through the string, making it both time and space efficient. The use of `open` and `close` as variable names makes the logic more intuitive and easier to understand.
