```py
class Solution:
    def calculate(self, s: str) -> int:
        stack, cur, op = [], 0, '+'
        for c in s + '+':
            if c == " ":
                continue
            elif c.isdigit():
                cur = (cur * 10) + int(c)
            else:
                if op == '-':
                    stack.append(-cur)
                elif op == '+':
                    stack.append(cur)
                elif op == '*':
                    stack.append(stack.pop() * cur)
                elif op == '/':
                    stack.append(int(stack.pop() / cur))
                cur, op = 0, c
        return sum(stack)
```

### Explanation:

1. **Initialization**:
   - `stack`: This is used to keep track of numbers and intermediate results.
   - `cur`: This variable builds the current number.
   - `op`: This stores the last operator encountered, initialized to `'+'`.

2. **Iterate through the string**:
   - The loop iterates through each character `c` in the string `s` with an additional `'+'` appended at the end to handle the last number.
   - If the character is a space (`' '`), it is ignored.
   - If the character is a digit, it is added to the `cur` variable to build the current number.
   - If the character is an operator (`'+', '-', '*', '/'`):
     - Depending on the value of `op` (the last operator encountered):
       - If `op` is `'-'`, the current number `cur` (negated) is appended to the stack.
       - If `op` is `'+'`, the current number `cur` is appended to the stack.
       - If `op` is `'*'`, the top number on the stack is popped, multiplied by `cur`, and the result is appended back to the stack.
       - If `op` is `'/'`, the top number on the stack is popped, divided by `cur`, and the result (converted to integer) is appended back to the stack (integer division towards zero).
     - After processing the operator, `cur` is reset to 0, and `op` is updated to the current operator `c`.

3. **Final Result**:
   - The final result is obtained by summing all elements in the stack.

### Dry Run:

Let's dry run the code with the example input `s = "3+2*2"`.

1. Initial state:
   - `stack = []`
   - `cur = 0`
   - `op = '+'`
   - `s = "3+2*2+"` (extra '+' added)

2. First character `c = '3'`:
   - `cur = 3` (since `cur * 10 + int('3')`)

3. Second character `c = '+'`:
   - Since `op = '+'`, append `cur` (which is 3) to the stack: `stack = [3]`
   - Reset `cur = 0`
   - Update `op = '+'`

4. Third character `c = '2'`:
   - `cur = 2` (since `cur * 10 + int('2')`)

5. Fourth character `c = '*'`:
   - Since `op = '+'`, append `cur` (which is 2) to the stack: `stack = [3, 2]`
   - Reset `cur = 0`
   - Update `op = '*'`

6. Fifth character `c = '2'`:
   - `cur = 2` (since `cur * 10 + int('2')`)

7. Sixth character `c = '+'` (extra '+' to handle the last number):
   - Since `op = '*'`, pop the top of the stack, multiply by `cur`, and append the result back to the stack:
     - Pop `2` from the stack: `stack = [3]`
     - Multiply `2` by `cur` (which is 2): `2 * 2 = 4`
     - Append `4` to the stack: `stack = [3, 4]`
   - Reset `cur = 0`
   - Update `op = '+'`

8. Summing up the stack: `sum([3, 4]) = 7`

Final result: `7`

This process correctly follows the order of operations, handling multiplication and division before addition and subtraction, and ensures intermediate results are stored and calculated appropriately.
