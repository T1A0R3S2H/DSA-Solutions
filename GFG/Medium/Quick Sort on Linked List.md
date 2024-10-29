```cpp
#include <algorithm>
// Solution class with quickSort function
class Solution {
  public:
    struct Node* quickSort(struct Node* head) {
    struct Node* temp = head;
    vector<int> v;

    while(temp != NULL) {
        v.push_back(temp->data);
        temp = temp->next;
    }

    sort(v.begin(), v.end());

    struct Node* newHead = new Node(0);
    temp = newHead;
    for(int num : v) {
        temp->next = new Node(num);
        temp = temp->next;
    }

    struct Node* result = newHead->next;
    delete newHead;
    return result;
}
```
