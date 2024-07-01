# Method 1 (temp Stack)
```cpp
#include <bits/stdc++.h>
void deleteMiddle(stack<int> &inputStack, int N) {
  int size = inputStack.size();
  int middle;
  
  if (size % 2 == 0) {
    middle = size / 2 - 1;
  } else {
    middle = size / 2;
  }

  stack<int> tempStack;
  
  // Remove elements until we reach the middle
  for (int i = 0; i < middle; i++) {
    tempStack.push(inputStack.top());
    inputStack.pop();
  }
  
  // Remove the middle element
  inputStack.pop();
  
  // Put back the elements from tempStack
  while (!tempStack.empty()) {
    inputStack.push(tempStack.top());
    tempStack.pop();
  }
}
```


# Dry Run

## Example with Odd Number of Elements

Let's dry run this code with an example: `[1, 2, 3, 4, 5]` (odd number of elements)

1. Initially:
   * `inputStack = [1, 2, 3, 4, 5]` (top is 5)
   * `size = 5`
   * `middle = 5 / 2 = 2` (odd case)

2. Moving elements to tempStack:
   * After first iteration: `inputStack = [1, 2, 3, 4]`, `tempStack = [5]`
   * After second iteration: `inputStack = [1, 2, 3]`, `tempStack = [5, 4]`

3. Removing middle element:
   * `inputStack.pop()` removes 3
   * Now: `inputStack = [1, 2]`, `tempStack = [5, 4]`

4. Moving elements back to inputStack:
   * First iteration: `inputStack = [1, 2, 4]`, `tempStack = [5]`
   * Second iteration: `inputStack = [1, 2, 4, 5]`, `tempStack = []` (empty)

Final result: `inputStack = [1, 2, 4, 5]`

The middle element (3) has been successfully removed.

## Example with Even Number of Elements

Let's consider `[1, 2, 3, 4]` (even number of elements)

1. Initially:
   * `inputStack = [1, 2, 3, 4]` (top is 4)
   * `size = 4`
   * `middle = 4 / 2 - 1 = 1` (even case)

2. Moving elements to tempStack:
   * After first iteration: `inputStack = [1, 2, 3]`, `tempStack = [4]`

3. Removing middle element:
   * `inputStack.pop()` removes 3
   * Now: `inputStack = [1, 2]`, `tempStack = [4]`

4. Moving elements back to inputStack:
   * `inputStack = [1, 2, 4]`, `tempStack = []` (empty)

Final result: `inputStack = [1, 2, 4]`

The element just before the center (3) has been successfully removed.


# Method 2 (recursion)
```cpp

void solve(stack<int>&inputStack, int count, int size) {
    //base case
    if(count == size/2) {
        inputStack.pop();
        return ;
    }
    
    int num = inputStack.top();
    inputStack.pop();
    
	//RECURSIVE CALL
    solve(inputStack, count+1, size);
    
    inputStack.push(num);
    
}

void deleteMiddle(stack<int>&inputStack, int N){
	
  	int count = 0;
    solve(inputStack, count, N);
   
}
```
