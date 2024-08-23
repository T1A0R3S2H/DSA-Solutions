```cpp
void pre(Node* root, vector<int>& res){
        if (root != NULL) {
            res.push_back(root->data);  
            pre(root->left, res);  
            pre(root->right, res);  
        }
    }
    
vector <int> preorder(Node* root)
{
    vector<int> res;
    pre(root, res);
    return res;
}
```
