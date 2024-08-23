```cpp
void post(Node* root, vector<int>& res){
        if (root != NULL) {  
            post(root->left, res);  
            post(root->right, res);
            res.push_back(root->data);
        }
}
vector <int> postOrder(Node* root)
{
        vector<int> res;
        post(root, res);
        return res;
}
```
