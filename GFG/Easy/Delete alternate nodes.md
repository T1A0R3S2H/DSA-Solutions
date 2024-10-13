```cpp
void deleteAlt(struct Node *head) {
        // Code here
        Node* node=head;
        while(node && node->next){
            Node* tmp=node->next->next;
            node->next=tmp;
            node=tmp;
        }
    }
```
