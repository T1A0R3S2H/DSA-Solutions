```cpp
class Solution{
    
    public:
    // Your are required to complete this function
    // function should return root of the modified BST
    vector<int>ans;
    
    void inorder(Node*root)
    {
        if(root)
        {
            inorder(root->left);
            ans.push_back(root->data);
            inorder(root->right);
        }
    }

    Node* buildTree(int s, int e)
    {
        if(s > e) return NULL;
        int mid = s+(e-s)/2;
        Node* root = new Node(ans[mid]);
        root->left = buildTree(s, mid-1);
        root->right = buildTree(mid+1, e);
        return root;
    }
    
    Node* buildBalancedTree(Node* root)
    {
    	inorder(root);
        return buildTree(0, ans.size()-1);
    }

};
```
