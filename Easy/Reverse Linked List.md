```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr != nullptr) {
            ListNode* fwd = curr->next; // save the next node
            curr->next = prev;          // reverse the current node's pointer
            prev = curr;                // move prev to the current node
            curr = fwd;                 // move to the next node
        }
        return prev; // prev will be the new head
    }
};
```
