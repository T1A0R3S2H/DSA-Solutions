# Method 1 (recursion)
## Code
```cpp
#include <bits/stdc++.h>

void insertSorted(stack<int>& stack, int element) {
    // Base case: if stack is empty or current element is greater than top
    if (stack.empty() || element > stack.top()) {
        stack.push(element);
        return;
    }
    
    // Remove top element
    int top = stack.top();
    stack.pop();
    
    // Recursively insert the element
    insertSorted(stack, element);
    
    // Put back the top element
    stack.push(top);
}

void sortStack(stack<int>& stack) {
    // Base case: if stack is empty
    if (stack.empty()) {
        return;
    }
    
    // Remove the top element
    int top = stack.top();
    stack.pop();
    
    // Sort the remaining stack
    sortStack(stack);
    
    // Insert the top element in sorted position
    insertSorted(stack, top);
}
```
## Dry Run: Recursive Stack Sorting for [-3, 14, 18, -5, 30]

Initial stack: [-3, 14, 18, -5, 30] (top is 30)

## Phase 1: Recursively emptying the stack

1. Call `sortStack([-3, 14, 18, -5, 30])`
   - Pop 30
   - Stack: [-3, 14, 18, -5]
   - Recursive call: `sortStack([-3, 14, 18, -5])`

2. Call `sortStack([-3, 14, 18, -5])`
   - Pop -5
   - Stack: [-3, 14, 18]
   - Recursive call: `sortStack([-3, 14, 18])`

3. Call `sortStack([-3, 14, 18])`
   - Pop 18
   - Stack: [-3, 14]
   - Recursive call: `sortStack([-3, 14])`

4. Call `sortStack([-3, 14])`
   - Pop 14
   - Stack: [-3]
   - Recursive call: `sortStack([-3])`

5. Call `sortStack([-3])`
   - Pop -3
   - Stack: []
   - Recursive call: `sortStack([])`

6. Call `sortStack([])`
   - Base case reached (empty stack)
   - Return

## Phase 2: Inserting elements back in sorted order

7. Return to `sortStack([-3])`
   - Call `insertSorted([], -3)`
     - Base case: push -3
   - Stack: [-3]

8. Return to `sortStack([-3, 14])`
   - Call `insertSorted([-3], 14)`
     - 14 > -3, base case: push 14
   - Stack: [14, -3]

9. Return to `sortStack([-3, 14, 18])`
   - Call `insertSorted([14, -3], 18)`
     - 18 > 14, base case: push 18
   - Stack: [18, 14, -3]

10. Return to `sortStack([-3, 14, 18, -5])`
    - Call `insertSorted([18, 14, -3], -5)`
      - -5 < 18, pop 18
      - -5 < 14, pop 14
      - -5 < -3, pop -3
      - Stack empty, push -5
      - Push -3 back
      - Push 14 back
      - Push 18 back
    - Stack: [18, 14, -3, -5]

11. Return to `sortStack([-3, 14, 18, -5, 30])`
    - Call `insertSorted([18, 14, -3, -5], 30)`
      - 30 > 18, base case: push 30
    - Stack: [30, 18, 14, -3, -5]

Final sorted stack: [30, 18, 14, -3, -5] (top is 30)
