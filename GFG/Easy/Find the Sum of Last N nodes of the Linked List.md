### Code

```cpp
int sumOfLastN_Nodes(struct Node* head, int n) {
    struct Node* temp = head;
    int total_nodes = 0, sum = 0;

    // Step 1: Count the total number of nodes
    while (temp != NULL) {
        total_nodes++;
        temp = temp->next;
    }

    // Step 2: Traverse again to calculate the sum of last n nodes
    temp = head;
    int skip = total_nodes - n;
    while (skip > 0) {
        temp = temp->next;
        skip--;
    }

    // Step 3: Sum the remaining nodes
    while (temp != NULL) {
        sum += temp->data;
        temp = temp->next;
    }

    return sum;
}
```

### Explanation:
1. **Step 1**: Traverse the linked list to count the total number of nodes.
2. **Step 2**: Reset the pointer `temp` to the head and calculate how many nodes to skip before reaching the last `n` nodes.
3. **Step 3**: Once we reach the last `n` nodes, sum their values and return the sum.

### Time Complexity:
- **O(2M)**, where `M` is the number of nodes. The list is traversed twice: once to count the nodes and once to calculate the sum.

### Space Complexity:
- **O(1)**, as we are using a constant amount of space for variables like `temp`, `total_nodes`, `skip`, and `sum`.

### Dry Run:
- **Input**: Linked List = `5 -> 9 -> 6 -> 3 -> 4 -> 10`, `n = 3`
  - **Step 1**: Count nodes: 6 nodes in total.
  - **Step 2**: Skip 3 nodes (`6 - 3 = 3`), move to node with value `3`.
  - **Step 3**: Sum nodes from this point: `3 + 4 + 10 = 17`.
- **Output**: 17.
