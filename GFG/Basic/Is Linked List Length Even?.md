### Code

```cpp
class Solution {
  public:
    bool isLengthEven(struct Node **head) {
        int count=0;
        Node*temp=*head;
        while(temp!=NULL){
            temp=temp->next;
            count++;
        }
        return count%2==0;
    }
};
```

### Explanation
1. **Initialize Count**: We start with `count` set to 0. This variable keeps track of the number of nodes in the linked list.
2. **Traversal**: We use `temp` to traverse the list, starting from `*head` (dereferencing `head` as it's a pointer to a pointer).
3. **Counting Nodes**: For each node in the list, we move `temp` to the next node and increment `count`.
4. **Check Even/Odd**: After the loop, we check if `count` is even by calculating `count % 2 == 0`.
5. **Return Result**: If `count` is even, return `true`; otherwise, return `false`.

### Time Complexity
- **O(n)**: We traverse the linked list once, where \( n \) is the number of nodes.

### Space Complexity
- **O(1)**: We use only a fixed amount of extra space for `count` and `temp`.

### Dry Run
Consider a linked list with elements `12 -> 52 -> 10 -> 47 -> 95 -> 0`.

1. **Initialization**: `count = 0`, `temp = *head` (points to the first node with value `12`).
2. **Iteration**:
   - **First Node** (`12`): Move to next, `count = 1`.
   - **Second Node** (`52`): Move to next, `count = 2`.
   - **Third Node** (`10`): Move to next, `count = 3`.
   - **Fourth Node** (`47`): Move to next, `count = 4`.
   - **Fifth Node** (`95`): Move to next, `count = 5`.
   - **Sixth Node** (`0`): Move to next, `count = 6`.
3. **End of List**: `temp` is now `NULL`.
4. **Even Check**: `count % 2 == 0` (6 % 2 == 0), so we return `true`.
