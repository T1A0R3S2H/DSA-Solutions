```cpp
ListNode* convertarr2LL(vector<int>& arr) {
        if (arr.empty()) return nullptr;
        ListNode* head = new ListNode(arr[0]);  // Creating the head node
        ListNode* mover = head;  // Pointer to move along the linked list
        for (size_t i = 1; i < arr.size(); i++) {
            ListNode* temp = new ListNode(arr[i]);  // Creating a new node for each element
            mover->next = temp;  // Linking the current node to the new node
            mover = mover->next;  // Moving to the next node
        }
        return head;  // Returning the head of the linked list
}
```
