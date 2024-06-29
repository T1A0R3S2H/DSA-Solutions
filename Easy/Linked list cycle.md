
# Method 1 (Floyd's Cycle Finding Algorithm (Tortoise and Hare Algorithm))

This code implements the Floyd's Cycle-Finding Algorithm, also known as the "tortoise and hare" algorithm. Here's a more detailed explanation of each part:

1. **Edge Case Handling:**
   - We first check for edge cases. If the list is empty (`head == NULL`) or has only one node (`head->next == NULL`), it can't have a cycle, so we return `false`.

2. **Pointer Initialization:**
   - We initialize two pointers:
     - `slow`: starts at the head of the list.
     - `fast`: starts at the second node of the list.

3. **While Loop:**
   - We enter a while loop that continues as long as `slow` and `fast` are not pointing to the same node.

4. **Inside the Loop:**
   - We first check if `fast` has reached the end of the list. If `fast` is `NULL` or the next node after `fast` is `NULL`, we've reached the end of the list without finding a cycle, so we return `false`.
   - If we haven't reached the end, we move `slow` one step forward and `fast` two steps forward.

5. **Cycle Detection:**
   - If we exit the while loop, it means `slow` and `fast` are pointing to the same node, which only happens if there's a cycle in the list. In this case, we return `true`.

The key idea is that if there's a cycle, the fast pointer will eventually catch up to the slow pointer by going around the cycle. If there's no cycle, the fast pointer will reach the end of the list.

## Code
  ```cpp
  class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (head == NULL || head->next == NULL) {
            return false;
        }
        
        ListNode *slow = head;
        ListNode *fast = head->next;
        
        while (slow != fast) {
            if (fast == NULL || fast->next == NULL) {
                return false;
            }
            slow = slow->next;
            fast = fast->next->next;
        }
        
        return true;
    }
};
```
- **Time Complexity:** O(n), where n is the number of nodes in the list.
- **Space Complexity:** O(1), as it only uses two pointers regardless of the list size.

# Method 2 (unordered map)
```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        unordered_map<ListNode*, bool> visited;
        ListNode* current = head;
        while (current != nullptr) {
            if (visited.count(current)) {
                return true; // cycle detected
            }
            visited[current] = true;
            current = current->next;
        }
        return false; // no cycle
    }
};
```
