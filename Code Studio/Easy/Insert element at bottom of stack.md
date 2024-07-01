# Method 1 (temp stack)

## Code
```cpp
#include <stack>

stack<int> pushAtBottom(stack<int>& myStack, int x) 
{
    // Create a temporary stack
    stack<int> temp;
    
    // Pop all elements from myStack and push to temp
    while (!myStack.empty()) {
        temp.push(myStack.top());
        myStack.pop();
    }
    
    // Push x to the now-empty myStack (effectively at the bottom)
    myStack.push(x);
    
    // Pop all elements from temp back to myStack
    while (!temp.empty()) {
        myStack.push(temp.top());
        temp.pop();
    }
    
    // Return the modified stack
    return myStack;
}
```

## Algorithm Steps

1. Create a temporary stack `tempStack`.
2. While `myStack` is not empty:
   - Pop the top element from `myStack`.
   - Push this element onto `tempStack`.
3. Push the new element `x` onto the now-empty `myStack`.
4. While `tempStack` is not empty:
   - Pop the top element from `tempStack`.
   - Push this element back onto `myStack`.
5. Return the modified `myStack`.

## Time Complexity

The time complexity of this algorithm is O(n), where n is the number of elements in the original stack.

- We iterate through all elements of `myStack` once to move them to `tempStack`: O(n)
- We push the new element to `myStack`: O(1)
- We iterate through all elements of `tempStack` to move them back to `myStack`: O(n)

Total: O(n) + O(1) + O(n) = O(n)

## Space Complexity

The space complexity of this algorithm is O(n), where n is the number of elements in the original stack.

- We use an additional stack `tempStack` which, in the worst case, will store all n elements from the original stack.

Therefore, the extra space used is proportional to the size of the input, resulting in O(n) space complexity.

## Trade-offs

This approach is intuitive and easy to implement. However, it uses an additional data structure (the temporary stack), which may not be optimal if memory is a constraint. In scenarios where we're not allowed to use additional data structures, a recursive approach would be more appropriate.

# Method 2 (recursion)
## Code
```cpp
#include <stack>

void recInsertAtBottom(std::stack<int>& myStack, int x) {
    // Base case: if the stack is empty, insert x
    if (myStack.empty()) {
        myStack.push(x);
        return;
    }
    
    // Remove the top element
    int top = myStack.top();
    myStack.pop();
    
    // Recursively insert x at the bottom
    recInsertAtBottom(myStack, x);
    
    // Push back the top element
    myStack.push(top);
}

std::stack<int> pushAtBottom(std::stack<int>& myStack, int x) 
{
    recInsertAtBottom(myStack, x);
    return myStack;
}
```


## Time Complexity

The time complexity of this recursive solution is O(n), where n is the number of elements in the stack.

- We make n recursive calls, one for each element in the stack.
- Each recursive call performs constant time operations (pushing or popping an element).

Therefore, the total time complexity is O(n).

## Space Complexity

The space complexity is O(n) due to the recursive calls on the call stack.

- In the worst case (when inserting at the very bottom), we will have n recursive calls on the call stack.
- Each recursive call uses a constant amount of extra space (to store the top element).

Thus, the space complexity is O(n).

## Trade-offs

This recursive approach satisfies the constraint of not using any additional data structure. It's elegant and straightforward to implement. However, for very large stacks, it could potentially cause a stack overflow due to the depth of recursion. In such cases, an iterative approach with an explicit stack might be preferable.
