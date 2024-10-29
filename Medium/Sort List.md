# 🔥🔥Method 1
- Here, we are converting the LL to a vector, then sorting the vector, then converting the sorted vector again to a LL.
- But, this approach will take `O(n)` space. If we want to do the problem in `O(1)` space, then we need to do in-place merge sort.
### Code + ETSD

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        ListNode* temp = head;
        vector<int> v;
        while(temp != NULL){
            v.push_back(temp-> val);
            temp = temp-> next;
        }
        sort(v.begin(),v.end());
        ListNode* newHead = new ListNode(0);
        temp = newHead;
        for(int num : v){
            temp-> next = new ListNode(num);
            temp = temp-> next;
        }
        ListNode* result = newHead-> next;
        delete newHead;
        return result;
    }
};
```

### Explanation

1. **Copy values**: Traverse the linked list and copy each node’s value into a vector `v`.
2. **Sort vector**: Use `sort(v.begin(), v.end())` to sort the vector in ascending order.
3. **Rebuild sorted list**: Create a new linked list by iterating through the sorted vector and adding each value as a new node.
4. **Return sorted list**: Return the head of the newly sorted list.

### Time Complexity

- **Copy values**: \(O(n)\) for traversing the list and storing values in the vector.
- **Sort vector**: \(O(n \log n)\) for sorting the vector.
- **Rebuild list**: \(O(n)\) for creating the sorted list from the vector.
- **Total**: \(O(n \log n)\).

### Space Complexity

- **Auxiliary Space**: \(O(n)\) due to storing values in the vector.

### Dry Run
Here's a detailed dry run of the code for the input `head = [-1, 5, 3, 4, 0]`.

### Initial Setup
- Input linked list: `head = [-1 -> 5 -> 3 -> 4 -> 0]`.

### Step-by-Step Execution

#### Step 1: Copy Values into Vector
1. Initialize `temp = head`, `v = []`.
2. Traverse the linked list, appending each node's value to the vector `v`:
   - `temp = head` (value `-1`): `v = [-1]`
   - Move `temp` to the next node.
   - `temp` now points to node with value `5`: `v = [-1, 5]`
   - Move `temp` to the next node.
   - `temp` now points to node with value `3`: `v = [-1, 5, 3]`
   - Move `temp` to the next node.
   - `temp` now points to node with value `4`: `v = [-1, 5, 3, 4]`
   - Move `temp` to the next node.
   - `temp` now points to node with value `0`: `v = [-1, 5, 3, 4, 0]`
   - `temp` reaches the end of the list (`temp = NULL`), stopping the traversal.

At this point, the vector `v` contains all node values: `v = [-1, 5, 3, 4, 0]`.

#### Step 2: Sort the Vector
- Sort `v` to arrange values in ascending order.
- `v = [-1, 0, 3, 4, 5]`

#### Step 3: Create a New Linked List from the Sorted Vector
1. Create a dummy node `newHead = ListNode(0)` to serve as the starting point.
2. Set `temp = newHead` to start building the new list.
3. Iterate over the sorted vector `v` and add each element as a new node in the linked list:
   - For `num = -1`:
     - Create a new node `ListNode(-1)` and set `temp->next` to this new node.
     - Move `temp` to this new node.
   - For `num = 0`:
     - Create a new node `ListNode(0)` and set `temp->next` to this new node.
     - Move `temp` to this new node.
   - For `num = 3`:
     - Create a new node `ListNode(3)` and set `temp->next` to this new node.
     - Move `temp` to this new node.
   - For `num = 4`:
     - Create a new node `ListNode(4)` and set `temp->next` to this new node.
     - Move `temp` to this new node.
   - For `num = 5`:
     - Create a new node `ListNode(5)` and set `temp->next` to this new node.
     - Move `temp` to this new node.

The linked list now looks like: `newHead -> -1 -> 0 -> 3 -> 4 -> 5`.

4. Set `result = newHead->next` to remove the dummy head node and point to the start of the sorted list.

#### Step 4: Return the Sorted List
- Return `result`, which points to the head of the sorted linked list.

### Final Sorted Linked List
The output linked list is: `[-1 -> 0 -> 3 -> 4 -> 5]`, matching the expected result.



# 🔥🔥Method 2 (O(1) space)
To achieve \(O(n \log n)\) time complexity and \(O(1)\) space, we can implement an in-place merge sort directly on the linked list. Here’s the step-by-step solution:

1. **Split the List**: Recursively divide the list into two halves until each half has a single node.
2. **Merge**: Use the merge operation to merge two sorted lists back together.

This approach uses recursion to split the list but does not require extra space for sorting.

### Code + ETSD

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    // Helper function to split the list into two halves and return the second half.
    ListNode* getMid(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head->next;
        
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        
        ListNode* mid = slow->next;
        slow->next = nullptr;
        return mid;
    }
    
    // Merge two sorted linked lists into one sorted list.
    ListNode* merge(ListNode* left, ListNode* right) {
        ListNode* dummy = new ListNode(0);
        ListNode* tail = dummy;
        
        while (left && right) {
            if (left->val < right->val) {
                tail->next = left;
                left = left->next;
            } else {
                tail->next = right;
                right = right->next;
            }
            tail = tail->next;
        }
        
        tail->next = (left) ? left : right;
        return dummy->next;
    }
    
    // Main sort function using merge sort.
    ListNode* sortList(ListNode* head) {
        if (!head || !head->next) return head;
        
        ListNode* mid = getMid(head);
        ListNode* left = sortList(head);
        ListNode* right = sortList(mid);
        
        return merge(left, right);
    }
};
```

### Explanation

1. **getMid Function**:
   - Finds the middle of the list using the slow-and-fast pointer approach.
   - `slow` moves one step, while `fast` moves two steps. When `fast` reaches the end, `slow` is at the midpoint.
   - Split the list at `slow`, making `slow->next = nullptr` to separate the two halves.

2. **merge Function**:
   - Merges two sorted linked lists `left` and `right` into one sorted list.
   - Uses a dummy node to make it easy to build the merged list.
   - Iterates through both lists and attaches the smaller node to `tail`.
   - At the end, attaches any remaining nodes from either `left` or `right`.

3. **sortList Function**:
   - Recursively splits the list until it reaches the base case (single-node or empty list).
   - Calls `merge` to combine sorted halves and returns the sorted list.

### Time Complexity

- **Overall Time Complexity**: \(O(n \log n)\) because the list is split into halves recursively and each merge operation takes \(O(n)\) time.

### Space Complexity

- **Auxiliary Space**: \(O(1)\) as we are sorting the list in place without using extra space for data storage.
- **Recursion Stack Space**: The recursion depth is \(O(\log n)\), but it does not count towards auxiliary space in this context.

### Dry Run

For `head = [-1, 5, 3, 4, 0]`:

1. **Split Phase**:
   - Split `[-1, 5, 3, 4, 0]` into `[-1, 5, 3]` and `[4, 0]`.
   - Split `[-1, 5, 3]` into `[-1]` and `[5, 3]`.
   - Split `[5, 3]` into `[5]` and `[3]`.
   - Split `[4, 0]` into `[4]` and `[0]`.

2. **Merge Phase**:
   - Merge `[5]` and `[3]` to get `[3, 5]`.
   - Merge `[-1]` and `[3, 5]` to get `[-1, 3, 5]`.
   - Merge `[4]` and `[0]` to get `[0, 4]`.
   - Merge `[-1, 3, 5]` and `[0, 4]` to get `[-1, 0, 3, 4, 5]`.

The final output list is `[-1, 0, 3, 4, 5]`.
