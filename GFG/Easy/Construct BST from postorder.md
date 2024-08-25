```cpp
Node* solve(Node* root, int data) {
    if(root==NULL){
        return new Node(data);
    }
    if(data > root->data) {
        root->right = solve(root->right, data);
    }
    else {
        root->left = solve(root->left, data);
    }
    return root;
}

Node *constructTree (int post[], int size)
{
   Node* root = NULL;
   for(int i=size-1;i>=0;i--) {
       root = solve(root,post[i]);
   }
   return root;
}
```
