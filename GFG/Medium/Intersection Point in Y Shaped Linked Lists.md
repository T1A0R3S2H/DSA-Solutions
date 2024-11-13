### Code

```cpp
class Solution {
  public:
    // Function to find intersection point in Y shaped Linked Lists.
    int intersectPoint(Node* head1, Node* head2) {
        unordered_set<Node*>st;
        while(head1!=NULL){
            st.insert(head1);
            head1=head1->next;
        }
        
        // check if any node in 2nd list is in the set
        while(head2!=NULL){
            if(st.find(head2)!=st.end()){
                return head2->data;
            }
            head2=head2->next;
        }
        return -1;
    }
};
```

### Explanation
In this problem, we need to find the intersection point of two singly linked lists that merge at some node. The solution provided here follows a straightforward approach using a hash set to store nodes of the first linked list and then checking each node in the second linked list to see if it exists in the set.

**Steps:**
1. Traverse the first linked list, storing each node's address in an unordered set.
2. Traverse the second linked list and check if each node's address is present in the set.
3. If a node from the second list is found in the set, it means this node is the intersection point, so we return its data.
4. If no intersection is found after traversing both lists, return `-1`.

This approach ensures that we find the first common node where the two lists merge, which is the intersection point.

### Why the Solution Using Vectors Doesn't Work
A solution where we store each list in a separate vector and compare nodes will fail because:
1. **Reference Loss**: When the nodes of the two linked lists are stored in two vectors, we lose the ability to efficiently compare the memory addresses of nodes. The order and position in the vectors might not directly help in determining the intersection.
2. **Space Complexity Issue**: Using vectors would increase space complexity unnecessarily, as we are storing all nodes, instead of directly comparing memory references, leading to inefficiency.

### Time Complexity
- **O(m + n)**, where \( m \) and \( n \) are the lengths of the two linked lists. We traverse each list once to populate the set and then to check for intersections.

### Space Complexity
- **O(m)**, where \( m \) is the number of nodes in the first linked list due to the hash set storage.

### Dry Run
Consider the input:

- Linked list 1: `4 -> 1 -> 8 -> 4 -> 5`
- Linked list 2: `5 -> 6 -> 1 -> 8 -> 4 -> 5`

1. **First Pass (Storing Nodes from List 1)**:
   - Insert nodes from list 1 into the set: `4`, `1`, `8`, `4`, `5`.

2. **Second Pass (Checking Nodes from List 2)**:
   - Check each node in list 2. Nodes `5` and `6` are not in the set, but node `1` is found in the set.
   - Return `8` as the data for the first intersecting node.

This confirms that node with data `8` is the intersection point.

### Why vectors wali approach fails?
The approach of using vectors to store the nodes of both linked lists and then finding the intersection is problematic for the following reasons:

1. **Loss of Address Comparison**: The intersection in linked lists happens because two lists share a common node **by memory address**, not by value. When you store nodes in vectors, you are only storing their values (or their data), not their memory references. So, even if two nodes in the vector have the same data value, they may not actually be the same node in memory, which is what determines the intersection point.

2. **Incorrect Alignment of Nodes**: Linked lists can have different lengths, so their nodes may not align in vectors the way they do in memory. For instance, if list 1 has nodes `A -> B -> C` and list 2 has nodes `X -> Y -> B -> C`, their vector representation would look like:
   - List 1 in vector: `[A, B, C]`
   - List 2 in vector: `[X, Y, B, C]`

   Comparing nodes from the start would fail to reveal the intersection at `B` because the positions in the vectors do not represent the true intersection point in the lists. We would need to start comparing nodes from specific offsets, making it complicated and inefficient.

3. **Increased Space Complexity**: Storing all nodes of both lists in vectors would increase the space complexity to **O(m + n)**, where \( m \) and \( n \) are the lengths of the lists. This is unnecessary, as the intersection can be detected by just comparing nodes directly in the original lists.

4. **Inefficiency in Finding the Intersection**: Even if you were to align the vectors correctly, iterating through both vectors to find the first common node by data comparison (assuming we track addresses somehow) would make the solution inefficient. The hash set approach or pointer manipulation methods allow a direct comparison based on node addresses, leading to an **O(m + n)** solution without the overhead of storing all nodes.

In summary, the vector approach is both inefficient and ineffective because it loses the direct address comparison needed to detect where the two linked lists actually intersect in memory.
